---
artefact_type: unit
id: UNIT-SHELL
intent: requirements/intent.md
status: approved
bolts: [BOLT-01, BOLT-07, BOLT-08]
---

# Unit: 메뉴바 앱 (Shell)

## Mission
`MenuBarExtra` 메뉴바 UI, 승인 흐름, 설정, 알림, 권한 안내, 설치·배포(서명·공증·Homebrew cask)를 맡는다. 도메인 판단은 하지 않고 다른 Unit을 조합한다.

## DDD 위치
- subdomain: generic
- bounded context: App Shell

## 포함 User Story
- US-SHELL-001 — 메뉴바에서 배고픔 보기
- US-SHELL-002 — 필요할 때만 권한 요청
- US-SHELL-003 — 설정
- US-SHELL-004 — brew 한 줄로 설치

## Loose Coupling 검증
- FORAGE: `scan()` 호출, `ScanCompleted` 구독 → 알림 표시
- DIGEST: `ApprovedMeal` 전달, 결과 표시
- GROW: 코스튬·대사 조회
- 데이터 소유권: 사용자 설정의 source of truth

## Suggested Bolts
- BOLT-01, BOLT-07, BOLT-08 (`plans/bolts/` 참조)

## NFR
- NFR-A1~A3, D1~D4, U1~U3

## Risk
- R-08, R-11

## Measurement Criteria
- business metric: M-3·M-4 동료 설치·유지
- technical metric: NFR-F3·F5 유휴 CPU·메모리
