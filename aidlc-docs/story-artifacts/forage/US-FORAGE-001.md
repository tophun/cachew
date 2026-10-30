---
artefact_type: user_story
id: US-FORAGE-001
unit: UNIT-FORAGE
intent: requirements/intent.md
priority: high
status: draft
linked_domain_elements: []
linked_adrs: []
---

# US-FORAGE-001: 기본 경로의 먹이 찾기

## Story
**As a** 개발자,
**I want** 캐슈가 홈 디렉터리의 알려진 개발 캐시 경로(먹이 카탈로그)를 찾아 크기와 함께 보여 주기를,
**so that** 무엇이 디스크를 차지하는지 직접 뒤지지 않고 알 수 있다.

## Acceptance Criteria

- [ ] Given 카탈로그에 등록된 기본 경로가 있을 때, When 스캔하면, Then 항목마다 이름·경로·크기·안전 등급·재생성 방법이 목록에 나온다.
- [ ] Given 카탈로그에 없는 경로는, Then 어떤 경우에도 목록에 나오지 않는다 (NFR-S1).
- [ ] Given 안전 등급 X 경로는, Then 목록에 나오지 않는다 (NFR-S2).
- [ ] Given 대상이 symlink이면, Then 따라가지 않고, 실제 경로가 allowlist 밖이면 제외한다 (NFR-S4).
- [ ] Given 스캔 중일 때, Then 진행률이 보이고 언제든 취소할 수 있다.

## Out of Scope
- 프로젝트 폴더 안의 먹이(US-FORAGE-002)

## NFR 연관
- NFR-S1, S2, S4, F1, F4

## Risk 연관
- R-03, R-04, R-09

## Open Questions
- 환경 변수로 위치가 바뀐 캐시를 어디까지 따라갈지(R-04) — 초안: 기본 경로만 + 설정에서 추가 경로
