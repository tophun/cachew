---
artefact_type: plan
task: inception
phase: inception
unit: n/a
status: approved
created_at: 2026-10-30
---

# Plan: Inception — 요구사항 분석과 먹이 카탈로그

Your Role: product manager + software architect. 계획을 먼저 승인받고, 승인된 순서대로 한 step씩 실행하며, step이 끝날 때마다 체크박스를 갱신한다. material decision은 단독으로 내리지 않는다.

## 목적

intent를 user story, NFR, 리스크, Unit, Bolt로 바꾸고, 캐슈가 먹을 수 있는 것의 목록(먹이 카탈로그)을 근거와 함께 정리한다.

## 입력

- `requirements/intent.md`
- `plans/level1_plan.md`
- `00-naming.md` (캐릭터·알림 말투)

## 산출물

- 명확화 답변 — `requirements/clarifications.md`
- 먹이 카탈로그 — `requirements/food_catalog.md`
- User story + acceptance criteria — `story-artifacts/<unit>/<story>.md`
- NFR — `risks/nfr.md`
- Risk register — `risks/risk_register.md`
- Measurement criteria — `requirements/measurement.md`
- Unit 정의 — `units/<unit>.md`
- Bolt 제안 — `plans/bolts/<bolt_id>.md`
- (선택) PRFAQ — `requirements/prfaq.md`

## 단계 (Step-by-Step)

- [x] **Step 1. intent 등록과 pathway 식별**
  - 산출: `requirements/intent.md` (green-field)
  - 검증: 한 문장 intent, 킥오프 확정 결정 D1~D8 기록

- [x] **Step 2. 명확화 질문을 묶어서 한 번에 묻기** — 답: `requirements/clarifications.md`
  - 산출: `requirements/clarifications.md`
  - 검증: 아래 세 질문에 사용자 답이 기록됨
  - clarification 필요:
    - **Q1. 배포 방식.** Mac App Store로 배포하려면 App Sandbox를 켜야 한다(Apple 문서). 샌드박스 앱은 사용자가 고른 폴더 밖의 개발 캐시를 마음대로 읽거나 옮길 수 없다. 반면 Developer ID로 서명·공증해 직접 배포(Homebrew cask 등)하면 샌드박스가 필요 없다. 단, 다른 앱의 컨테이너(예: Docker Desktop의 `~/Library/Containers/com.docker.docker`)는 샌드박스가 없어도 macOS 권한 프롬프트가 뜰 수 있다(확인 필요).
    - **Q2. 휴지통으로 보낸 것은 휴지통을 비우기 전까지 디스크 공간을 그대로 차지한다.** 먹어도 공간이 바로 늘지 않는 문제를 어떻게 다룰지 정해야 한다. "N일 뒤 자동 소화(완전 삭제)" 안은 D3(삭제는 승인 후에만)과 충돌할 수 있고, 앱이 휴지통 속 특정 항목만 지우는 데 Full Disk Access가 필요한지 확인되지 않았다 → 고르면 Step 3의 기술 스파이크 대상이 된다.
    - **Q3. 휴지통에 넣을 수 없는 먹이.** Docker 이미지·볼륨, 시뮬레이터 런타임처럼 파일 단위로 휴지통에 넣을 수 없고 각 도구 명령으로만 지워야 하는 항목이 있다. 이들은 복구가 안 되므로 D4(휴지통) 원칙의 예외다.

- [x] **Step 3. 기술 스파이크 (Q2에서 자동 소화를 고른 경우에만)** — 생략: Q2 답이 "휴지통 + 비우기 안내"
  - 산출: `design-artifacts/adr/0001-trash-digestion.md` 초안
  - 검증: `FileManager.trashItem(at:resultingItemURL:)`로 받은 휴지통 경로의 항목을 앱이 지울 수 있는지, Full Disk Access 없이 가능한지 실제 실행 결과로 확인
  - clarification 필요: 없음

