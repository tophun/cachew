---
artefact_type: bolt
id: BOLT-01
units: [FORAGE, DIGEST, SHELL]
status: approved
estimate: 1~2일
---

# BOLT-01: 첫 세로 슬라이스: 찾기 → 승인 → 휴지통

- Unit: FORAGE, DIGEST, SHELL
- 예상 기간: 1~2일 (Bolt 단위)
- 대상 story: US-FORAGE-001(일부), US-FORAGE-003(일부), US-FORAGE-004(일부: 먹이 3종의 실행 중 판정), US-DIGEST-001, US-SHELL-001(일부)

## 할 일
- [ ] 카탈로그 데이터 형식(버전 포함) 정의, 먹이 3종만 등록(아래 표)
- [ ] 기본 경로 스캔 + 크기 계산 + allowlist/deny-list/symlink 검사. 경로는 전체 일치(NFR-S8)이고, 이웃 X 경로(`~/.npmrc`, `~/.gradle/gradle.properties`) 픽스처로 거부를 테스트
- [ ] 경로는 도구에 먼저 묻는다(NFR-O3): `brew --cache`. 메뉴바 앱에서 `brew`를 찾는지, 셸과 같은 값을 받는지 실측(R-04)
- [ ] 3종에 한한 최소 "실행 중" 판정(npm·brew 프로세스, Gradle 데몬): 실행 중이면 그 먹이 제외(CR-5)
- [ ] `MenuBarExtra` 패널에 목록·체크·먹기 버튼
- [ ] 실행 직전 재검증 → 휴지통 이동 → 먹은 기록 저장
- [ ] 검증 스파이크: R-07(앱이 휴지통으로 보낸 항목의 Finder '되돌려 놓기' 동작), NFR-F1·F3·F5 실측

## 먹이 3종 (카탈로그에서 선정)

| 먹이 | 등급 | 경로 | 고른 이유 |
|---|---|---|---|
| npm 캐시 | A | `~/.npm/_cacache` | 프론트·RN 동료 대부분이 가짐. 휴지통 가능, 권한 없음, 공유 블록 없음 |
| Homebrew 다운로드 캐시 | A | `brew --cache` (기본 `~/Library/Caches/Homebrew`) | 직군과 무관하게 흔함. 도구에 경로를 묻는 흐름(CR-3)과 R-04를 첫 Bolt에서 검증 |
| Gradle 의존성 캐시 | B | `~/.gradle/caches/modules-2` | Android·백엔드 공용, 대용량. B 표시("다시 만들려면 시간이 걸림")와 실행 중 데몬 제외를 검증 |

제외한 후보: DerivedData(출처 미확인으로 C, R-16), pnpm 스토어(공유 블록, CR-6), uv 캐시(휴지통 불가, V-1), Go 모듈 캐시(읽기 전용 파일이라 휴지통 경로가 까다로움).

## 완료 조건
- 대상 story의 acceptance criteria 중 이 Bolt 범위 항목이 테스트로 확인됨
- 사람 review 완료
