---
artefact_type: user_story
id: US-DIGEST-001
unit: UNIT-DIGEST
intent: requirements/intent.md
priority: high
status: draft
linked_domain_elements: []
linked_adrs: []
---

# US-DIGEST-001: 승인한 먹이만 휴지통으로

## Story
**As a** 개발자,
**I want** 내가 체크한 먹이만 휴지통으로 보내기를,
**so that** 마음이 바뀌면 되돌릴 수 있다.

## Acceptance Criteria

- [ ] Given 사용자가 명시적으로 승인한 항목만, Then 실행된다 (D3).
- [ ] Given 실행 직전에 대상이 바뀌었으면(경로·symlink·git 변경·도구 실행), Then 그 항목은 건너뛰고 이유를 보여 준다 (NFR-S3).
- [ ] Given 휴지통 가능 먹이는, Then 휴지통으로 보내고 결과 휴지통 경로를 먹은 기록에 남긴다 (D4, NFR-S5, S7).
- [ ] Given 일부가 실패하면, Then 성공·실패 항목을 나눠 보여 준다.

## Out of Scope
- 복구 불가 먹이(US-DIGEST-002)

## NFR 연관
- NFR-S1~S5, S7

## Risk 연관
- R-01, R-09

## Open Questions
- 없음
