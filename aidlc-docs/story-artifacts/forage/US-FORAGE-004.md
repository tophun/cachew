---
artefact_type: user_story
id: US-FORAGE-004
unit: UNIT-FORAGE
intent: requirements/intent.md
priority: medium
status: draft
linked_domain_elements: []
linked_adrs: []
---

# US-FORAGE-004: 실행 중인 도구의 먹이는 미루기

## Story
**As a** 개발자,
**I want** 관련 도구가 실행 중이면 그 먹이를 제안하지 않기를,
**so that** 빌드 중인 캐시를 지워 도구가 깨지는 일이 없다.

## Acceptance Criteria

- [ ] Given 카탈로그의 제외 조건에 해당하는 프로세스(예: Xcode)가 실행 중이면, Then 해당 먹이는 "○○ 종료 후 먹을 수 있어요"로 표시되고 선택할 수 없다.
- [ ] Given 승인 후 실행 직전에 도구가 켜졌으면, Then 그 항목은 실행되지 않는다 (NFR-S3).
- [ ] Given "실행 중" 판정은, Then 실행 중인 프로세스의 실행 파일 경로(필요하면 인자)로 한다. 셸 환경 변수(`VIRTUAL_ENV`, `NVM_DIR` 등)에 기대지 않는다 — 메뉴바 앱은 그 값을 볼 수 없다 (카탈로그 CR-5).
- [ ] Given 버전 매니저가 설치한 런타임(nvm Node, pyenv Python 등)이면, When 그 런타임 경로의 프로세스가 실행 중이면, Then 그 버전은 제안하지 않는다.

## Out of Scope
- 없음

## NFR 연관
- NFR-S3

## Risk 연관
- R-05

## Open Questions
- 없음
