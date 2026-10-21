# 03 — Unknown Unknowns

## 무엇을 모르는지조차 모를 때

경험이 부족하면 질문 자체가 머릿속에 존재하지 않을 수 있다.

이 상태에서 바로 도구부터 고르지 않는다.

Apidog, SQL, Debugger, Log는 문제 해결 방법론이 아니라 **관측과 Evidence 확보를 위한 수단**이다.

> **Question first, tool second.**

```text
What do I need to know?
        ↓
What evidence would answer it?
        ↓
Which tool can obtain that evidence?
```

예를 들어:

```text
Question
"실제로 API에서 어떤 데이터가 들어오는가?"
        ↓
Evidence
Actual Request / Response
        ↓
Tool
Apidog / curl / Network Inspector
```

```text
Question
"실제로 몇 건이 저장되었는가?"
        ↓
Evidence
Database State
        ↓
Tool
SQL / DB Client
```

## Boundary → Flow → State → Decision → Evidence

무엇을 확인해야 하는지 모르겠다면 다섯 방향에서 질문을 만든다.

### Boundary

```text
Where does this feature begin?
Where does it end?
Which external systems does it depend on?
What is our responsibility?
```

### Flow

```text
What enters the system?
What happens to it?
Where does it go next?
What eventually comes out?
```

### State

```text
What is stored?
What changes?
What is the current actual state?
How many records exist?
```

### Decision

```text
Why this field?
Why this schema?
Why this API?
Why this algorithm?
Why this default value?
Why this boundary?
```

### Evidence

```text
How do I know this is true?
What can I observe?
What measurement proves it?
```

Evidence를 정의한 뒤 적절한 관측 수단을 선택한다.

```text
HTTP        → Apidog / curl / Network Inspector
Database    → SQL / DB Client
Runtime     → Logs / Debugger
Behavior    → Automated Test / Manual Verification
Performance → Metrics / Profiler
```

## Unknown Unknown Loop

모든 Unknown Unknown을 미리 제거할 수는 없다. 목표는 그것을 **발견해서 Known Unknown으로 바꾸는 능력**을 높이는 것이다.

```text
Unknown Unknown
      ↓
Question Discovery
      ↓
Known Unknown
      ↓
Observation
      ↓
Evidence
      ↓
Knowledge
      ↓
New Questions
      ↓
Repeat
```

예전에는 “DB에 실제 몇 건이 저장됐는가?”라는 질문 자체가 존재하지 않았더라도 Review를 통해 질문을 발견할 수 있다. 이후 SQL로 실제 상태를 확인하면 Known Unknown이 Knowledge로 바뀐다.

> **Experience converts unknown unknowns into questions.**

경험은 모든 답을 알려주는 것이 아니라 이전에는 존재조차 몰랐던 질문을 다음에는 먼저 떠올리게 한다.

## AI와 Review를 Blind Spot 탐지기로 사용한다

AI와 Senior Review는 답을 대신 결정하는 데만 사용하지 않는다. **놓친 질문을 발견하는 용도**로 사용할 수 있다.

예시 프롬프트:

> 이 기능 또는 설계에서 내가 놓쳤을 가능성이 있는 질문을 Boundary, Flow, State, Decision, Evidence 관점에서 찾아라. 답을 대신 결정하지 말고 내가 확인해야 할 Unknown을 제시하라.

AI나 Reviewer가 Blind Spot을 제시할 수는 있지만 최종 Observation, Validation, Decision, Explanation의 책임은 Human에게 있다.
