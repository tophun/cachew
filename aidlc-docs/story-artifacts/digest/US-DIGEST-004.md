---
artefact_type: user_story
id: US-DIGEST-004
unit: UNIT-DIGEST
intent: requirements/intent.md
priority: medium
status: draft
linked_domain_elements: []
linked_adrs: []
---

# US-DIGEST-004: 먹은 기록과 되돌리기

## Story
**As a** 개발자,
**I want** 캐슈가 언제 무엇을 먹었는지 보고 되돌리기를,
**so that** 잘못 먹은 것을 복구할 수 있다.

## Acceptance Criteria

- [ ] Given 먹은 기록에는, Then 시각·원래 경로·크기·휴지통 경로(또는 실행한 명령)·결과가 있다 (NFR-S7).
- [ ] Given 휴지통에 아직 있는 항목은, Then 원래 위치로 되돌릴 수 있다. 원래 위치에 같은 이름이 생겼으면 덮어쓰지 않고 알린다.
- [ ] Given 복구 불가 항목은, Then 되돌리기 대신 재생성 방법을 안내한다.

## Out of Scope
- 없음

## NFR 연관
- NFR-S7, P3

## Risk 연관
- R-07

## Open Questions
- Finder '되돌려 놓기'가 앱이 휴지통으로 보낸 항목에도 동작하는지(R-07, 첫 Bolt에서 검증)
