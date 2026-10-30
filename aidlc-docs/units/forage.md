---
artefact_type: unit
id: UNIT-FORAGE
intent: requirements/intent.md
status: approved
bolts: [BOLT-01, BOLT-02, BOLT-03]
---

# Unit: 먹이 찾기 (Forage)

## Mission
먹이 카탈로그(데이터)와 안전 정책(등급·allowlist·deny-list·제외 조건)으로 디스크를 스캔해, 사용자가 판단할 수 있는 **제안(Proposal)** 목록을 만든다. 캐슈의 핵심 가치인 "안전하게 먹을 수 있는 것만 찾는다"를 책임진다. UI와 독립된 모듈이라 이후 CLI가 그대로 재사용한다(P1).

## DDD 위치
- subdomain: core
- bounded context: Foraging

## 포함 User Story
- US-FORAGE-001 — 기본 경로의 먹이 찾기
- US-FORAGE-002 — 프로젝트 폴더 안의 먹이 찾기
- US-FORAGE-003 — 안전 등급에 따라 다르게 제안
- US-FORAGE-004 — 실행 중인 도구의 먹이는 미루기
- US-FORAGE-005 — 자동 찾기와 배고픔 알림(주기·기준 판단)
- US-FORAGE-006 — 다 쓴 git 워크트리 먹기

## Loose Coupling 검증
- SHELL에 `scan()` 결과로 `Proposal` 값 객체 목록을 준다(동기 API). 자동 찾기가 끝나면 `ScanCompleted` 이벤트를 낸다.
- DIGEST는 실행 직전 재검증에 FORAGE의 **안전 정책 모듈**을 쓴다 → 카탈로그·안전 정책은 FORAGE와 DIGEST가 공유하는 Shared Kernel로 분리한다(이 둘 사이의 유일한 결합).
- 데이터 소유권: 먹이 카탈로그와 최근 스캔 스냅샷의 source of truth

## Suggested Bolts
- BOLT-01, BOLT-02, BOLT-03 (`plans/bolts/` 참조)

## NFR
- NFR-S1~S4, S8, F1~F4, O1, O3

## Risk
- R-01, R-03, R-04, R-05, R-08, R-09, R-13, R-15, R-16

## Measurement Criteria
- business metric: M-2 확보 용량(제안 가능 총량)
- technical metric: NFR-F1 스캔 시간
