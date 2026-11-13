# 02 — System Thinking

## 제품과 시스템을 서로 다른 Zoom Level로 본다

```text
User / Stakeholder → Problem → Use Case → User Action → Expected Outcome
```

그 다음 구현된 시스템을 **책임, 협력, 데이터 흐름, 상태**로 이해한다.

```text
Input / External System → Processing → Transformation
→ State Change → Persistence → Output → User
```

> **제품은 Problem과 Use Case로 이해하고, 시스템은 Responsibility와 Data Flow로 이해한다.**

## Boundary → Responsibility → Role → Collaboration

**Boundary**는 우리 시스템이 책임질 것과 책임지지 않을 것을 결정하는 경계다.

```text
What is inside our system?
What is outside our system?
Where does our responsibility begin and end?
```

**Responsibility**는 무엇을 해야 하는가, **Role**은 협력에서 누가 그 책임을 수행하는가에 관한 것이다.

```text
Role A → Role B → Role C → Result
```

> **시스템은 단순한 객체의 집합이 아니라 책임을 가진 역할들의 협력이다.**

## Collaboration을 따라가면 Data Flow가 보인다

```text
Source
  │ Input Data
  ↓
Component A
  │ Transformed Data
  ↓
Component B
  │ Persisted State
  ↓
Component C
  │ Result
  ↓
User
```

대표 데이터 하나를 End-to-End로 추적하면서 묻는다.

```text
Who created this data?
Who transformed it?
Who owns this state?
Who persisted it?
Who consumes it next?
Why does this transformation exist?
```

> **Responsibility와 Collaboration은 시스템의 구조를 보여주고, Data Flow는 그 구조가 Runtime에서 어떻게 움직이는지 보여준다.**

## 숲과 나무를 동시에 본다

### Zoom Out — 숲
```text
Who has the problem?
Why does this system exist?
What is the Use Case?
Where is the Boundary?
What responsibilities exist?
How do the roles collaborate?
What is the End-to-End Flow?
```

### Zoom In — 나무
```text
What fields are in this input?
How is this value transformed?
What state actually exists?
Why was this default chosen?
What happens when this operation fails?
What does this evidence prove?
```

```text
Problem → System → Component → Data → Code
Code → Data → Component → System → Problem
```

> **A good engineer can zoom out to understand why the system exists, and zoom in to prove how it actually works.**

## Use Case — Actor와 System 사이의 Black Box

Use Case는 **Actor가 목표를 달성하기 위해 시스템과 상호작용하는 과정**을 외부 관찰 가능 행동 중심으로 기술한다.

- **Problem:** 어떤 어려움을 해결하려는가?
- **Goal:** Actor가 도달하려는 상태는 무엇인가?
- **Use Case:** Actor와 System이 어떤 상호작용으로 목표를 달성하는가?

Use Case는 내부 컴포넌트와 알고리즘을 나열하는 구현 설계서가 아니다. 단, **외부에서 관찰 가능한 시스템의 응답**은 Main Success Scenario에 포함된다.

### 기본 구성 요소

| 요소 | 의미 |
| --- | --- |
| Actor | 목표를 가진 상호작용 주체 |
| Goal | Actor가 달성하려는 목적 |
| Trigger | 상호작용을 시작하게 하는 사건 |
| Precondition | 해당 Use Case가 시작되기 전에 성립해야 하는 조건 |
| Main Success Scenario | 정상 경로의 Actor–System 상호작용 |
| Result | 실행 과정에서 생성되거나 반환된 결과물 |
| Success Postcondition | 정상 종료 후 보장되어야 하는 조건 |

Precondition은 **애플리케이션 프로세스의 시작 조건**과 다르다. 예를 들어 애플리케이션은 이미 실행 중이지만 특정 작업을 시작하려면 사용자의 권한이 필요할 수 있다.

### Result ≠ Success Postcondition

일반적인 정보 등록 기능을 예로 들자.

```text
Actor: Authorized User
Goal: 새로운 항목을 등록한다.
Precondition: 사용자가 등록 권한을 갖는다.
Result: "등록 성공"이라는 메시지가 표시된다.
Success Postcondition: 항목이 실제로 저장되어 조회할 수 있다.
```

화면에는 성공 메시지가 표시됐지만 실제 데이터가 저장되지 않았다면 **Result는 존재하지만 Success Postcondition은 미충족**이다. 따라서 Use Case가 성공했다고 판단할 수 없다.

> **Success Response ≠ Successful Outcome.**

Postcondition은 반드시 영속 상태 변경을 의미하지 않는다. 정보 조회처럼 외부 응답 자체가 Goal인 경우에도 성공 후 성립할 조건을 기술할 수 있다.

이 구분은 [Engineering Done](04-engineering-done.md)의 실제 상태 관측과 검증 원칙으로 이어진다.
