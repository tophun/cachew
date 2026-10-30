# 0단계: 이름 정하기

- 작성일: 2026-10-30
- 2차 추가: 2026-10-30 — "캐슈 같은 결로 더 찾기" 요청에 따라 캐슈류 후보 16개 추가(6절)
- 상태: **확정 — 캐슈 / Cachew** (2026-10-30 사용자 선택). 다음은 Inception.
- AI-DLC plan gate: 사용자 킥오프 브리프의 1~5단계를 승인된 plan으로 간주하고 별도 승인 라운드는 생략했다.
- 레포: [`tophun/cachew`](https://github.com/tophun/cachew) (Public), 로컬 `~/paseo/cachew`. 이 문서는 레포 생성 전 임시 위치(`~/paseo/tophun/aidlc-docs/`)에서 작성한 뒤 옮겼다.

이 이름은 앱 이름이자 캐릭터 이름이다. 메뉴바, 알림 문구, 코스튬 세계관이 모두 여기서 나온다.

---

## 1. 기준

사용자 기준 5개(C1~C5)에 3개(C6~C8)를 더했다. 모두 1~5점, 같은 가중치다. 사용자 기준만의 소계도 따로 계산해 순위가 달라지는지 확인한다(4절).

| # | 기준 | 출처 | 5점에 가까운 경우 |
|---|---|---|---|
| C1 | "먹는다"가 바로 떠오름 | 사용자 | 한국어 화자와 영어 화자 모두 이름만 듣고 먹는 행동을 떠올림 |
| C2 | 한국어·영어 모두 자연스러움, 식별자로 쓸 수 있음 | 사용자 | 한글 표기·영문 표기가 둘 다 어색하지 않고, 번들 ID·CLI에 그대로 씀 |
| C3 | 짧음 | 사용자 | 한글 2음절 이하이면서 영문 6자 이하 = 5, 한글 3음절 또는 영문 7~8자 = 4 |
| C4 | 개발자가 피식 웃음 | 사용자 | 개발 용어와 먹는 행동이 한 단어 안에서 겹침 |
| C5 | "그거 깔아봐"라고 말하기 쉬움 | 사용자 | 말로 듣고 철자를 바로 칠 수 있고, 말하기 머뭇거려지지 않음 |
| C6 | 고유성(충돌 위험 낮음) | 추가 | 아래 3절 충돌 확인 결과로 채점 |
| C7 | 안심 톤 | 추가 | "승인받은 것만 휴지통으로" 원칙과 맞음. 파괴·도둑·더러움 이미지가 없음 |
| C8 | 세계관·코스튬 확장성 | 추가 | 캐릭터 종족·직업에서 코스튬이 자연스럽게 여러 벌 나옴 |

C6 채점표(증거 기반):

| 점수 | 조건 |
|---|---|
| 5 | 같은 이름·접두 이름의 앱, 레포, brew 패키지가 모두 없음 |
| 4 | 같은 이름 앱 없음. GitHub에 소규모(★50 미만) 동명 계정·레포만 있음 |
| 3 | 다른 분야에 같은 이름 앱 1~4개, 또는 개발 생태계(GitHub·PyPI·npm·crates)에 ★1k 미만 동명 프로젝트 |
| 2 | 같은 접두 이름 앱이 5개 이상이거나, 일반어·사람 이름이라 검색으로 찾기 어렵거나, 같은 이름의 유명 앱·게임이 있음 |
| 1 | 같은 카테고리 직접 충돌, ★1k 이상 개발 프로젝트, brew CLI 충돌, 활성 국내 브랜드 중 하나라도 해당 |

※ 2차 조사(6절)에서 2점에 "같은 이름의 유명 앱·게임"을 추가하고, 3점의 앱 개수 구간을 1~2개에서 1~4개로 넓혔다. 1차 점수에는 영향이 없다(Nom의 Om Nom 캐릭터는 brew 충돌로 이미 1점).

---

## 2. 후보 13개

### A. 먹는 캐릭터 계열

| 이름 (한글 / 영문) | 뜻·유래 | 캐릭터 한 줄 설정 | 알림 문구 예시 | CLI 예시 |
|---|---|---|---|---|
| 먹깨비 / Mukkaebi (가칭) | 먹다 + 도깨비 | 밤마다 디스크 구석을 뒤지는 배고픈 꼬마 도깨비. 방망이 대신 숟가락을 든다 | "배고파요… DerivedData 12GB 먹어도 돼요?" | `mukk scan` |
| 냠냠이 / Nyamnyam | 먹는 소리 '냠냠' + 이 | 뭐든 맛있게 먹는 말랑한 젤리 | "냠냠할 거 찾았어요! node_modules 8.3GB, 먹어도 될까요?" | `nyam scan` |
| 오물이 / Omul | '오물오물'(씹는 모양) + '오물'(쓰레기)의 이중 의미 | 남이 버린 것을 오물오물 씹어 치우는 작은 청소 생물 | "오물오물… Gradle 캐시 5GB 씹어도 돼요?" | `omul scan` |
| 먹방 / Mukbang | 먹는 방송. 영어권에서도 통하는 단어 | 매일 캐시로 '오늘의 메뉴'를 고르는 먹방 스트리머 | "오늘의 먹방 메뉴는 Docker 이미지 20GB! 방송 켜도 될까요?" | `mukbang menu` |

### B. 개발 밈 계열

| 이름 (한글 / 영문) | 뜻·유래 | 캐릭터 한 줄 설정 | 알림 문구 예시 | CLI 예시 |
|---|---|---|---|---|
| 캐슈 / Cachew | cache + chew. 캐슈넛 말장난 | 사람들이 잊어버린 캐시를 찾아 오독오독 씹는 다람쥐. 다람쥐는 원래 먹이를 '캐싱'하는 동물이다 | "잊어버린 캐시 발견! ~/.npm 4.2GB, 오독오독 해도 돼요?" | `cachew scan` |
| 가비 / Gabi | 가비지 컬렉터(GC)의 애칭 | 아무도 참조하지 않는 파일만 골라 수거하는 꼼꼼한 수거반장 | "참조 0개인 친구들 발견. Simulator 런타임 9GB, 수거해도 될까요?" | `gabi gc` |
| 니블 / Nibble | nibble(4비트) + nibble(조금씩 갉아먹다) | 승인받은 만큼만 조금씩 갉아먹는 햄스터 | "한 입만요… .gradle 캐시 3GB 갉아먹어도 돼요?" | `nibble scan` |
| 촘프 / Chomp | chomp(덥석 물다) + Perl·Ruby의 `chomp`(끝 문자 잘라내기) | 문자열 끝 개행까지 깨물어 먹는 악어 | "덥석 할 준비 됐어요. Xcode Archives 6GB, 물어도 돼요?" | `chomp scan` |
| 바이티 / Bytey | byte + bite | 바이트를 한 입(bite)씩 먹는 작은 로봇 | "1바이트도 안 남길게요. pip 캐시 2.1GB 먹어도 될까요?" | `bytey scan` |

### C. 짧은 영문 브랜드 계열

| 이름 (한글 / 영문) | 뜻·유래 | 캐릭터 한 줄 설정 | 알림 문구 예시 | CLI 예시 |
|---|---|---|---|---|
| 고블린 / Gobblin | gobble(게걸스럽게 먹다) + goblin. 먹깨비의 영어판 | `~/Library` 동굴에서 보물 대신 캐시를 먹는 고블린 | "~/Library/Caches에서 먹을 걸 찾았어요. 7GB, 꿀꺽해도 돼요?" | `gobblin scan` |
| 먼치 / Munchy | munch(우적우적 먹다) + y | 늘 뭔가를 우물거리는 털뭉치 | "우적우적 할 시간! Cargo target 폴더 11GB, 먹어도 돼요?" | `munchy scan` |
| 크럼 / Crumb | 부스러기. breadcrumbs | 빌드가 남긴 부스러기를 줍는 작은 새 | "빌드 부스러기 4GB를 모았어요. 치워도 될까요?" | `crumb scan` |
| 놈 / Nom | nom nom(먹는 소리). node_modules를 비튼 nom_modules | node_modules만 보면 침을 흘리는 블랙홀 젤리 | "nom_modules 발견! 프로젝트 14개, 9.8GB… nom nom 해도 돼요?" | `nom scan` |

모든 알림 문구는 "먹어도 돼요?" 형태로 끝난다. 삭제는 언제나 승인 후에만 한다는 확정 결정을 캐릭터 말투에 담기 위해서다.

---

## 3. 이미 쓰는 이름인지 확인

### 3.1 확인한 것 (2026-10-30 기준)

| 확인 대상 | 방법 |
|---|---|
| Mac App Store | iTunes Search API `entity=macSoftware`, 미국·한국 스토어, 영문 표기와 한글 표기 각각 |
| iOS App Store(같은 이름 유명 앱) | iTunes Search API `entity=software`, 미국·한국 스토어 |
| GitHub | `gh api users/<이름>`(계정 선점), `search/repositories?q=<이름>+in:name`(레포 수, 별 상위 3개) |
| Homebrew | `brew info --formula/--cask <이름>` |
| 로컬 명령 충돌 | `command -v <이름>` (13개 모두 없음) |
| 패키지 레지스트리 | PyPI·npm·crates.io (상위 후보와 일부 후보만) |
| 웹 검색 | 먹깨비, Cachew, Nibble, Mukbang 4건 |

후보별 결과:

| 후보 | Mac App Store | iOS App Store | GitHub | brew·패키지 | 기타 | C6 |
|---|---|---|---|---|---|---|
| 먹깨비 | 없음 | KR: `먹깨비 - 할인에 지역화폐를 더하다`(Food & Drink, 평점 1,204개, 2018년 출시), `[업주용] 먹깨비 사장님`, `먹깨비 프렌즈`. 판매자는 모두 MUKKEBI | 계정 `mukkaebi` 없음, 레포 2개(최대 ★5). 계정 `mukk`는 선점(User) | brew·PyPI·npm·crates 모두 없음(`mukk`, `mukkaebi`) | ㈜먹깨비가 운영하는 **공공배달앱**. 전남·제주·양양 등에서 운영 중이고 하나은행·BNK경남은행과 제휴 | **1** |
| 냠냠이 | 없음 | US: `NyamNyam: Recipe Organizer` | 계정 `nyamnyam` 선점(레포 0개), 레포 197개(최대 ★3) | npm `nyam`(Yammer CLI), npm `nyamnyam`(템플릿). brew·PyPI·crates 없음 | | 3 |
| 오물이 | 없음 | 없음(US/KR, 한글 '오물이' 0건) | 계정 `omul` 선점(레포 5개), `binaryplease/omul` ★6 | brew·PyPI·npm·crates 모두 없음 | 영어 omul은 바이칼호 물고기 이름 | 4 |
| 먹방 | 없음 | `Mukbang …`으로 시작하는 ASMR 게임 다수(US/KR 각 5개 이상) | 계정 `Mukbang` 선점(레포 0개), 레포 129개(최대 ★4) | npm `mukbang` 선점. brew·PyPI·crates 없음 | 웹 검색에서 같은 이름의 Mac 앱·개발 도구는 찾지 못함. 일반어 | 2 |
| 캐슈 | 없음 | KR: `캐슈`(Shopping, 2026-10-25 출시, 평점 0개) | 계정 `CacheW` 선점, `karlicoss/cachew` ★238(Python 캐시 라이브러리) | PyPI `cachew`(위 라이브러리), npm `cachew`(Node 캐싱). brew·crates 없음 | Maven `io.github.rjjingar:cachew`(Java 캐시 라이브러리). 같은 말장난을 캐시 라이브러리들이 이미 쓰고 있음 | 3 |
| 가비 | 없음 | `Gabi: Learn Korean Hangul`, `Gabi Assistant`, `Gabi: 카페 경영 시뮬레이터` 등. KR '폰가비', '가비야' | 계정 `gabi` 선점, 레포 12,322개 | brew 없음. 패키지 미확인 | 사람 이름이라 검색에서 묻힘 | 2 |
| 니블 | 동명 없음(`NibbleKeys`, `Nibbles: Tasty Video Converter` 유사) | `Nibble: …`으로 시작하는 앱 5개 이상(학습, 칼로리, 반려동물 등) | 계정 `nibble` 선점, 레포 2,258개(`IanVS/eslint-nibble` ★888, `rooklift/nibbler` ★758) | PyPI·npm·crates 모두 `nibble` 선점. brew 없음 | 같은 이름의 Mac 디스크 정리 앱은 웹 검색에서 찾지 못함 | 2 |
| 촘프 | 동명 없음(`Number Chomp`) | `Chomp` 동명 앱 포함 다수 | 조직 `chomp` 선점, 레포 2,312개(`sledgeh4w/chomper` ★635) | brew 없음. 패키지 미확인 | | 2 |
| 바이티 | 없음 | `Bytey: AI robot for kids` | 계정 `bytey` 선점, 레포 57개(최대 ★4) | brew 없음. 패키지 미확인 | | 3 |
| 고블린 | 없음 | 동명 없음. KR '고블린' 게임 다수 | **`apache/gobblin` ★2,270**(Apache 데이터 수집 프레임워크) | brew 없음 | 개발자 대상 앱에서 Apache 프로젝트명과 겹침 | **1** |
| 먼치 | 없음 | `Munchy` 동명 앱 포함 다수 | 계정 `munchy` 선점, 레포 271개 | brew 없음. 패키지 미확인 | | 2 |
| 크럼 | **`Crumbs: Storage Cleaner`**(Utilities, 2026-10-22 출시) | `Crumb: …` 다수 | 조직 `Crumb` 선점, 레포 7,832개 | brew 없음 | 같은 플랫폼·같은 카테고리 직접 충돌. **회피 권고** | **1** |
| 놈 | 없음 | `Nom: …` 다수, Cut the Rope의 `Om Nom` 캐릭터 | `rust-bakery/nom` ★10,485 | **brew formula `nom`**(터미널 RSS 리더) | CLI `nom` 설치 충돌. **회피 권고** | **1** |

### 3.2 확인 못 한 것

- 상표권: KIPRIS(한국)와 USPTO(미국)는 조회하지 않았다. 특히 먹깨비는 운영 법인이 있어 상표 등록 가능성이 높아 보이지만, 이는 추론이고 확인하지 않았다.
- 도메인, Google Play, Setapp, Product Hunt.
- 패키지 레지스트리: 가비·촘프·바이티·고블린·먼치·크럼·놈은 PyPI·npm·crates를 조회하지 않았다(놈은 brew 충돌만 확인).
- 실행 파일 충돌: PyPI·npm의 같은 이름 패키지가 실제로 같은 이름의 CLI 실행 파일을 설치하는지는 확인하지 않았다.
- 한계: iTunes Search API는 정확 일치 조회가 아니라 검색(상위 50건)이다. "결과 없음"이 "앱 없음"을 보장하지 않는다. 한국 스토어 조회는 429(rate limit)를 받은 뒤 재시도한 결과다. GitHub 레포 수는 이름 부분 일치 개수다.

### 3.3 참고: 같은 카테고리의 기존 앱

웹 검색에서 개발자용 Mac 디스크 정리 앱이 여럿 확인됐다: `Dev Disk Hygiene`, `DevPowerCleanup`, `CodeCleaner – Dev Disk Cleanup`, `CleanMyCodeMac`(오픈소스), `Crumbs: Storage Cleaner`. 이름이 대부분 Dev·Code·Disk·Clean 같은 설명형이라서, 캐릭터 이름이 차별점이 될 수 있다는 것이 이번 판단이다(근거는 위 목록뿐인 추론).

---

## 4. 점수표

점수는 C3(측정값)와 C6(3절 증거)을 빼면 판단에 따른 값이다. 마음에 안 드는 칸은 고쳐서 다시 합산하면 된다.

| 후보 | C1 먹기 | C2 한·영 | C3 짧음 | C4 피식 | C5 구전 | **소계 (사용자 5)** | C6 고유 | C7 안심 | C8 세계관 | **합계 (8)** |
|---|---|---|---|---|---|---|---|---|---|---|
| 캐슈 / Cachew | 3 | 5 | 5 | 5 | 3 | **21** | 3 | 5 | 4 | **33** |
| 먹방 / Mukbang | 5 | 5 | 4 | 3 | 5 | **22** | 2 | 4 | 4 | **32** |
| 니블 / Nibble | 3 | 4 | 5 | 4 | 4 | **20** | 2 | 5 | 3 | **30** |
| 냠냠이 / Nyamnyam | 5 | 3 | 4 | 1 | 4 | 17 | 3 | 5 | 3 | 28 |
| 먹깨비 / Mukkaebi | 5 | 3 | 4 | 2 | 3 | 17 | 1 | 4 | 5 | 27 |
| 가비 / Gabi | 1 | 4 | 5 | 4 | 4 | 18 | 2 | 4 | 3 | 27 |
| 오물이 / Omul | 3 | 3 | 4 | 3 | 3 | 16 | 4 | 2 | 3 | 25 |
| 먼치 / Munchy | 3 | 4 | 5 | 1 | 3 | 16 | 2 | 4 | 3 | 25 |
| 촘프 / Chomp | 3 | 2 | 5 | 4 | 2 | 16 | 2 | 3 | 3 | 24 |
| 바이티 / Bytey | 2 | 3 | 4 | 3 | 2 | 14 | 3 | 4 | 3 | 24 |
| 크럼 / Crumb | 2 | 3 | 5 | 3 | 3 | 16 | 1 | 4 | 2 | 23 |
| 놈 / Nom | 3 | 2 | 5 | 4 | 2 | 16 | 1 | 3 | 2 | 22 |
| 고블린 / Gobblin | 2 | 4 | 4 | 2 | 2 | 14 | 1 | 2 | 4 | 21 |

채점 메모:

- 영문 중심 후보(Cachew, Nibble, Chomp, Munchy, Nom)의 C1은 3 이하로 줬다. 영어 화자에게는 바로 먹는 단어지만 한국어로 '캐슈', '니블'을 들으면 먹는 행동이 바로 떠오르지 않는다.
- 오물이는 C6이 가장 깨끗한 편이지만, '오물'이 먼저 들려서 C7(안심 톤)과 C5(말하기 머뭇거림)에서 감점했다. 쓰레기를 먹는 캐릭터와 정확히 맞는 말장난이라는 장점과 "내 파일을 오물이라 부른다"는 리스크가 함께 있다.
- 먹깨비는 C8(도깨비 코스튬)이 최고점이지만, C6에서 활성 국내 브랜드와 겹친다. 영문 표기도 흔들린다(로마자 표기법으로는 Meokkkaebi, 배달앱 판매자명은 MUKKEBI).

순위 민감도: 8개 합계와 사용자 기준 5개 소계 모두 상위 3개는 **캐슈·먹방·니블**로 같다. 순서만 다르다(합계: 캐슈 > 먹방 > 니블, 소계: 먹방 > 캐슈 > 니블). 4위는 합계로는 냠냠이, 소계로는 가비다.

---

## 5. 추천 상위 3개

번들 ID는 개인 계정 레포 기준의 예시다. 번들 ID는 처음 배포한 뒤에는 바꾸기 어려우므로, 회사 조직으로 옮길 계획이 있다면 첫 배포 전에 prefix를 확정해야 한다.

### 1위. 캐슈 / Cachew — 합계 33, 소계 21

- 번들 ID 예시: `com.tophun.cachew` / CLI: `cachew scan`, `cachew chew`
- 추천 이유: 개발 농담(cache + chew)이 이름 자체에 들어 있는 유일한 후보다. 한글 '캐슈'도 캐슈넛으로 익숙하다. 다람쥐는 먹이를 '캐싱'하는 동물이라 캐릭터 설정을 따로 설명할 필요가 없다. 도토리·계절 코스튬으로도 쉽게 이어진다. 귀여운 소동물이라 "승인받고 먹는다"는 안심 톤과도 맞는다.
- 약점: 말로 들으면 `cashew`로 칠 수 있다. 같은 말장난을 쓰는 캐시 라이브러리(PyPI·npm, GitHub ★238)가 있어 개발자 검색 결과에 섞인다. 한국 iOS에 2026-10-25 출시된 쇼핑 앱 `캐슈`가 있다.

### 2위. 먹방 / Mukbang — 합계 32, 사용자 기준 소계로는 1위(22)

- 번들 ID 예시: `com.tophun.mukbang` / CLI: `mukbang scan`, `mukbang menu`
- 추천 이유: 먹는 행동이 가장 직접적이고, 한국어와 영어에서 모두 그대로 통한다. "디스크 먹방 앱 깔아봐"는 설명이 필요 없다. '오늘의 메뉴', '구독', '리액션' 같은 방송 세계관이 알림 문구와 코스튬으로 자연스럽게 이어진다.
- 약점: 일반어라 App Store와 웹 검색에서 먹방 ASMR 게임들에 묻힌다. 일반어는 상표로 보호받기 어려울 가능성이 있다(추론, 미확인). npm `mukbang` 이름은 이미 선점돼 있다. 또 '먹방'은 행위를 가리키는 말이라 "앱 이름 = 캐릭터 이름" 전제에서는 캐릭터를 부르는 이름으로 어색하다("먹방아, 먹어"보다는 "먹방 켜기"에 가깝다).

### 3위. 니블 / Nibble — 합계 30, 소계 20

- 번들 ID 예시: `com.tophun.nibble` / CLI: `nibble scan`
- 추천 이유: '4비트'와 '조금씩 갉아먹다'의 이중 의미가 있다. "승인받은 만큼만 조금씩 먹는다"는 제품 원칙과 톤이 가장 잘 맞는다. 영어 화자에게는 바로 먹는 단어다.
- 약점: 고유성이 낮다. iOS에 `Nibble:`로 시작하는 앱이 5개 이상 있고 PyPI·npm·crates 이름도 모두 선점돼 있다. 한국어 화자에게 '니블'은 먹는 말로 바로 들리지 않는다.

### 가칭 먹깨비에 대한 판단

캐릭터성과 세계관(도깨비 코스튬)은 13개 중 가장 좋지만, ㈜먹깨비의 공공배달앱이 활발히 운영 중이다(평점 1,204개, 은행 제휴, 지자체 확산). Public 레포와 동료 배포만으로도 "먹깨비 깔아봐"라고 하면 배달앱이 먼저 검색된다. 먹깨비를 살리고 싶다면 KIPRIS 상표 조회를 먼저 해야 한다.

---

## 6. 2차 후보 — 캐슈류

사용자 피드백: "캐슈 나쁘지 않은데 이런 류로 좀 더 찾아보자." 여기서 "이런 류"는 아래 세 가지로 해석했다. 해석이 다르면 알려 주면 된다.

1. 개발 용어가 음식 이름이나 먹는 말 속에 숨어 있다(캐슈 안에 cache와 chew).
2. 결과물이 한국어와 영어 모두에서 실제 단어처럼 들린다.
3. 귀여운 캐릭터가 바로 그려진다.

채점과 충돌 확인 방법은 1~4절과 같다. 단, 일반어는 1차의 먹방과 같은 규칙으로 C6을 2점 이하로 제한했다.

### 6.1 후보 16개

A. 음식 이름 속에 개발 용어가 숨은 형

| 이름 (한글 / 영문) | 뜻·유래 | 캐릭터 한 줄 설정 | 알림 문구 예시 | CLI 예시 |
|---|---|---|---|---|
| 템푸라 / Tempura | temp(임시 파일) + 템푸라(튀김) | 임시 파일을 바삭하게 튀겨 먹는 새우튀김. 튀김옷이 곧 코스튬이다 | "임시 파일 3.2GB, 바삭하게 튀겨 먹어도 돼요?" | `tempura fry` |
| 덤플링 / Dumpling | dump(코어 덤프·힙 덤프·DB 덤프) + 만두 | 덤프 파일로 만두를 빚어 먹는 만두 요정 | "크래시 덤프 1.8GB로 만두 빚어도 돼요?" | `dumpling scan` |
| 브루니 / Brewnie | Homebrew + brownie. 브라우니는 초콜릿 디저트이자, 밤에 몰래 집안일을 해 주고 음식을 받는 스코틀랜드 민담 속 집요정이다 | 모두 잠든 밤에 디스크를 둘러보고, 허락받은 것만 먹고 가는 집요정 | "밤새 치울 거 찾아놨어요. Homebrew 캐시 2.4GB, 먹어도 돼요?" | `brewnie scan` |
| 팝콘 / Popcorn | `pop()` + kernel(커널 = 옥수수 알) | 커널을 하나씩 팝 해서 먹는 꼬마 팝콘 기계 | "팡! Docker 레이어 15GB, 튀겨 먹어도 돼요?" | `popcorn pop` |
| 스낵바 / Snackbar | Material Design의 Snackbar(하단 알림) + 스낵바(매점). 메뉴바 앱이라 menubar → snackbar 말장난도 된다 | 메뉴바에 매점을 차린 주인장. 남은 간식(캐시)은 손님 허락을 받고 정리한다 | "오늘의 간식: pip 캐시 2.1GB. 먹어도 될까요?" | `snackbar scan` |
| 피스타캐시 / Pistacache | pistachio + cache. 캐슈의 형제 견과 | 캐시 껍질을 딱 까서 먹는 다람쥐 | "딱! 잊어버린 캐시 껍질 깠어요. Gradle 캐시 5GB, 먹어도 돼요?" | `pistacache scan` |
| 키블 / Kibble | KiB(키비바이트) + kibble(반려동물 건사료) | 바이트를 사료처럼 먹는 강아지. 반려견 옷이 곧 코스튬이다 | "밥 시간이에요! node_modules 8GB, 먹어도 돼요?" | `kibble scan` |

B. 먹는 말 + 개발 용어 합성형

| 이름 (한글 / 영문) | 뜻·유래 | 캐릭터 한 줄 설정 | 알림 문구 예시 | CLI 예시 |
|---|---|---|---|---|
| 바이트코드 / Bitecode | bytecode + bite | 바이트코드를 한 입씩 씹어 먹는 JVM 로봇 | "한 입씩 먹을게요. ~/.m2 저장소 6GB, 먹어도 돼요?" | `bitecode scan` |
| 기가바이트 / Gigabite | gigabyte + bite | 한 입에 1기가씩 먹는 대식가 공룡 | "한 입에 12기가! DerivedData, 먹어도 돼요?" | `gigabite scan` |
| 펙키지 / Peckage | peck(쪼아 먹다) + package | 안 쓰는 패키지 캐시를 콕콕 쪼아 먹는 병아리 | "콕콕! 안 쓰는 패키지 캐시 7GB, 쪼아 먹어도 돼요?" | `peckage scan` |
| 디바우어 / Devour | dev + devour(먹어치우다) | 개발 찌꺼기만 골라 먹어치우는 대식가 | "DEV 찌꺼기 18GB 발견! 먹어치워도 돼요?" | `devour scan` |
| 칩멍크 / Chipmunch | chip + munch → chipmunk(줄무늬다람쥐) | 볼주머니 가득 캐시를 채워 먹는 줄무늬다람쥐 | "볼이 터지겠어요… 시뮬레이터 9GB, 먹어도 돼요?" | `chipmunch scan` |
| 스낵샷 / Snackshot | snack + snapshot | 오래된 스냅샷을 과자처럼 와삭 먹는 꼬마 사진사 | "오래된 스냅샷 3개, 간식으로 먹어도 돼요?" | `snackshot scan` |
| 캐시보어 / Cachivore | cache + -vore(carnivore처럼 '~을 먹는 동물') | 도감에 막 등록된 신종 '캐시식 동물' | "캐시식 동물 출현! ~/Library/Caches 7GB, 먹어도 돼요?" | `cachivore scan` |
| 니빗 / Nibbit | nibble + bit. 개구리 울음 ribbit과도 운이 맞는다 | 비트를 조금씩 갉아먹는 아기 개구리 | "조금씩만요… .gradle 캐시 3GB 먹어도 돼요?" | `nibbit scan` |
| 캐츄 / Cachu | cache + 츄(츄릅) | 캐시를 츄릅 빨아 먹는 말랑한 아기 괴물 | "츄릅… ~/.npm 4.2GB, 먹어도 돼요?" | `cachu scan` |

### 6.2 이미 쓰는 이름인지 확인 (2026-10-30 기준)

방법은 3.1과 같다(App Store 미국·한국 × Mac·iOS, 한글 표기는 한국 스토어, GitHub, brew, PyPI·npm·crates, 로컬 PATH). 로컬 PATH 충돌은 16개 모두 없었다.

| 후보 | Mac App Store | iOS App Store | GitHub | brew·패키지 | 기타 | C6 |
|---|---|---|---|---|---|---|
| 템푸라 | 없음 | `Tempura 2 Go`, `Tempura Man` | 계정 선점, 레포 389개(`BendingSpoons/tempura-swift` ★713 — Swift 앱 아키텍처 프레임워크, `lukeed/tempura` ★533) | PyPI·npm·crates 선점. brew 없음 | 일반어(음식) | 2 |
| 덤플링 | 없음 | `Dumpling …` 스퀴시·ASMR 앱 5개 이상, KR `덤플링 스퀴시 컬렉터` | 계정 선점, 레포 1,580개(`pingcap/dumpling` ★281) | **brew formula `dumpling`**(MySQL 덤프 도구). PyPI·npm·crates 선점 | CLI 설치 충돌 | **1** |
| 브루니 | 없음 | 없음(한글 '브루니'도 없음) | 계정 없음, 레포 1개(★0) | 모두 없음 | 웹 검색에서 같은 이름 앱 없음 | 4 |
| 팝콘 | `Popcorn for Safari`, `Popcorn: Movies & TV Watchlist` | 다수(KR '팝콘티비' 등) | **`popcorntime/popcorntime` ★10,701**, `mozilla/popcorn-js` ★2,127 | PyPI·npm·crates 선점 | 일반어 | **1** |
| 스낵바 | 없음 | `Snackbar …` 다수, KR `스낵바 타이쿤` | **`nispok/snackbar` ★1,510**, `MrEngineer13/SnackBar` ★1,210 | npm 선점 | Android Material 컴포넌트 이름 자체 | **1** |
| 피스타캐시 | 없음 | 없음 | 계정 없음, 레포 0개 | 모두 없음 | 웹 검색에서 같은 이름 앱 없음 | **5** |
| 키블 | 없음 | `Kibble`, `Kibble — Pet Food Scanner`, `Kibble: Dog Care Log` | 계정 선점(레포 30개), 레포 465개(`apache/kibble-1` ★58) | npm·crates 선점 | 일반어(사료) | 2 |
| 바이트코드 | 없음 | 없음 | 계정 선점(레포 0개), 레포 36개(최대 ★4) | PyPI·npm 선점 | 듣고 bytecode로 칠 철자 함정 | 3 |
| 기가바이트 | 없음 | 없음 | 계정 선점(레포 2개), 레포 110개(최대 ★14) | 모두 없음 | 발음이 일반어 gigabyte와 같아 검색에서 묻힌다. 하드웨어 브랜드 GIGABYTE와도 발음이 같다(브랜드는 이번에 따로 조회하지 않음) | 2 |
| 펙키지 | 없음 | 없음 | 계정 선점(레포 21개, ★1), 레포 26개 | 모두 없음 | 듣고 package로 칠 철자 함정 | 4 |
| 디바우어 | 없음 | `Devour Idle RPG`, `Devour: Read Faster` 등 | 조직 선점, 레포 820개(★432 두 개) | PyPI·npm 선점 | Steam 협동 호러 게임 **DEVOUR**(리뷰 68,367개). 일반어 | 2 |
| 칩멍크 | 없음 | 없음 | 조직 선점(레포 0개), 레포 2개(★1) | 모두 없음 | 듣고 chipmunk로 칠 철자 함정 | 4 |
| 스낵샷 | 없음 | `Snackshot` 동명, `SnackShot: Archery World Tour` | 계정 선점(레포 0개), 레포 7개 | 모두 없음 | 듣고 snapshot으로 칠 철자 함정 | 3 |
| 캐시보어 | 없음 | 없음 | 계정 없음, 레포 0개 | 모두 없음 | 웹 검색에서 같은 이름 앱 없음 | **5** |
| 니빗 | 없음 | `Nibbit: Code Daily`(코딩 학습), `nibbit: Habit Tracker`, `Nibbit: learn words to scroll` | 계정 선점(레포 1개), 레포 11개(최대 ★28) | npm 선점 | | 3 |
| 캐츄 | 없음 | 동명 없음(`Picachu …` 유사) | 계정 선점(레포 1개), 레포 312개(최대 ★32) | PyPI·npm 선점 | 포켓몬 '-츄' 연상 | 3 |

확인 못 한 것은 3.2와 같다(상표, 도메인, Google Play, 실행 파일 충돌, iTunes Search API 한계). 2차 App Store 조회는 429 없이 끝났다.

### 6.3 점수표 (캐슈 = 1차 점수 그대로, 기준선)

| 후보 | C1 먹기 | C2 한·영 | C3 짧음 | C4 피식 | C5 구전 | **소계 (사용자 5)** | C6 고유 | C7 안심 | C8 세계관 | **합계 (8)** |
|---|---|---|---|---|---|---|---|---|---|---|
| 캐슈 / Cachew (1차 기준선) | 3 | 5 | 5 | 5 | 3 | **21** | 3 | 5 | 4 | **33** |
| 피스타캐시 / Pistacache | 3 | 4 | 3 | 5 | 3 | **18** | 5 | 5 | 4 | **32** |
| 키블 / Kibble | 4 | 4 | 5 | 3 | 4 | **20** | 2 | 5 | 5 | **32** |
| 브루니 / Brewnie | 3 | 3 | 4 | 4 | 3 | **17** | 4 | 5 | 5 | **31** |
| 템푸라 / Tempura | 3 | 4 | 4 | 4 | 4 | 19 | 2 | 4 | 5 | 30 |
| 팝콘 / Popcorn | 4 | 5 | 4 | 4 | 5 | 22 | 1 | 4 | 3 | 30 |
| 스낵바 / Snackbar | 3 | 5 | 4 | 4 | 5 | 21 | 1 | 5 | 3 | 30 |
| 펙키지 / Peckage | 3 | 3 | 4 | 4 | 2 | 16 | 4 | 5 | 4 | 29 |
| 캐시보어 / Cachivore | 3 | 3 | 3 | 5 | 2 | 16 | 5 | 4 | 4 | 29 |
| 니빗 / Nibbit | 3 | 3 | 5 | 4 | 3 | 18 | 3 | 5 | 3 | 29 |
| 덤플링 / Dumpling | 3 | 4 | 4 | 4 | 4 | 19 | 1 | 4 | 4 | 28 |
| 칩멍크 / Chipmunch | 3 | 3 | 3 | 3 | 3 | 15 | 4 | 5 | 4 | 28 |
| 스낵샷 / Snackshot | 4 | 4 | 3 | 4 | 3 | 18 | 3 | 4 | 3 | 28 |
| 캐츄 / Cachu | 3 | 3 | 5 | 3 | 3 | 17 | 3 | 5 | 3 | 28 |
| 기가바이트 / Gigabite | 3 | 4 | 3 | 5 | 2 | 17 | 2 | 4 | 3 | 26 |
| 바이트코드 / Bitecode | 2 | 4 | 3 | 4 | 2 | 15 | 3 | 4 | 3 | 25 |
| 디바우어 / Devour | 3 | 3 | 3 | 4 | 3 | 16 | 2 | 2 | 3 | 23 |

채점 메모:

- 철자 함정(Brewnie → brownie, Bitecode → bytecode, Gigabite → gigabyte, Peckage → package, Chipmunch → chipmunk, Snackshot → snapshot)은 1차에서 캐슈(cashew로 칠 위험)에 C5=3을 준 것과 같은 기준으로 감점했다. 원래 단어와 발음이 완전히 같으면 2, 비슷하면 3이다.
- 팝콘(소계 22)과 스낵바(소계 21)는 사용자 기준 소계로는 캐슈(21)와 같거나 높지만, ★1k 이상 개발 프로젝트와 겹쳐서 C6이 1이다.

### 6.4 2차 결론

**합계로 캐슈(33)를 넘는 후보는 2차에서도 나오지 않았다.** 가장 가까운 후보는 다음 셋이다.

- **피스타캐시 / Pistacache (32)**: 캐슈와 같은 견과·다람쥐 세계관이고, 조회한 모든 곳에서 충돌이 0건인 두 후보 중 하나다(다른 하나는 캐시보어). 하지만 5음절·10자라 메뉴바와 알림에 넣기엔 길고, 말로 들으면 pistachio로 칠 수 있다.
- **키블 / Kibble (32)**: 반려견 옷 = 코스튬이라 세계관이 가장 자연스럽다. '사료 = 먹이'라서 Inception의 "먹이 카탈로그"와도 이어진다. 약점은 일반어라 검색에 묻히고 같은 이름 iOS 앱이 3개라는 점, 그리고 KiB 말장난은 캐슈의 cache만큼 바로 와닿지 않는다는 점이다.
- **브루니 / Brewnie (31)**: "밤에 몰래 집안일을 하고 음식을 받는 집요정"이라는 민담이 "백그라운드에서 찾고, 허락받은 것만 먹는다"는 제품 콘셉트와 가장 잘 맞는다. 충돌도 거의 없다(GitHub 레포 1개, ★0). 약점은 brownie로 칠 철자 함정, 한글 '브루니'에 뜻이 없다는 점, 그리고 개발 말장난이 Homebrew 하나에만 기대고 있다는 점이다.

1·2차 합산 상위 5개:

| 순위 | 후보 | 합계 | 소계 | 캐슈 대비 |
|---|---|---|---|---|
| 1 | 캐슈 / Cachew | 33 | 21 | 기준 |
| 2 | 먹방 / Mukbang | 32 | 22 | 소계(사용자 기준)에서 캐슈를 이김. 고유성에서 짐 |
| 2 | 피스타캐시 / Pistacache | 32 | 18 | 고유성(C6 5 vs 3)에서 이김. 길이·구전성에서 짐 |
| 2 | 키블 / Kibble | 32 | 20 | 먹기(C1)·세계관(C8)에서 이김. 고유성·피식에서 짐 |
| 5 | 브루니 / Brewnie | 31 | 17 | 고유성·세계관에서 이김. 한·영 자연스러움·피식에서 짐 |

판단(제안): 캐슈를 고른다면 2차 후보 일부를 버리지 않고 쓸 수 있다. 피스타캐시·칩멍크는 견과·다람쥐 코스튬 이름으로, 브루니의 "밤에 일하고 음식을 받는 요정" 서사는 캐릭터 설정으로 가져올 수 있다.

---

## 7. 다음 단계

- [x] 사용자가 이름 1개를 고른다 → **캐슈 / Cachew** (2026-10-30)
- [ ] 상표(KIPRIS/USPTO)를 조회할지 결정한다(미정, 아직 조회 안 함)
- [x] `tophun/cachew` Public 레포 생성 → `~/paseo/cachew`에 clone → 이 문서를 `aidlc-docs/00-naming.md`로 이동
- [ ] Inception 시작(요구사항 분석, 먹이 카탈로그)

---

## 출처

- 먹깨비 공공배달앱: [서울신문 2024-07-26](https://m.go.seoul.co.kr/news/2024/07/26/20240726500148), [서울신문 2025-04-24](https://m.go.seoul.co.kr/news/2025/04/24/20250424024002), [디지털데일리 2026-06-12](https://www.ddaily.co.kr/page/view/2026061217185932827), [스마트투데이](https://www.smarttoday.co.kr/ko-kr/articles/106226), [헤럴드K 2026-04-08](https://www.heraldk.com/article/2026040822575046445)
- App Store: [먹깨비](https://apps.apple.com/kr/app/%EB%A8%B9%EA%B9%A8%EB%B9%84-%ED%95%A0%EC%9D%B8%EC%97%90-%EC%A7%80%EC%97%AD%ED%99%94%ED%8F%90%EB%A5%BC-%EB%8D%94%ED%95%98%EB%8B%A4/id1355591390), [캐슈](https://apps.apple.com/kr/app/%EC%BA%90%EC%8A%88/id6824707374), [Crumbs: Storage Cleaner](https://apps.apple.com/us/app/crumbs-storage-cleaner/id6817333077)
- Cachew 라이브러리: [Maven Central io.github.rjjingar:cachew](https://central.sonatype.com/artifact/io.github.rjjingar/cachew), GitHub `karlicoss/cachew`
- 같은 카테고리 앱: [Dev Disk Hygiene](https://apps.apple.com/app/dev-disk-hygiene/id6757937868), [DevPowerCleanup](https://apps.apple.com/us/app/devpowercleanup/id6758896751), [CodeCleaner – Dev Disk Cleanup](https://apps.apple.com/us/app/codecleaner-dev-disk-cleanup/id6760048159), [CleanMyCodeMac](https://awesome.ecosyste.ms/projects/github.com%2Fitling%2Fcleanmycodemac)
- 2차: [Brownie (folklore) — Wikipedia](https://en.wikipedia.org/wiki/Brownie_(folklore)), [DEVOUR — Steam](https://store.steampowered.com/app/1274570/DEVOUR/)
- 그 밖의 수치(GitHub 레포 수·별, brew, PyPI·npm·crates)는 2026-10-30에 `gh api`, `brew info`, 각 레지스트리 API로 직접 조회한 값이다.
