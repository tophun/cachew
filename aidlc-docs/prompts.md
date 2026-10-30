# AI-DLC Prompts Log

이 파일은 본 프로젝트 세션에서 사용된 모든 표준 프롬프트를 시간 순으로 누적한다.
각 entry는 `## YYYY-MM-DD HH:mm — <phase> — <task>` 형태의 헤더로 구분한다.

## 2026-10-30 — Phase 0 — 이름 정하기 (킥오프)

사용자 원문:

> # 디스크 정리 캐릭터 앱 — AI-DLC 킥오프 (0단계: 이름 정하기)
>
> ## 목표
> 개발자가 즐겁게 디스크를 확보하는 macOS 앱을 만든다.
> 귀여운 캐릭터가 개발 환경의 불필요한 캐시와 파일을 찾아 제안하고,
> 사용자가 승인한 것만 "먹는다". 회사 동료 개발자들이 같이 쓸 수 있는 수준이 목표다.
> 가칭은 "먹깨비"지만 확정이 아니다.
>
> ## 확정된 결정
> - 플랫폼: macOS 네이티브, Swift + SwiftUI, 메뉴바 앱
> - 주 타겟: 전 개발 직군 (프론트/Node, iOS, Android, 백엔드/JVM/Go/Rust, Python/ML, Docker, IDE, AI 도구)
> - "자동"의 의미: 찾기와 제안만 자동. 삭제는 언제나 사용자 승인 후에만.
> - 먹은 것은 휴지통(복구 가능)으로 보낸다. 커밋 안 된 변경이 있는 worktree는 제안하지 않는다.
> - LLM은 선택 사항: 없음(기본) / 사용자의 에이전트 연결 / 로컬 LLM. 파일 내용은 절대 보내지 않는다.
> - 성장 요소: 먹은 용량만큼 경험치, 레벨 대신 코스튬이 열린다.
>
> ## 0단계: 이름 정하기 (이번 작업은 여기까지만)
> 이름은 앱 이름이자 캐릭터 이름이다. 메뉴바, 알림 문구, 코스튬 세계관이 전부 이 이름에서 나온다.
>
> 1. 기준을 먼저 제시하라. 아래 기준에 빠진 게 있으면 추가해도 된다.
>    - 캐릭터가 "먹는다"는 행동이 바로 떠오를 것
>    - 한국어와 영어 둘 다 자연스러울 것 (영문 표기, 번들 ID, CLI 명령어로 쓸 수 있는지)
>    - 짧을 것 (메뉴바와 알림에 들어감)
>    - 개발자가 피식 웃을 포인트가 있을 것
>    - 동료한테 "그거 깔아봐"라고 말하기 쉬울 것
> 2. 이름 후보 10~15개를 방향별로 나눠 제안하라.
>    - 먹는 캐릭터 계열 (가칭 "먹깨비" 포함)
>    - 개발 밈 계열 (캐시, 가비지 컬렉션, node_modules 등)
>    - 짧은 영문 브랜드 계열
> 3. 후보마다 아래를 적어라.
>    - 한글 표기, 영문 표기, 뜻이나 유래
>    - 캐릭터 한 줄 설정
>    - 알림 문구 예시 1개 (예: "배고파요… 12GB 먹어도 돼요?")
>    - CLI 명령어 예시 (예: `mukk scan`)
> 4. 이미 쓰는 이름인지 확인하라: Mac App Store, GitHub, 같은 이름의 유명 앱.
>    확인한 것과 확인 못 한 것을 구분해서 적어라.
> 5. 기준표로 점수를 매겨 상위 3개를 추천하고, 추천 이유를 적어라.
>
> 결과는 `aidlc-docs/00-naming.md`에 남기고, 여기서 멈춰라.
> 내가 이름을 고르면 그다음에 Inception(요구사항 분석, 먹이 카탈로그)을 시작한다.

이어진 지시:

- "일단 github 레포부터 생성하고 clone부터하자" → 질문 결과: 이름부터 정하기, `tophun` 계정 Public, `~/paseo/<레포명>`에 clone, 개입 깊이 LIGHT
- (1차 후보 13개 보고 후) "캐슈 나쁘지 않은데 이런 류로 좀 더 찾아보자" → 2차 캐슈류 16개 조사
- 이름 선택: 캐슈 / Cachew → `tophun/cachew` 생성·clone, 문서 이동

## 2026-10-30 21:07 — Inception — 착수

- "ㅇㅇ" → 질문 결과: "커밋·push 후 Inception" 선택
- 네이밍 문서 첫 커밋 `8136d4b` push
- 적용한 표준 프롬프트: Setup Prompt(워크스페이스 초기화), Inception — User Stories / Units의 plan-first 패턴
- 산출: `requirements/intent.md`, `plans/level1_plan.md`, `plans/inception_plan.md` (승인 대기)


## 2026-10-30 — Inception — 명확화 답변·계획 승인

- 질문 결과: 배포 "직접 배포", 휴지통 공간 "휴지통 + 비우기 안내", 휴지통 불가 먹이 "포함, 복구 불가 표시", Inception 계획 "승인"
- 산출: `requirements/clarifications.md`

## 2026-10-30 21:58 — Inception — Step 4~10 실행

- 먹이 카탈로그: 직군 4묶음(프론트·Python / iOS·Android / 백엔드·Docker / IDE·AI·공통)을 같은 브리프로 병렬 조사. 브리프 핵심: 열 정의, 등급 A/B/C/X, "공식 출처 또는 미확인 + 등급 하향", 파일 삭제·이동과 로컬 파일 시스템 조회 금지
- 교차 검증: 판단을 바꾸는 출처 13건을 원문에서 다시 확인(13건 일치, WWDC23 세션 스크립트 포함)
- 카탈로그 결과 반영: NFR-S8·S9·O3, R-04·R-08 갱신, R-14~R-16, US-FORAGE-006, BOLT-01 먹이 3종
- 적용한 표준 프롬프트: Inception — User Stories, Units, NFR·Risk 패턴(plan-first, step별 체크)
- 산출: `requirements/food_catalog.md`, `story-artifacts/*`, `risks/nfr.md`, `risks/risk_register.md`, `requirements/measurement.md`, `units/*`, `plans/bolts/*`

## 2026-10-30 22:03 — Inception — Step 12 승인

- 질문 결과: Unit·Bolt 구성 "승인", 경험치 규칙 "초안대로", 도구 관리 워크트리 "안내만"
- Inception 종료. 다음: Construction — BOLT-01 Domain Design plan
