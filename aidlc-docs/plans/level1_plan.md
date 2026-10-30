---
artefact_type: plan
task: level1_plan
phase: all
unit: n/a
status: approved
created_at: 2026-10-30
---

# Plan: Level 1 — Cachew 전체 단계

## 목적

intent(`requirements/intent.md`)를 Inception → Construction → Operations로 이어지는 단계로 나누고, 각 단계의 사람 승인 지점을 정한다.

## 입력

- `requirements/intent.md`
- `00-naming.md`

## 단계

- [x] **Phase 0. 이름 정하기**
  - 산출: `00-naming.md` → 캐슈 / Cachew 확정, 첫 커밋 `8136d4b`

- [x] **Phase 1. Inception (Mob Elaboration)** — 세부 plan: `plans/inception_plan.md`
  - 산출: 명확화 답변, 먹이 카탈로그, user story + acceptance criteria, NFR, risk register, measurement criteria, Unit 정의, Bolt 제안
  - 승인 지점: inception_plan 승인 → 명확화 답변 → Unit·Bolt 승인

- [ ] **Phase 2. Construction (Unit별)**
  - Domain Model(DDD) → 사람 review
  - Logical Design + ADR(배포 방식, 권한 모델, 휴지통 처리 같은 결정) → 사람 승인
  - Code + Unit Test → 사람 review
  - Deployment Unit 패키징(서명·공증된 앱, 설치 경로)
  - 반복 단위는 Bolt(시간~일 단위)다

- [ ] **Phase 3. Operations**
  - 동료 배포 승인 → 사용 피드백 수집
  - 오삭제·권한 문제 같은 사고 대응 runbook
  - Bolt closeout, 다음 Bolt 제안

## 기본값으로 둔 결정 (승인 시 확정, 바꾸려면 알려 줄 것)

| # | 기본값 | 근거 |
|---|---|---|
| P1 | MVP는 **메뉴바 앱**이다. 스캔 엔진은 UI와 분리된 모듈로 만들고, **CLI(`cachew scan`)는 후속 Bolt**에서 붙인다 | MVP 범위를 줄이면서 CLI 재사용 경로를 남긴다 |
| P2 | 최소 지원 OS는 **macOS 14**다 | `MenuBarExtra`(`.window` 스타일 포함)는 macOS 13.0부터, `@Observable`은 macOS 14.0부터 쓸 수 있다(Apple 문서 확인). macOS 13까지 내리면 `ObservableObject`를 써야 한다 |
| P3 | 성공 지표 초안: (1) 오삭제 사고 0건, (2) 동료 설치 5명 이상, (3) 1인당 첫 주 확보 용량 중앙값 기록 | 회사 동료가 같이 쓰는 수준이라는 목표에서 도출. 숫자는 Inception의 measurement criteria에서 확정한다 |

## Trade-off / 고려사항

- Inception에서 배포 방식(샌드박스 여부)과 휴지통 처리 방식을 정해야 Construction의 권한 모델이 흔들리지 않는다. 두 가지는 `inception_plan.md` Step 2의 명확화 질문으로 묻는다.

## 사용자 승인 기록

- 요청: 2026-10-30 21:10
- 승인: 2026-10-30, 사용자 승인("승인")
- 메모: 기본값 P1~P3 그대로 확정
