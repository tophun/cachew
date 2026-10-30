---
artefact_type: user_story
id: US-FORAGE-003
unit: UNIT-FORAGE
intent: requirements/intent.md
priority: high
status: draft
linked_domain_elements: []
linked_adrs: []
---

# US-FORAGE-003: 안전 등급에 따라 다르게 제안

## Story
**As a** 개발자,
**I want** 먹이가 안전 등급(A/B/C/X)에 따라 다르게 제안되기를,
**so that** 위험한 것을 실수로 먹지 않는다.

## Acceptance Criteria

- [ ] Given 등급 A·B 먹이는, Then 기본 목록에 제안된다. B에는 "다시 만들려면 시간이 걸림"이 표시된다.
- [ ] Given 등급 C 먹이는, Then 자동 제안되지 않고 "직접 확인 필요" 영역을 펼쳐야 보이며, 일괄 선택할 수 없다.
- [ ] Given 등급 X는, Then 어디에도 나오지 않는다.
- [ ] Given 카탈로그에서 출처가 "미확인"인 항목은, Then 한 단계 낮은 등급으로 다뤄진다.

## Out of Scope
- 등급 판정 기준 자체(카탈로그 문서가 소유)

## NFR 연관
- NFR-S2, U3

## Risk 연관
- R-01

## Open Questions
- 없음
