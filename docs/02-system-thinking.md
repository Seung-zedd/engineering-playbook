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
