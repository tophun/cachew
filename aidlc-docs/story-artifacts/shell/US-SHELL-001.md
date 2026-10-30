---
artefact_type: user_story
id: US-SHELL-001
unit: UNIT-SHELL
intent: requirements/intent.md
priority: high
status: draft
linked_domain_elements: []
linked_adrs: []
---

# US-SHELL-001: 메뉴바에서 배고픔 보기

## Story
**As a** 개발자,
**I want** 메뉴바의 캐슈 아이콘으로 지금 먹을 게 얼마나 있는지 알기를,
**so that** 앱을 열지 않아도 상태를 안다.

## Acceptance Criteria

- [ ] Given 제안 가능한 먹이 양에 따라, Then 아이콘이 배부름/출출함/배고픔 3단계로 바뀐다(기준은 US-FORAGE-005와 같은 값).
- [ ] Given 아이콘을 누르면, Then 패널(`MenuBarExtra`, `.window` 스타일)에 먹이 목록이 열린다.
- [ ] Given 아이콘 상태는, Then VoiceOver로 읽힌다 (NFR-U2).

## Out of Scope
- 없음

## NFR 연관
- NFR-U2, D3

## Risk 연관
- 없음

## Open Questions
- 없음
