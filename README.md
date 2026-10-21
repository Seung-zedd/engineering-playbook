# Engineering Playbook

실전 소프트웨어 개발과 Agentic Coding 과정에서 얻은 재사용 가능한 엔지니어링 원칙을 기록한다.

이 저장소는 특정 프레임워크의 사용법보다 **문제를 이해하고, 시스템을 설계하고, 실제 상태를 검증하고, 결과를 책임지는 방법**을 다룬다.

## Core Principle

> **TEST PASS ≠ ENGINEERING DONE.**

Agent가 구현을 완료한 순간은 작업의 끝이 아니다. Human의 검증이 시작되는 순간이다.

## Documents

- [01 — Engineering Mindset](docs/01-engineering-mindset.md) — TEST PASS의 한계, Agent와 Human의 책임, Seniority
- [02 — System Thinking](docs/02-system-thinking.md) — Problem, Boundary, Responsibility, Role, Collaboration, Data Flow
- [03 — Unknown Unknowns](docs/03-unknown-unknowns.md) — Question First, Tool Second와 Evidence 기반 탐색
- [04 — Engineering Done](docs/04-engineering-done.md) — Slice Loop, Human Validation, Definition of Done

## Core Loop

```text
Problem
  ↓
Boundary
  ↓
Understand
  ↓
Decide
  ↓
Implement     ← Agent-assisted
  ↓
Observe
  ↓
Validate
  ↓
Explain
  ↓
Next Slice → Repeat
```

## One-line Reminders

> **Don't stop at green. Understand why it's green.**

> **Do not trust what you have not observed.**

> **If I cannot explain it, I do not own it yet.**

> **Question first, tool second.**

> **Agent가 구현해도 판단 경험치는 Human이 먹는다.**
