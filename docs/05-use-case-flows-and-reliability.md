# 05 — Use Case Flows & Reliable Outcomes

> 공개 가능한 합성 결제 예제로 Use Case의 경로와 실제 시스템 상태를 연결한다. 특정 서비스의 설계나 구현을 재현하지 않는다.

## Main / Alternative / Exception Flow

- **Main Success Scenario:** Actor의 목표를 달성하는 대표적인 정상 경로.
- **Alternative Flow:** 다른 유효 경로로 진행하는 분기. 목표 달성 여부는 분기 종료 상태에 따라 달라질 수 있다.
- **Exception Flow:** 오류·예외 상황에 대응하는 분기. 복구 후 목표를 달성할 수도, 실패 종료할 수도 있다.
- 분류 방식은 명세 기법마다 다르다. 모든 분기를 **Extensions**로 표현하기도 한다.

```text
Payment Use Case
├─ Main: 카드 결제 정상 완료 → Goal 달성
├─ Alternative: 다른 결제 수단 선택 → Goal 달성 가능
└─ Exception: 통신 타임아웃
   ├─ 상태 확인 및 안전한 복구 → Goal 달성 가능
   └─ 처리 실패 종료 → Goal 미달성
```

> **Exception Occurrence ≠ Use Case Failure.**

경로에서 예외가 발생했다는 사실과 최종 성공 여부는 별도로 평가한다.

## Goal / Postcondition / Invariant

결제의 Goal은 단순히 POST를 보내는 것이 아니라 **구매를 유효하게 완료하는 것**이다.

| 개념 | 확인할 내용 | 합성 예시 |
| --- | --- | --- |
| Actor Action | 어떤 상호작용을 수행하는가? | 결제 버튼을 누른다 |
| Success Postcondition | 성공 종료 후 무엇을 보장하는가? | 주문에 연결된 결제가 완료된다 |
| Failure Postcondition | 실패 종료 후 무엇을 보장하는가? | 실패한 재시도로 추가 청구가 발생하지 않는다 |
| Invariant | 실행 중을 포함해 무엇을 위반해서는 안 되는가? | 동일한 논리적 결제 시도로 중복 청구하지 않는다 |

불변조건은 성공 시점뿐 아니라 실패·재시도·동시 처리에서도 지켜져야 한다. 이 제약은 Functional Requirement와도 연결될 수 있지만 개념상의 관점은 다르다.

## Timeout → Unknown: 실제 상태와 관측 상태 구분

```text
Client                  Server
  ├── POST /payments ──>│
  │                     ├─ 결제 처리 / 상태 저장
  │      응답 유실 X <──┤
  └─ Timeout → 결과 미확인
```

- **Timeout은 응답을 제시간에 받지 못했다는 관측**이다. 결제가 처리되지 않았다는 증거가 아니다.
- 서버의 실제 결제 처리 상태가 성공이더라도 클라이언트가 아는 상태는 **UNKNOWN**일 수 있다.
- TCP ACK(전송 계층), HTTP 성공 상태 코드(응용 계층 응답), 비즈니스 성공(실제 거래 결과)은 서로 다르다.

> **Actual State ≠ Observed State ≠ Known State.**

## HTTP Stateless ≠ Application Has No State

HTTP는 이전 요청에 대한 애플리케이션 상태를 프로토콜 자체가 자동으로 관리하지 않는다. 하지만 서버는 DB 같은 저장소에서 거래·주문·처리 기록을 명시적으로 저장하고 다음 요청에서 조회할 수 있다.

- POST는 기본적으로 멱등성을 보장하지 않는다.
- 재시도 POST는 별도의 애플리케이션 요청이며 TCP가 두 요청의 비즈니스 중복을 판단하지 않는다.
- 클라이언트의 `isPaid` 값은 UI용으로 유용하지만, 중복 결제 방지의 최종 근거가 될 수 없다.
- 서버가 소유하는 영속 거래 상태를 기준으로 판단해야 한다. 외부 결제기관의 최종 상태와 차이가 생길 가능성도 고려한다.

## Idempotency: 보장할 성질부터 정의한다

동일한 논리적 요청에 대한 재시도가 새로운 결제 효과를 일으키지 않도록 설계한다.

