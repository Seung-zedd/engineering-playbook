# 04 — Engineering Done

## 개발을 선형 Phase가 아니라 Slice Loop로 본다

```text
Problem → Boundary → Understand → Decide → Implement
→ Observe → Validate → Explain → Next Slice → Repeat
```

Agent가 `Implementation completed. All tests passed.`라고 보고해도 아직 DONE이 아니다.

> **이 시점부터 Human Validation이 시작된다.**

## 1. Observe

```text
State count / cardinality
Representative records
External request / response
Application logs
Persisted state
Derived artifacts
Network traffic
Error state
```

추측하지 말고 가능한 한 **실제 값**을 본다.

## 2. Trace

대표 데이터 하나를 선택해 End-to-End로 추적한다.

```text
External Input
→ Adapter / Interface
→ Raw Input
→ Transformation
→ Persistence
→ Derived Processing
→ Retrieval / Business Logic
→ Application
→ Output
```

각 단계에서 `Input / Processing / Output / State Change / Failure Condition`을 확인하고 묻는다.

- Whose responsibility is this?
- Why does this step exist?
- What evidence proves that it works?

## 3. Validate

```text
Expected ↔ Actual
```

테스트 결과뿐 아니라 실제 데이터와 시스템 상태를 비교한다.

## 4. Explain

코드나 Agent의 설명 없이 기능의 목적, 입력, 처리 과정, 책임, 설계 근거, 실제 상태, Evidence, 한계를 설명한다.

> **If I cannot explain it, I do not own it yet.**

## Engineering Done — 8 Questions

### WHAT
무엇을 구현했는가?

### WHY
왜 이 방식으로 구현했는가?

### DATA
실제 Input, Output, Stored State, Cardinality, Representative Data, Schema, Important Fields를 알고 있는가?

### FLOW
End-to-End 흐름을 설명할 수 있는가?

### DECISION
```text
Why this field?
Why this schema?
Why this interface?
Why this storage?
Why this value?
Why this default?
```

### DIFFICULTY
실제로 어려웠던 것은 무엇인가? 기술적 난이도, 도메인 이해, 요구사항 모호성, 통합 문제, 설계 판단, 운영 제약, 단순 구현 작업을 구분한다.

### EVIDENCE
```text
Queries / Counts / Representative Data
Logs / Metrics / External Response
Automated Tests / Manual Verification
```

“잘 된다”가 아니라 **“이 증거 때문에 정상이라고 판단했다”**고 말할 수 있어야 한다.

### LIMITATION
```text
Implemented
Not Implemented
Assumption
Known Limitation
Future Work
```

특히 Prototype/PoC에서는 구현 범위와 한계를 명확하게 구분한다.

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
