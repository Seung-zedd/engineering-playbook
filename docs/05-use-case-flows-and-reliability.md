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


### 하나의 Use Case를 구성하는 Flow

하나의 Use Case 명세에는 대표 정상 경로와 필요한 분기 경로를 함께 기술할 수 있다.

- **Main Success Scenario:** Actor의 Goal을 달성하는 대표적인 정상 경로.
- **Alternative Flow:** 정상적으로 예상 가능한 다른 유효 경로. Actor의 Goal을 달성하지 못하더라도 시스템이 책임을 정상 수행한 결과일 수 있다(예: 검색 결과 0건을 정확히 안내).
- **Exception Flow:** 정상적인 진행을 방해하는 예외 상황에 대응하는 경로. 복구 후 Goal을 달성할 수도 있고, 실패 종료할 수도 있다.

따라서 `Actor Goal Failure ≠ Exception Flow`이며, `Exception Occurrence ≠ Use Case Failure`다. Goal 달성 여부와 Flow 유형은 별도로 판단한다. 분류 관행은 명세 기법에 따라 다를 수 있으며, 모든 Use Case에 Alternative/Exception Flow가 반드시 존재하는 것은 아니다.

| 상황 | Flow 분류 예시 | 시스템 책임 |
| --- | --- | --- |
| 검색 결과를 정상적으로 제공 | Main Success | 정상 이행 |
| 검색 결과 0건을 정확히 안내 | Alternative | 정상 이행 |
| 모호한 질의에 추가 조건 요청 | Alternative | 정상 이행 |
| 검색 서비스 장애 발생 | Exception | 예외 처리 필요 |
| 장애 복구 후 검색 결과 제공 | Exception 후 복구 | 최종 Goal 달성 가능 |

### Use Case 간 관계: `«include»`, `«extend»`, Generalization

Flow는 **하나의 Use Case 내부 진행 경로**를 설명하고, Relationship은 **서로 다른 Use Case 사이의 구조적 관계**를 설명한다. `include`와 `extend`는 Alternative/Exception Flow의 다른 이름이 아니다.

| 관계 | 핵심 의미 | 판단 기준 |
| --- | --- | --- |
| `«include»` | 기본 Use Case가 다른 Use Case의 행위를 필수적으로 포함 | 매번 수행되는 공통 행위인가? |
| `«extend»` | 조건에 따라 독립적인 기본 Use Case에 추가 행위를 확장 | 특정 조건/선택에서만 추가되는 행위인가? |
| Generalization | 특수 Use Case가 일반 Use Case의 공통 특성을 상속·구체화 | 공통 목표·행위의 일반형과 구체형 관계인가? |

**`«include»`와 Precondition은 다르다.**

- **Precondition:** Use Case를 시작하기 전에 참이어야 하는 조건. 예: 개인화 검색을 시작할 때 사용자가 이미 인증된 상태여야 한다.
- **`«include»`:** Use Case 수행 과정에서 매번 포함되는 별도의 행위. 예: 매번 인증 절차 자체를 수행하는 것이 해당 Use Case의 필수 행위라면 인증 Use Case를 포함할 수 있다.

즉, 필수성이라는 공통점만으로 둘을 동일시하면 안 된다. 이미 충족되어 있어야 하는 조건인지, 실행 중 수행해야 하는 행위인지 구분한다.

**관계 방향(UML 점선 화살표):**

- `include`: 포함하는 Base Use Case → 포함되는 Use Case
- `extend`: 확장하는 Extension Use Case → 확장 대상 Base Use Case
- Generalization: 특수 Use Case → 일반 Use Case (속이 빈 삼각형이 일반 Use Case를 향함)

**예시와 판단:**

