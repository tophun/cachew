---
artefact_type: user_story
id: US-DIGEST-002
unit: UNIT-DIGEST
intent: requirements/intent.md
priority: high
status: draft
linked_domain_elements: []
linked_adrs: []
---

# US-DIGEST-002: 복구 불가 먹이는 두 번 확인

## Story
**As a** 개발자,
**I want** 휴지통에 넣을 수 없는 먹이(Docker 이미지 등)는 복구할 수 없다는 것을 분명히 알고 한 번 더 확인한 뒤 먹기를,
**so that** 되돌릴 수 없는 삭제를 실수로 하지 않는다.

## Acceptance Criteria

- [ ] Given 복구 불가 먹이는, Then 목록에서 "복구 불가" 표시로 일반 먹이와 구별된다 (C3).
- [ ] Given 복구 불가 먹이를 승인하면, Then 실행할 명령 원문을 보여 주는 두 번째 확인을 거친 뒤에만 실행한다 (NFR-S6).
- [ ] Given 일반 먹이와 함께 선택해도, Then 복구 불가 먹이는 별도로 확인받는다.
- [ ] Given 명령을 실행하면, Then 종료 코드와 출력 요약을 먹은 기록에 남긴다.
- [ ] Given 해당 도구가 없거나 데몬이 꺼져 있으면, Then 그 먹이는 제안하지 않는다.
- [ ] Given Docker 먹이는, Then 이미지·빌드 캐시·정지 컨테이너·볼륨을 행별로 나눠 각각 두 번째 확인을 받는다. `docker system prune` 같은 한 번에 지우는 버튼은 두지 않는다 (카탈로그 CR-10).
- [ ] Given 순서가 있는 도구 명령(예: 시뮬레이터 런타임 삭제 → `xcrun simctl delete unavailable`)은, Then 순서대로 보여 주고 앞 단계가 실패하면 뒤 단계를 실행하지 않는다 (카탈로그 CR-7).
- [ ] Given 도구 명령은, Then 셸을 거치지 않고 실행 파일 절대 경로 + 인자 배열로 실행한다 (NFR-S9).

## Out of Scope
- 없음

## NFR 연관
- NFR-S6, S7, S9

## Risk 연관
- R-02

## Open Questions
- 없음
