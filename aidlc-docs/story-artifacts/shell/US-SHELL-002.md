---
artefact_type: user_story
id: US-SHELL-002
unit: UNIT-SHELL
intent: requirements/intent.md
priority: medium
status: draft
linked_domain_elements: []
linked_adrs: []
---

# US-SHELL-002: 필요할 때만 권한 요청

## Story
**As a** 개발자,
**I want** 권한이 필요한 먹이를 다룰 때만 이유와 함께 권한 요청을 받기를,
**so that** 불필요한 권한을 주지 않아도 된다.

## Acceptance Criteria

- [ ] Given 권한이 필요한 먹이를 처음 다룰 때만, Then 이유를 설명하고 권한을 요청한다 (NFR-A2).
- [ ] Given 권한이 거부되면, Then 그 먹이만 "권한 없음"으로 표시하고 나머지는 계속 처리한다.
- [ ] Given 앱은, Then Full Disk Access를 기본으로 요구하지 않는다 (NFR-A3).

## Out of Scope
- 없음

## NFR 연관
- NFR-A1~A3

## Risk 연관
- R-08

## Open Questions
- 없음