- 관광객이 검색 결과 번역을 선택적으로 요청한다면, 번역은 검색 Use Case를 조건부로 확장하는 `«extend»` 후보가 된다.
- 관광지 검색과 축제 검색이 공통적인 관광 콘텐츠 검색 목표·행위를 구체화한다면 Generalization을 검토할 수 있다.
- 단순히 내부 처리 단계라는 이유만으로 별도 Use Case를 만들거나 `«include»`를 남발하지 않는다. 별도 Use Case는 Actor-System 상호작용으로 의미 있게 식별되는 행위인지 검토한다.

> **Include = 필수 행위 재사용. Extend = 조건부 행위 확장. Generalization = 공통 행위의 특수화.**

#### 다음 세션 질문 — 미해결

관광객이 관광 정보를 검색할 때 시스템은 반드시 검색 결과에 출처 정보를 함께 제공해야 한다.

이때 출처 제공을 `«include»` 관계로 모델링하는 것이 적절한가? 판단할 때는 **필수성뿐 아니라 출처 제공이 독립적인 Actor-System 상호작용으로 정의될 만한 별도 Use Case인지**도 검토한다.

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

## Connecting the Dots — DB 중복 방지와 트랜잭션 (심화 참고)

이 절은 Use Case의 핵심 명세가 아니라, **중복 요청을 방지한다는 보장을 DB가 어떻게 뒷받침하는지** 탐색하며 확인한 일반 CS 원칙이다.

### UNIQUE 제약의 Scope

- `UNIQUE(idem_key, order_id)`는 같은 키라도 **다른 order_id라면 허용**한다.
- 같은 논리적 요청에 하나의 키를 묶는 정책이라면, 예를 들어 `UNIQUE(user_id, idem_key)`처럼 **키의 유효 범위**에 맞게 제약을 둔다.
- 키가 같더라도 요청 본문(주문·금액·통화 등)이 다르면 단순 재시도가 아닌 **키 재사용 충돌**로 다뤄야 한다.
- 별개 키로 같은 주문을 여러 번 청구하지 못하게 하는 **주문 단위 불변조건**은 별도로 설계해야 한다.

### 중복 요청 방지의 근본적인 보장 — 요청과 주문을 분리한다

**식별자 3개의 역할은 다르다.**

| 식별자 | 무엇을 식별하는가? |
| --- | --- |
| `user_id` | 요청 주체 및 멱등성 키의 적용 범위 |
| `order_id` | 결제 대상 주문 |
| `idem_key` | 한 번의 논리적 결제 요청과 그 재시도 |
| `payment_id` (선택적 별도 식별자) | 서버에서 생성·관리하는 결제 시도/결제 리소스 |

하나의 주문은 여러 상품 항목을 담을 수 있고, 실패·재시도 정책에 따라 여러 **결제 시도**를 가질 수 있다. 그러나 상품 항목의 수가 멱등성 키의 카디널리티를 결정하지 않는다. POST 응답이 유실되어 클라이언트가 `payment_id`를 몰라도 서버의 결제 기록은 유효할 수 있다.

**두 가지 보장은 별도로 필요하다.**

1. **Request-level idempotency:** 동일 범위의 `idem_key`가 여러 번 도착해도 **동일한 논리적 요청을 새로 실행하지 않음**. 예: `UNIQUE(user_id, idem_key)`를 DB에서 강제한 뒤, 키 충돌 시 원래 요청의 주체·`order_id`·금액·통화 등 의미 있는 내용을 비교한다. 동일하면 처리 중/완료 상태 또는 기존 결과를 반환하고, 다르면 **키 재사용 충돌**로 거절한다.
2. **Order-level invariant:** 다른 `idem_key`로도 동일 주문에 대한 부당한 **중복 청구가 발생하지 않도록** 주문 상태와 기존 결제 시도를 확인·동시성 제어한다. `UNIQUE(user_id, idem_key)`는 다른 키로 같은 주문에 접근하는 것을 막지 못한다. 반대로 무조건 `UNIQUE(user_id, order_id)`를 걸면 실패 후 합법적인 새 결제 시도까지 차단할 수 있다.