```text
POST with Idempotency-Key: k1
    ↓
서버 측 거래 기록 확인
    ├─ 새 키       → 원자적으로 처리 소유권 확보 → 실행
    ├─ PROCESSING  → 별도 결제 시작 없이 처리 중 안내/대기
    ├─ SUCCEEDED   → 기존 처리 결과 제공
    └─ FAILED      → 사전에 정의한 실패·재시도 정책 적용
```

주의할 사항:

1. 키뿐 아니라 **요청의 주체·범위와 내용**도 검증한다. 같은 키에 서로 다른 결제 내용을 매핑하면 안 된다.
2. `if (keyMatches || status == PROCESSING)`는 다른 거래까지 거절할 수 있다. 먼저 **키에 해당하는 기록**을 찾고 그 기록의 상태로 분기한다.
3. **Short-circuit evaluation**은 조건식 평가 방식이지 중복 요청에 대한 원자적 보장이 아니다.
4. 동시에 도착한 요청이 모두 키가 없다고 판단하는 **Race Condition**을 방지해야 한다. 유일성 제약·트랜잭션 등으로 처리 권한을 확보하되, 이것만으로 외부 결제의 정확히 한 번 실행이 자동 보장되지는 않는다.
5. Waiting Queue는 선택 가능한 조정 메커니즘이지 멱등성 자체가 아니다.

> **First define the guarantee, then choose the mechanism.**

## Command vs Query / PRG / SSE

| 메커니즘 | 책임 |
| --- | --- |
| `POST /payments` | 결제 실행(Command) |
| `GET /payments/{paymentId}` | 기존 결제 상태 확인(Query) |
| PRG (`POST → 303 → GET`) | 브라우저 새로고침의 의도치 않은 POST 재전송 감소 |
| Idempotency Key | 동일한 논리적 요청의 중복 효과 방지 |
| SSE | 상태 변화를 실시간 통지 |
| Persisted State | 연결 유실 이후에도 현재 거래 상태를 확인할 기준 |

**PRG 한계:** POST 후 303 응답 자체를 수신하지 못하면 브라우저는 GET으로 이동하지 못한다. 별도의 복구 경로가 필요하다.

**식별자의 역할:** `orderId`는 주문, `paymentId`는 결제 리소스, `Idempotency-Key`는 논리적 요청의 재시도를 식별한다. 서로의 역할을 혼동하지 않는다. POST 응답이 유실되어 `paymentId`를 받지 못했다면, 인증·인가된 `orderId` 기반 상태 조회처럼 미리 합의한 복구 API를 사용할 수 있다.

**SSE 한계:** 실시간 알림을 놓쳤다고 영속 상태가 사라지는 것은 아니다. SSE 재연결만으로 과거 이벤트 재생을 보장하지 않는다. `Last-Event-ID` 추적은 재생 설계의 일부일 뿐, 서버 측 이벤트 이력과 재전송 구현이 필요하다. 상태 조회와 구독 사이의 경합도 고려한다.

```text
페이지 복귀 / SSE 재연결
          ↓
인증된 GET 상태 조회
          ├─ SUCCEEDED → 완료 UI 복원
          ├─ PROCESSING → 진행 안내 및 갱신 구독
          └─ 실패/불확실 → 확인·복구 정책으로 전환
```

> **Persistent State is the Source of Truth. SSE is a Notification Channel.**

외부 결제기관이 실제 거래를 최종적으로 관리하는 경우, 로컬 DB는 그 상태와 대조·동기화해야 할 수 있다.

## Connecting the Dots

```text
Use Case (Actor의 Goal)
  ↓
Success / Failure Postcondition
  ↓
Invariant (위반해서는 안 되는 제약)
  ↓
Responsibility (누가 보장하는가)
  ↓
State Ownership & Persistence
  ↓
Decision (Idempotency / Retry / Query / SSE)
  ↓
Evidence (실제 거래 상태를 어떻게 확인하는가)
```

## 다음 복습 후 풀 문제 — 미해결

> 결제사에서는 실제 결제가 성공했지만 서버가 로컬 DB에 `SUCCEEDED`를 반영하기 직전에 종료되었다. 재시작 후 로컬 DB에는 `PROCESSING`만 남아 있다. 서버는 **어디에서 실제 상태를 확인**하고, **어떤 순서로 복구**해야 중복 청구를 방지할 수 있을까?

이 문제의 해결 과정은 다음 학습에서 질문과 근거를 통해 도출한다. 이 문서에는 아직 정답을 기록하지 않는다.
