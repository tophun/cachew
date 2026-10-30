---
artefact_type: unit
id: UNIT-GROW
intent: requirements/intent.md
status: approved
bolts: [BOLT-06]
---

# Unit: 성장 (Grow)

## Mission
먹은 용량을 경험치로 바꾸고, 레벨 대신 코스튬을 연다(D7). 캐릭터 대사(한국어·영어)를 관리해 알림과 UI에 공급한다.

## DDD 위치
- subdomain: supporting
- bounded context: Character Growth

## 포함 User Story
- US-GROW-001 — 먹은 만큼 경험치
- US-GROW-002 — 경험치로 코스튬 열기

## Loose Coupling 검증
- DIGEST의 `FoodEaten`/`FoodRestored` 이벤트만 구독한다(비동기). DIGEST를 직접 호출하지 않는다.
- SHELL에 현재 코스튬·경험치·대사를 제공한다.
- 데이터 소유권: 경험치·코스튬 상태의 source of truth. 먹은 기록에서 다시 계산할 수 있다

## Suggested Bolts
- BOLT-06 (`plans/bolts/` 참조)

## NFR
- NFR-P3, U1

## Risk
- R-10

## Measurement Criteria
- business metric: M-5 즐거움(또 쓰고 싶다)
- technical metric: —
