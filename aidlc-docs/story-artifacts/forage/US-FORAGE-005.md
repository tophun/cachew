---
artefact_type: user_story
id: US-FORAGE-005
unit: UNIT-FORAGE
intent: requirements/intent.md
priority: medium
status: draft
linked_domain_elements: []
linked_adrs: []
---

# US-FORAGE-005: 자동 찾기와 배고픔 알림

## Story
**As a** 개발자,
**I want** 캐슈가 주기적으로 먹이를 찾아 일정량 이상이면 알려 주기를,
**so that** 디스크가 꽉 차기 전에 정리할 수 있다.

## Acceptance Criteria

- [ ] Given 자동 찾기가 실행되면, Then 찾기와 제안만 하고 먹지는 않는다 (D3). 알림을 누르면 목록이 열린다.
- [ ] Given 기본 설정이면, Then 자동 찾기는 하루 1회다(설정에서 변경 가능).
- [ ] Given 제안 가능한 A·B 먹이 합계가 기준 이상이거나 디스크 여유 공간이 기준 미만이면, Then 캐릭터 말투 알림을 보낸다(예: "배고파요… 12GB 먹어도 돼요?").
- [ ] Given 같은 내용의 알림은, Then 하루 1회를 넘지 않는다.

## Out of Scope
- 알림에서 바로 먹기(D3 위반이라 하지 않음)

## NFR 연관
- NFR-F3, F4

## Risk 연관
- 없음

## Open Questions
- 알림 기준값 — 초안: A·B 합계 5GB 이상 또는 여유 공간 10% 미만
