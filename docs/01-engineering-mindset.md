# 01 — Engineering Mindset

## TEST PASS ≠ ENGINEERING DONE

회사에서 Crawling + RAG PoC를 구현했다. Use Case를 분석하고 SPEC을 작성했으며 Agent를 이용해 구현했다. 테스트는 통과했고 UI에서도 정상적으로 동작했다.

그러나 리뷰에서 다음과 같은 질문에 제대로 답하지 못했다.

- 실제로 크롤링된 데이터는 몇 건인가?
- 각 사이트에서 어떤 필드를 가져오는가?
- 원본 데이터가 어떻게 정규화되는가?
- 데이터가 어떤 과정을 거쳐 저장되고 embedding 되는가?
- 사용자 질의가 들어오면 내부에서 무슨 일이 일어나는가?
- 왜 retrieval의 `top_k = 5`인가?
- 현재 PoC의 구현 범위와 한계는 무엇인가?
- 구현 과정에서 실제로 어려웠던 것은 무엇인가?

문제는 코드가 동작하지 않는 것이 아니었다.

> **내가 만든 시스템을 내가 충분히 이해하고 있지 않았다.**

## Test PASS가 보장하지 않는 것

Test PASS는 **테스트가 정의한 조건을 현재 구현이 만족한다**는 뜻이다.

```text
TEST PASS
  ≠ 요구사항을 올바르게 이해했다.
  ≠ 설계가 적절하다.
  ≠ 실제 데이터가 예상대로 들어왔다.
  ≠ 데이터 흐름을 이해하고 있다.
  ≠ 설계 결정의 근거를 설명할 수 있다.
  ≠ 실제 시스템 상태를 알고 있다.
  ≠ 시스템 전체가 올바르게 동작한다.
  ≠ 내가 만든 시스템을 내가 이해한다.
```

따라서 테스트 통과는 완료 조건 중 하나일 뿐이다.

## Agentic Coding의 함정

전통적인 개발에서는 다음과 같은 인지적 마찰을 개발자가 직접 경험한다.

```text
Problem
→ Hypothesis
→ Implementation
→ Failure
→ Debugging
→ Root Cause
→ Modification
→ Success
```

이 과정에서 실패 원인, 시스템 내부 동작, 선택한 구현과 버린 대안을 자연스럽게 학습한다.

Agentic Coding에서는 잘못하면 다음처럼 압축된다.

```text
Requirement
    ↓
  Agent
    ↓
Implementation
    ↓
Tests PASS
    ↓
   DONE?
```

Agent가 구현의 시행착오를 제거하는 것은 좋다. 하지만 그 과정에서 **Human이 가져가야 할 이해와 판단까지 제거되어서는 안 된다.**

> **Agent가 구현해도 판단 경험치는 Human이 먹어야 한다.**

## Human과 Agent의 역할

### Human

```text
Problem Definition
Requirement / Use Case Analysis
System / Data Modeling
Design & Trade-off Decision
Observation
Validation
Explanation
Final Responsibility
Definition of Done
```

### Agent

```text
Code Generation
Boilerplate
Implementation Assistance
Refactoring
Test Generation
Repetitive Work
Exploration Assistance
```

> **Agent가 구현을 대신할 수는 있다. Agent가 책임을 대신할 수는 없다.**

## Seniority에 대한 관점

경험이 쌓인다는 것은 단순히 답을 많이 외우는 것이 아니다.

> **의심하고 확인해야 할 지점의 목록이 늘어나는 것이다.**

Senior는 모든 답을 알고 있는 사람이 아니라 시스템을 신뢰하기 전에 어떤 질문에 답해야 하는지 더 많이 아는 사람에 가깝다.

> **Seniority is not knowing all the answers.  
> It is knowing which questions must be answered before you can trust the system.**

Agent 시대에 구현 비용이 줄어든다면 Human은 절약된 시간을 Problem Framing, Modeling, Decision, Observation, Validation, Explanation, Retrospective에 투자해야 한다.

```text
Agent implements.        Human understands.
Agent tests.             Human validates.
Agent proposes.          Human decides.
Agent reports complete.  Human decides DONE.
```
