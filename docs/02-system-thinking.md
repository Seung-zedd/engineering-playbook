# 02 — System Thinking

## 제품과 시스템을 서로 다른 Zoom Level로 본다

제품은 먼저 **사용자의 문제와 Use Case**로 이해한다.

```text
User / Stakeholder
      ↓
Problem
      ↓
Use Case
      ↓
User Action
      ↓
Expected Outcome
```

그 다음 구현된 시스템을 **책임, 협력, 데이터 흐름, 상태**로 이해한다.

```text
Input / External System
      ↓
Processing
      ↓
Transformation
      ↓
State Change
      ↓
Persistence
      ↓
Output
      ↓
User
```

> **제품은 Problem과 Use Case로 이해하고, 시스템은 Responsibility와 Data Flow로 이해한다.**

## Boundary → Responsibility → Role → Collaboration

### Boundary

Boundary는 책임 그 자체라기보다 **우리 시스템이 책임질 것과 책임지지 않을 것을 결정하는 경계**다.

```text
What is inside our system?
What is outside our system?
Where does our responsibility begin?
Where does it end?
```

### Responsibility

```text
What must this component do?
```

책임은 **무엇을 해야 하는가**다.

### Role

```text
Who performs this responsibility?
```

역할은 **이 협력에서 누가 그 책임을 수행하는가**다.

### Collaboration

하나의 Use Case는 한 객체가 혼자 완성하지 않는다.

```text
Role A
  ↓
Role B
  ↓
Role C
  ↓
Result
```

> **시스템은 단순한 객체의 집합이 아니라 책임을 가진 역할들의 협력이다.**

## Collaboration을 따라가면 Data Flow가 보인다

역할들이 협력하려면 데이터나 메시지를 주고받는다.

```text
Source
  │ Raw Data
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

반대로 실제 데이터 하나를 End-to-End로 추적하면 시스템의 책임과 협력이 드러난다.

```text
Who created this data?
Who transformed it?
Who owns this state?
Who persisted it?
Who consumes it next?
Why does this transformation exist?
```

> **Responsibility와 Collaboration은 시스템의 구조를 보여주고, Data Flow는 그 구조가 Runtime에서 어떻게 움직이는지 보여준다.**

따라서 시스템을 이해할 때 파일을 순서대로 읽는 것만으로 시작하지 않는다. 가능하면 **실제 데이터 하나를 잡고 처음부터 끝까지 추적한다.**

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
What fields are in this response?
How is this value transformed?
How many rows actually exist?
Why is top_k = 5?
What happens when this function fails?
What state changed?
What does this log prove?
```

좋은 엔지니어는 필요할 때 Zoom Level을 바꾼다.

```text
Problem → System → Component → Data → Code
Code → Data → Component → System → Problem
```

숲만 보면 추상적인 설계만 남고 실제 시스템을 증명하지 못한다. 나무만 보면 개별 기능은 구현하지만 무엇을 만들고 있는지 잃어버릴 수 있다.

> **A good engineer can zoom out to understand why the system exists, and zoom in to prove how it actually works.**
