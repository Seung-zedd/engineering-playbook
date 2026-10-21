# 01 — Engineering Mindset

## TEST PASS ≠ ENGINEERING DONE

Agent-assisted development로 작은 데이터 처리 기능을 구현한 경험이 있었다. Use Case와 SPEC을 바탕으로 구현했고 자동화 테스트와 UI 확인도 통과했다.

그러나 리뷰에서 다음과 같은 종류의 질문에 충분히 답하지 못했다.

- 실제 입력 데이터의 규모와 형태는 무엇인가?
- 외부 입력에서 어떤 필드를 선택하며 그 이유는 무엇인가?
- 입력은 내부 모델로 어떻게 변환되는가?
- 데이터는 어떤 과정을 거쳐 저장되고 후속 처리되는가?
- 사용자의 요청이 들어오면 시스템 내부에서 어떤 단계가 실행되는가?
- 결과 개수 같은 기본값은 왜 그렇게 정했는가?
- 현재 프로토타입의 구현 범위와 한계는 무엇인가?
- 구현 과정에서 실제로 어려웠던 문제와 단순 작업은 무엇인가?

문제는 코드가 동작하지 않는 것이 아니었다.

> **동작하는 시스템과 그 시스템을 이해하고 책임질 수 있는 것은 서로 다른 문제다.**

이 경험은 특정 조직이나 프로젝트의 회고가 아니라, Agentic Coding 환경에서 반복될 수 있는 일반적인 엔지니어링 문제로 추상화한다.

## Test PASS가 보장하지 않는 것

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

```text
Problem → Hypothesis → Implementation → Failure
→ Debugging → Root Cause → Modification → Success
```

Agentic Coding에서는 이 과정이 쉽게 `Requirement → Agent → Implementation → Tests PASS → DONE?`으로 압축된다.

Agent가 구현의 시행착오를 제거하는 것은 좋다. 하지만 **Human이 가져가야 할 이해와 판단까지 제거되어서는 안 된다.**

> **Agent가 구현해도 판단 경험치는 Human이 먹어야 한다.**

## Human과 Agent의 역할

### Human
```text
Problem Definition
Requirement / Use Case Analysis
System / Data Modeling
Design & Trade-off Decision
Observation / Validation / Explanation
Final Responsibility
Definition of Done
```

### Agent
```text
Code Generation / Boilerplate
Implementation Assistance
Refactoring / Test Generation
Repetitive Work / Exploration Assistance
```

> **Agent가 구현을 대신할 수는 있다. Agent가 책임을 대신할 수는 없다.**

## Seniority에 대한 관점

경험이 쌓인다는 것은 단순히 답을 많이 외우는 것이 아니라 **의심하고 확인해야 할 지점의 목록이 늘어나는 것**이다.

> **Seniority is not knowing all the answers.  
> It is knowing which questions must be answered before you can trust the system.**

```text
Agent implements.        Human understands.
Agent tests.             Human validates.
Agent proposes.          Human decides.
Agent reports complete.  Human decides DONE.
```
