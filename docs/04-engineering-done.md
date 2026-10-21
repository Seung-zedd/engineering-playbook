# 04 — Engineering Done

## 개발을 선형 Phase가 아니라 Slice Loop로 본다

```text
Problem → Boundary → Understand → Decide → Implement
→ Observe → Validate → Explain → Next Slice → Repeat
```

Agent가 `Implementation completed. All tests passed.`라고 보고해도 아직 DONE이 아니다.

> **이 시점부터 Human Validation이 시작된다.**

## 1. Observe

실제 시스템 상태를 직접 확인한다.

```text
DB row count
Sample rows
API request / response
Application logs
Stored state
Embedding existence
Vector records
Network traffic
Error state
```

추측하지 말고 가능한 한 **실제 값**을 본다.

## 2. Trace

실제 데이터 하나를 선택해 End-to-End로 추적한다.

```text
External API
→ Connector / Crawler
→ Raw Response
→ Normalization
→ Database
→ Chunk / Embedding
→ Vector Store
→ Query Embedding
→ Similarity Search
→ Retrieval
→ Application
→ UI
```

각 단계에서 `Input / Processing / Output / State Change / Failure Condition`을 확인한다. 그리고 묻는다.

- Whose responsibility is this?
- Why does this step exist?
- What evidence proves that it works?

## 3. Validate

```text
Expected ↔ Actual
```

테스트 결과뿐 아니라 실제 데이터와 시스템 상태를 비교한다.

## 4. Explain

코드나 Agent의 설명 없이 답해본다.

- 이 기능은 무엇을 하는가?
- 왜 필요한가?
- 데이터는 어디서 들어오는가?
- 어떤 과정을 거치는가?
- 누가 각 단계의 책임을 수행하는가?
- 왜 이렇게 설계했는가?
- 현재 실제 상태는 어떤가?
- 어떤 Evidence로 정상이라고 판단했는가?
- 어디까지 동작하고 어디부터 동작하지 않는가?

> **If I cannot explain it, I do not own it yet.**

## Engineering Done — 8 Questions

### WHAT
무엇을 구현했는가? 기능의 목적과 책임을 설명할 수 있는가?

### WHY
왜 이 방식으로 구현했는가? “Agent가 이렇게 구현했다”는 설계 근거가 아니다.

### DATA
실제 Input, Output, Stored Data, Row Count, Sample Data, Schema, Important Fields를 알고 있는가?

### FLOW
데이터와 시스템 로직의 End-to-End 흐름을 설명할 수 있는가?

### DECISION
어떤 판단을 내렸으며 왜 그렇게 결정했는가?

```text
Why this field?
Why this schema?
Why this API?
Why this table?
Why this value?
Why top_k = 5?
```

Magic Number, Default Value, Field Selection을 특히 의심한다.

### DIFFICULTY
실제로 어려웠던 것은 무엇인가? 어려운 것이 없었다면 “기술적으로 어려운 부분은 없었다”고 정확히 말한다.

```text
Technical Difficulty
Domain Understanding
Requirement Ambiguity
Integration Issue
Design Decision
Operational Constraint
Simple Implementation Work
```

### EVIDENCE
정상 동작을 무엇으로 증명할 수 있는가?

```text
SQL
Row Count
Sample Data
Logs
Metrics
API Response
Automated Tests
Manual Verification
```

“잘 된다”가 아니라 **“이 증거 때문에 정상이라고 판단했다”**고 말할 수 있어야 한다.

### LIMITATION
현재 구현이 하지 못하는 것은 무엇인가?

```text
Implemented
Not Implemented
Assumption
Known Limitation
Future Work
```

특히 PoC에서는 구현 범위와 한계를 명확하게 구분한다.

## Definition of Done

```text
Problem Understood
+ Boundary Understood
+ Responsibilities Understood
+ Code Complete
+ Tests Pass
+ Actual State Observed
+ Data Flow Traced
+ Expected == Actual
+ Design Decisions Understood
+ Evidence Collected
+ Limitations Known
+ Can Explain Without Agent
= ENGINEERING DONE
```

## Final Principles

> **TEST PASS ≠ ENGINEERING DONE.**

> **Agent's implementation completion is the beginning of Human validation.**

> **Do not trust what you have not observed.**

> **Question first, tool second.**

> **Understand the forest. Verify the trees.**

> **Don't stop at green. Understand why it's green.**
