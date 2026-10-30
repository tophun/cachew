---
artefact_type: unit
id: UNIT-DIGEST
intent: requirements/intent.md
status: approved
bolts: [BOLT-01, BOLT-04, BOLT-05]
---

# Unit: 먹기 (Digest)

## Mission
사용자가 승인한 제안만 실행한다. 실행 직전 재검증 → 휴지통 이동(기본) 또는 복구 불가 먹이의 2단계 확인 후 도구 명령 실행 → 먹은 기록 저장 → 휴지통 비우기 안내와 되돌리기를 제공한다.

## DDD 위치
- subdomain: supporting
- bounded context: Eating

## 포함 User Story
- US-DIGEST-001 — 승인한 먹이만 휴지통으로
- US-DIGEST-002 — 복구 불가 먹이는 두 번 확인
- US-DIGEST-003 — 휴지통 비우기 안내
- US-DIGEST-004 — 먹은 기록과 되돌리기

## Loose Coupling 검증
- SHELL에서 `ApprovedMeal`(승인된 제안 ID 목록 + 복구 불가 항목의 두 번째 확인 토큰)을 받는다.
- 먹기·되돌리기 결과를 `FoodEaten(크기, 등급, 복구 가능 여부)`, `FoodRestored` 도메인 이벤트로 낸다(비동기) → GROW가 구독.
- 재검증에 Shared Kernel(카탈로그·안전 정책)을 쓴다.
- 데이터 소유권: 먹은 기록(eat log)의 source of truth

## Suggested Bolts
- BOLT-01, BOLT-04, BOLT-05 (`plans/bolts/` 참조)

## NFR
- NFR-S3, S5~S7, S9, P3

## Risk
- R-01, R-02, R-06, R-07, R-14

## Measurement Criteria
- business metric: M-1 오삭제 0건, M-2 확보 용량
- technical metric: 먹기 실패율
