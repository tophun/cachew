---
artefact_type: user_story
id: US-DIGEST-003
unit: UNIT-DIGEST
intent: requirements/intent.md
priority: high
status: draft
linked_domain_elements: []
linked_adrs: []
---

# US-DIGEST-003: 휴지통 비우기 안내

## Story
**As a** 개발자,
**I want** 먹은 뒤 "휴지통을 비우면 N GB 확보"라는 안내를 받기를,
**so that** 공간이 바로 늘지 않은 이유를 알고 직접 비울 수 있다.

## Acceptance Criteria

- [ ] Given 먹기가 끝나면, Then 휴지통에 있는 캐슈가 먹은 양(확보 예정 용량)을 보여 준다.
- [ ] Given 복구 불가 먹이로 즉시 확보된 용량은, Then 따로 표시한다.
- [ ] Given 블록을 공유하는 먹이(pnpm·Bun·uv·conda)는, Then 크기를 "최대 N GB"로 표시한다 (카탈로그 CR-6).
- [ ] Given 먹기가 끝나면, Then 먹기 전후 볼륨 여유 공간 차이를 "실제 확보량"으로 먹은 기록에 남긴다. 휴지통 먹이는 비운 뒤에야 늘어나므로 그 경우 "휴지통 비우면 확보"로 구분한다.
- [ ] Given 앱은, Then 휴지통을 비우지 않는다 (C2). Finder 휴지통을 여는 버튼을 제공한다.

## Out of Scope
- 앱이 휴지통을 자동으로 비우는 것

## NFR 연관
- NFR-S5

## Risk 연관
- R-06, R-14

## Open Questions
- 없음