```text
Authenticated POST /payments (order_id, idem_key, amount, currency)
  ↓
요청 주체·주문 소유권·금액의 유효성 확인
  ↓
DB에서 (scope, idem_key) 원자적 등록 시도
  ├─ 기존 키 발견 + 요청 내용 동일 → 기존 시도 상태/결과 반환 (새 청구 없음)
  ├─ 기존 키 발견 + 요청 내용 다름 → 충돌 응답 (새 청구 없음)
  └─ 새 키 확보
      ↓
    해당 주문의 중복 청구 금지 정책/진행 중 거래 확인
      ↓
    결제 처리 권한 확보 및 영속 기록
      ↓
    결제사 호출과 결과 확인/반영
```

이 흐름은 **개념적 설계**다. 정확한 순서·트랜잭션 경계·예외 복구는 외부 결제사의 계약과 장애 모델에 따라 달라진다. 특히 조회 후 분기하는 애플리케이션 `if`문만으로는 동시성 경쟁을 막을 수 없으므로, 원자적 INSERT/제약·적절한 락 또는 상태 전이 제약이 필요하다. 동일 키로 진행 중인 작업을 만나면 **새 청구를 시작하지 않고** 처리 중 상태를 반환하거나 기존 작업의 결과를 기다린다.

**보장의 한계:** 로컬 DB의 UNIQUE 제약만으로 외부 결제사에 대한 *exactly-once* 효과를 증명할 수 없다. 타임아웃·서버 장애로 외부 처리 성공 여부를 모를 때는 무작정 재청구하지 말고, 결제사의 멱등성 지원/거래 식별자/상태 조회 및 정합성 복구 정책을 활용한다. 이 외부 상태 불일치 사례는 아래 **미해결 문제**로 남겨둔다.

> **Unique Request ≠ Unique Charge.** 먼저 요청 단위 멱등성을 보장하고, 별도로 주문 단위 중복 청구 불변조건을 보장한다.

### Race Condition / ACID

- `SELECT → 키 없음 → INSERT`만으로는 동시 요청을 막지 못한다. 서로 다른 요청이 같은 결론을 내릴 수 있기 때문이다.
- DB의 UNIQUE 제약과 원자적 INSERT/충돌 처리를 이용해 처리 소유권을 경쟁적으로 확보할 수 있다.
- 한 트랜잭션이 유일 키를 INSERT했지만 아직 커밋하지 않은 경우, 경쟁 INSERT는 먼저 진행 중인 트랜잭션의 결과를 기다릴 수 있다. 선행 트랜잭션이 **ROLLBACK**하면 경쟁 INSERT가 성공할 수도 있다.
- **Atomicity:** 트랜잭션 내부 중간 상태의 존재를 금지하는 것이 아니라 최종적으로 모든 변경이 함께 반영되거나 취소되도록 한다.
- **Isolation:** 동시 트랜잭션에서 어떤 변경을 관찰할 수 있는지는 격리 수준과 DB 구현에 따른다.
- **Consistency / Durability:** 정의된 DB 무결성 제약의 유지 / 커밋된 DB 변경의 지속성.
- DB 트랜잭션의 원자성은 **외부 결제 API 호출까지** 자동으로 롤백하거나 정확히 한 번 처리해 주지 않는다.

### PostgreSQL MVCC와 JPA 1차 캐시

| 관점 | 무엇을 관리하는가? |
| --- | --- |
| JPA 1차 캐시 | 영속성 컨텍스트 내 관리 엔티티의 객체 동일성·상태 |
| PostgreSQL MVCC Snapshot | DB 트랜잭션이 볼 수 있는 행 버전의 가시성 |

PostgreSQL 기준:

