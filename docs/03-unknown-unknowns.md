# 03 — Unknown Unknowns

## 무엇을 모르는지조차 모를 때

경험이 부족하면 질문 자체가 머릿속에 존재하지 않을 수 있다. 이 상태에서 바로 도구부터 고르지 않는다.

API Client, SQL Client, Debugger, Log, Profiler는 문제 해결 방법론이 아니라 **관측과 Evidence 확보를 위한 수단**이다.

> **Question first, tool second.**

```text
What do I need to know?
        ↓
What evidence would answer it?
        ↓
Which tool can obtain that evidence?
```

예:

```text
"실제 외부 입력은 어떤 형태인가?"
→ Actual Request / Response
→ API Client / curl / Network Inspector

"실제로 어떤 상태가 저장되었는가?"
→ Database State
→ SQL / DB Client
```

## Boundary → Flow → State → Decision → Evidence

### Boundary
```text
Where does this feature begin and end?
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
What is its scale or cardinality?
```

### Decision
```text
Why this field / schema / interface?
Why this algorithm / default / boundary?
```

### Evidence
```text
How do I know this is true?
What can I observe?
What measurement proves it?
```

```text
HTTP        → API Client / curl / Network Inspector
Database    → SQL / DB Client
Runtime     → Logs / Debugger
Behavior    → Automated Test / Manual Verification
Performance → Metrics / Profiler
```

## Unknown Unknown Loop

```text
Unknown Unknown → Question Discovery → Known Unknown
→ Observation → Evidence → Knowledge → New Questions → Repeat
```

처음에는 “실제 저장 상태를 확인해야 한다”는 질문 자체가 존재하지 않을 수 있다. Review를 통해 질문을 발견하고 실제 상태를 관측하면 Known Unknown이 Knowledge로 바뀐다.

> **Experience converts unknown unknowns into questions.**

## AI와 Review를 Blind Spot 탐지기로 사용한다

> 이 기능 또는 설계에서 내가 놓쳤을 가능성이 있는 질문을 Boundary, Flow, State, Decision, Evidence 관점에서 찾아라. 답을 대신 결정하지 말고 내가 확인해야 할 Unknown을 제시하라.

AI나 Reviewer가 Blind Spot을 제시할 수는 있지만 최종 Observation, Validation, Decision, Explanation의 책임은 Human에게 있다.