- [x] **Step 4. 먹이 카탈로그 작성** — 106행(A 29·B 33·C 24·X 20), 공통 규칙 CR-1~10, 출처 대조 13건 일치
  - 산출: `requirements/food_catalog.md`
  - 열 정의: 생태계 / 먹이(경로 또는 대상) / 재생성 방법(지웠을 때 무엇이 다시 만들어 주는가) / 휴지통 가능 여부 / 권한 필요 여부 / 안전 등급 / 감지 신호(존재·크기·마지막 사용 시각 등) / 제외 조건(예: 커밋 안 된 worktree, 실행 중인 프로세스)
  - 대상 직군: 프론트/Node, iOS, Android, 백엔드(JVM·Go·Rust), Python/ML, Docker, IDE, AI 도구
  - 규칙: 항목마다 **공식 문서 출처**를 단다. 출처가 없으면 "미확인"으로 표시하고 안전 등급을 낮춘다
  - 검증: 모든 행에 출처 또는 미확인 표시가 있고, Q1~Q3 답이 휴지통 가능·권한 열에 반영됨

- [x] **Step 5. User story + acceptance criteria 작성** — 16개(카탈로그 반영으로 US-FORAGE-006 추가)
  - 산출: `story-artifacts/<unit>/<story>.md`
  - 검증: INVEST 점검, 모든 story에 acceptance criteria. D3(승인 후 삭제)·D5(worktree 제외)가 acceptance criteria로 드러남
  - clarification 필요: 코스튬·경험치(D7)의 세부 규칙은 초안을 제시하고 확인받는다

- [x] **Step 6. NFR 정의** — 카탈로그 반영으로 NFR-S8·S9·O3 추가
  - 산출: `risks/nfr.md`
  - 검증: 안전성(오삭제 방지), 프라이버시(D6, 파일 내용 전송 금지), 성능(스캔 시간·CPU), 권한, 배포·업데이트 항목이 있음

- [x] **Step 7. Risk register 작성** — R-01~R-16
  - 산출: `risks/risk_register.md`
  - 검증: 오삭제, 권한 거부, 도구별 경로 변경(버전 차이), 휴지통 공간 오해, 외부 LLM 연결 시 데이터 유출 위험이 식별되고 대응안이 있음

- [x] **Step 8. Measurement criteria 확정** — M-1~M-6
  - 산출: `requirements/measurement.md`
  - 검증: level1_plan의 P3 초안이 intent로 추적 가능한 지표로 확정됨

- [x] **Step 9. Unit 분해** — FORAGE(core)·DIGEST·GROW·SHELL
  - 산출: `units/<unit>.md`
  - 검증: Unit 간 결합이 느슨하고 각 Unit이 독립적으로 build 가능함. 스캔 엔진과 UI가 분리됨(P1)

- [x] **Step 10. Bolt 제안** — BOLT-01~08
  - 산출: `plans/bolts/<bolt_id>.md`
  - 검증: 각 Unit에 Bolt가 1개 이상 있음. 첫 Bolt는 "먹이 몇 종 스캔 + 승인 + 휴지통" 세로 슬라이스

- [ ] **Step 11. (선택) PRFAQ** — 이번에는 쓰지 않음. 동료 배포 직전(BOLT-08)에 쓰는 편이 내용이 정확하다
  - 산출: `requirements/prfaq.md` — 동료에게 "그거 깔아봐"라고 보여줄 한 페이지

- [x] **Step 12. 사용자 승인 기록**
  - 검증: Unit·Bolt에 대한 사용자 승인이 이 파일 아래에 기록됨

## Trade-off / 고려사항

- 먹이 카탈로그(Step 4)를 user story(Step 5)보다 먼저 둔 이유: 휴지통 가능 여부·권한 같은 사실이 story의 acceptance criteria를 바꾸기 때문이다.
- 카탈로그 조사는 직군이 많아 분량이 크다. 승인 후 직군별로 나눠 병렬 조사할 수 있다.

## 사용자 승인 기록

- 요청: 2026-10-30 21:10
- 승인: 2026-10-30, 사용자 승인("승인")
- 메모: 명확화 답 C1 직접 배포, C2 휴지통 + 비우기 안내, C3 복구 불가 먹이 포함(표시·재확인)

### Step 12 — Unit·Bolt 승인

- 요청: 2026-10-30 21:58
- 승인: 2026-10-30 22:03, 사용자 승인("승인")
- 함께 확정: 경험치 규칙 초안대로(1 GB = 10 XP, 휴지통 이동 시 적립, 되돌리면 차감, C·복구 불가 0.5배), 도구 관리 워크트리는 안내만
- BOLT-08 전에 정할 것: 서명 계정 개인/회사(R-11), Intel 지원(NFR-D4)