- `READ COMMITTED`: 다른 트랜잭션의 미커밋 변경을 읽지 못하며, **각 문장**에서 새로 커밋된 변경을 볼 수 있다.
- `REPEATABLE READ`: 보통 첫 쿼리에서 확보한 스냅샷을 유지한다. 다른 트랜잭션의 뒤늦은 커밋은 보이지 않지만, **자기 트랜잭션의 변경은 보인다**.
- 같은 행의 값이 재조회 시 달라지면 **Non-repeatable Read**, 같은 조건에 맞는 행 집합이 달라지면 **Phantom Read**라고 한다. PostgreSQL의 `REPEATABLE READ`는 둘 다 방지한다(다른 DB의 구현은 다를 수 있다).
- 스냅샷은 행의 값을 애플리케이션 캐시처럼 복사하는 것이 아니라 **가시성 기준**이다.
- JPA 벌크 UPDATE는 관리 중인 엔티티 필드를 직접 갱신하지 않아 DB 결과와 1차 캐시가 불일치할 수 있다. 필요에 따라 `flush()`, `clear()`, `refresh()`를 적절히 사용하되 기존 미반영 변경을 주의한다.
- `em.clear()`로 1차 캐시를 비워도 DB 스냅샷은 초기화되지 않는다. 같은 트랜잭션이 벌크 UPDATE한 행은 `REPEATABLE READ` 상태에서도 새로 조회할 때 **자기 변경 값**을 읽을 수 있다.

> **Atomicity ≠ Isolation. JPA 1차 캐시 ≠ DB MVCC Snapshot.**

### PG–Local 불일치: UNKNOWN과 MISMATCH를 구분한다

PG 조회 결과와 로컬 주문 기록이 다르다고 해서 무조건 `UNKNOWN`으로 표시하지 않는다.

- **UNKNOWN:** 실제 거래 결과를 아직 확인하지 못한 상태. 예: PG 조회 타임아웃, 조회 결과 불명확.
- **MISMATCH:** 두 시스템의 관찰 가능한 기록이 서로 다르다는 사실을 확인한 상태. 예: PG 승인 금액 50,000원, 로컬 주문 확정 금액 30,000원.
- **SUCCEEDED:** PG 거래가 성공했다는 사실을 나타낼 수 있지만, 주문 조건까지 충족했다는 의미와 동일하지 않다.

서로 다른 데이터의 **권위 범위**도 구분한다.

| 근거 | 권위 있는 사실 |
| --- | --- |
| PG 거래 조회/정산 기록 | 실제 승인·청구된 금액, PG 거래 상태 |
| 로컬 주문 기록 | 주문 당시 확정된 금액, 주문 소유자와 주문 조건 |

따라서 PG의 `SUCCEEDED` 응답만으로 로컬 주문까지 정상 완료 처리해서는 안 된다. 최소한 거래 식별자, 주문 식별자, 금액, 통화, 승인 상태 등 사전에 정의한 조건을 검증한다.

**권장 복구 원칙:**

1. 모니터링/로그로 장애 시점과 처리 단계를 관찰한다. 모니터링 도구는 장애 발견과 진단 근거이지, PG 거래 성공을 최종 증명하는 원천은 아니다.
2. PG의 기존 거래 조회 API 또는 신뢰할 수 있는 거래/정산 기록으로 실제 상태를 확인한다. 불확실한 경우 새 결제를 요청하지 않는다.
3. PG 기록과 로컬 주문 기록을 대조한다.
4. 일치하면 기존 거래를 기준으로 로컬 상태를 안전하게 동기화한다.
5. 금액·통화·주문 식별자 등이 불일치하면 `MISMATCH` / `RECONCILIATION_REQUIRED`로 분류하고 자동 성공 확정과 자동 재청구를 중단한다.
6. 근거를 조사한 뒤 승인된 조정·취소·환불 등의 절차를 수행하고 결과를 검증한다.

일시적 조회 실패처럼 재시도가 안전한 경우에는 지수 백오프와 Jitter, 최대 재시도 횟수 및 전체 기한을 적용할 수 있다. 그러나 **금액 불일치는 단순 재시도로 해결할 문제가 아니다.**

> **UNKNOWN = 아직 확인하지 못함. MISMATCH = 서로 다른 사실을 확인함.**

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
