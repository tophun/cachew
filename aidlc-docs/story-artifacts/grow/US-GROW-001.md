---
artefact_type: user_story
id: US-GROW-001
unit: UNIT-GROW
intent: requirements/intent.md
priority: medium
status: draft
linked_domain_elements: []
linked_adrs: []
---

# US-GROW-001: 먹은 만큼 경험치

## Story
**As a** 개발자,
**I want** 먹은 용량만큼 캐슈가 경험치를 얻기를 (D7),
**so that** 정리가 즐거워진다.

## Acceptance Criteria

- [ ] Given 휴지통 이동(또는 복구 불가 먹이의 명령)이 성공하면, Then 그 순간 경험치가 쌓인다: 1 GB = 10 XP. 앱은 휴지통 비움을 감지하지 않는다(C2).
- [ ] Given 안전 등급 C 또는 복구 불가 먹이는, Then 0.5배만 쌓인다(R-10).
- [ ] Given 앱의 되돌리기로 복구하면, Then 그만큼 경험치를 뺀다.
- [ ] Given 경험치는, Then 로컬에만 저장된다 (NFR-P3).

## Out of Scope
- 레벨(D7: 레벨 대신 코스튬)

## NFR 연관
- NFR-P3

## Risk 연관
- R-10

## Open Questions
- ~~적립 시점, C·복구 불가 가중치~~ → 2026-10-30 승인: 휴지통 이동 시 적립, 0.5배
