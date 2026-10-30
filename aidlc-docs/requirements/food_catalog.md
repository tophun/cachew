---
artefact_type: requirements
task: food_catalog
phase: inception
status: draft
created_at: 2026-10-30
---

# 먹이 카탈로그 (Inception Step 4)

캐슈가 찾아서 제안할 수 있는 개발 캐시·산출물 목록이다. 앱은 **이 카탈로그에 있는 경로만** 다룬다(NFR-S1). `X` 행은 캐시 옆에 붙어 있어 실수로 먹기 쉬운 설정·인증 파일이며, 하드 deny-list가 된다(NFR-S2).

## 열 정의

| 열 | 뜻 |
|---|---|
| 기본 경로/대상 | macOS 기본 위치와 위치를 바꾸는 환경 변수·설정 |
| 재생성 방법 | 지웠을 때 무엇이 다시 만들어 주는가 |
| 휴지통 가능 | `예` / `아니오 — 도구 명령: …`(복구 불가, C3) / `둘 다 — 권장: …` |
| 권한 필요 | `없음` / `확인 필요`(macOS 권한 프롬프트 여부를 공식 출처로 확인하지 못함) |
| 안전 등급 | 아래 기준 |
| 감지 신호 | 스캐너가 크기·오래됨을 판단하는 근거 |
| 제외 조건 | 제안하면 안 되는 상황(D5 커밋 안 된 변경, 도구 실행 중 등) |
| 출처 | 공식 문서 URL + 근거 인용. 공식 출처가 없으면 `미확인`이고 등급을 한 단계 낮춘다 |

## 안전 등급

| 등급 | 뜻 | 캐슈의 동작 (US-FORAGE-003) |
|---|---|---|
| A | 순수 캐시. 도구가 알아서 다시 만들고 사용자 데이터가 없다 | 기본 목록에 제안 |
| B | 다시 만들 수 있지만 비용이 크다(대용량 재다운로드·재빌드, 오프라인 작업 불가) | 기본 목록에 제안 + "다시 만들려면 시간이 걸림" 표시 |
| C | 사용자 데이터·상태가 섞여 있을 수 있다 | 자동 제안 금지. "직접 확인 필요"에서 하나씩만 |
| X | 먹으면 안 된다(설정·인증·사용자가 의존하는 전역 설치) | 어디에도 표시하지 않음. deny-list |

## 조사 방법

- 직군을 4묶음으로 나눠 조사했다. 묶음마다 같은 열 정의·등급 기준·출처 규칙을 썼다.
- 각 묶음은 이번 세션(2026-10-30)에 공식 문서·공식 저장소를 직접 열어 확인한 결과다. 파일 삭제나 로컬 파일 시스템 조회는 하지 않았다.
- 교차 검증 결과는 맨 끝 "검증" 절에 있다.

---

## 1. 프론트/Node

| 생태계 | 먹이 | 기본 경로/대상 (macOS) | 재생성 방법 | 휴지통 가능 | 권한 필요 | 안전 등급 | 감지 신호 | 제외 조건 | 출처 |
|---|---|---|---|---|---|---|---|---|---|
| Node | npm 캐시 | `~/.npm/_cacache` (`npm_config_cache` 또는 `.npmrc`의 `cache=`로 변경). npx 캐시도 `~/.npm` 아래 | 다음 `npm install`/`npx` 때 다시 받음 | 둘 다 — 권장: `_cacache` 휴지통. `npm cache clean --force`는 복구 불가 | 없음 | A | 크기, mtime | npm/npx 실행 중 | https://docs.npmjs.com/cli/v11/commands/npm-cache — "it should never be necessary to clear the cache for any reason other than reclaiming disk space" |
| Node | npm 사용자 설정 | `~/.npmrc` | — | 아니오 | 없음 | **X** | 파일 존재 | 항상 제외. `_authToken` 등 레지스트리 인증 정보 | https://docs.npmjs.com/cli/v11/configuring-npm/npmrc |
| Node | Yarn Classic(v1) 전역 캐시 | `~/Library/Caches/Yarn` (`YARN_CACHE_FOLDER`, `XDG_CACHE_HOME`, `yarn cache dir`) | 다음 `yarn install` 때 다시 채워짐 | 둘 다 — 권장: 휴지통. `yarn cache clean`은 복구 불가 | 없음 | A | 크기, mtime | yarn 실행 중 | https://classic.yarnpkg.com/en/docs/cli/cache — "It will be populated again the next time yarn or yarn install is run." (macOS 경로: yarn 소스 `src/util/user-dirs.js`) |
| Node | Yarn Berry(v2+) 전역 캐시 | `globalFolder`(기본 `~/.yarn/berry`) 안의 캐시 폴더. 정확한 하위 경로는 `yarn config get cacheFolder` | 다음 `yarn install` 때 다시 받음 | 둘 다 — 권장: 캐시 폴더만 휴지통 | 없음 | A | 크기, mtime | yarn 실행 중. `~/.yarn/berry` 전체는 대상 아님 | https://yarnpkg.com/configuration/yarnrc — "If true (the default), Yarn will store the cache files into a folder located within globalFolder" |
| Node | Yarn Berry 프로젝트 로컬 캐시 | `<project>/.yarn/cache` (`enableGlobalCache: false`일 때) | `yarn install`이 다시 만듦 | 예 (git 미추적일 때만) | 확인 필요 | C (git 추적 중이면 X) | `.yarnrc.yml`, 크기 | `git ls-files .yarn/cache` 결과가 있으면 금지(Zero-Installs). 커밋 안 된 변경 | https://yarnpkg.com/features/caching |
| Node | pnpm 콘텐츠 주소 스토어 | `~/Library/pnpm/store` (`storeDir`, `$PNPM_HOME/store`, `pnpm store path`) | 다음 `pnpm install` 때 다시 받음 | 둘 다 — 권장: `pnpm store prune`(참조되지 않는 패키지만, 복구 불가). 통째로 휴지통은 공유 블록 때문에 실제 확보량이 작음 | 없음 | A | 크기(공유 블록 포함이라 부풀려짐) | pnpm 설치 중 | https://pnpm.io/cli/store — "Running pnpm store prune is not harmful and has no side effects on your projects." / https://pnpm.io/settings/store — "On macOS: ~/Library/pnpm/store" |
| Node | pnpm 메타데이터·dlx 캐시 | `~/Library/Caches/pnpm` (`cacheDir`) | 다음 설치·`pnpm dlx` 때 다시 생김 | 예 | 없음 | A | 크기, mtime | pnpm 실행 중 | https://pnpm.io/settings/other — "On macOS: ~/Library/Caches/pnpm" |
| Node | Bun 전역 캐시 | `~/.bun/install/cache` (`BUN_INSTALL_CACHE_DIR`) | 다음 `bun install` 때 다시 받음 | 둘 다 — 권장: `links/`를 뺀 나머지만 휴지통. `bun pm cache rm`은 `links/`까지 지움(복구 불가) | 없음 | A (`links/` 있으면 B) | 크기, `links/` 존재 | `links/`(전역 스토어)는 자동 제안 금지 — 지우면 연결된 프로젝트가 깨짐 | https://bun.sh/docs/install/cache / https://bun.sh/docs/pm/global-store — "breaks until you run bun install" |
| Node | 프로젝트 `node_modules` | 스캔 루트 아래 프로젝트의 최상위 `node_modules` | lockfile 기준 `npm ci`·`pnpm install`·`yarn install`·`bun install` | 예 | 확인 필요 | B | `package.json`과 lockfile 존재, 크기, 마지막 커밋·수정 시각 | 커밋 안 된 변경(D5). lockfile 없으면 C. 실행 중 node 프로세스의 cwd. git이 추적 | https://docs.npmjs.com/cli/v11/configuring-npm/folders — "Local install (default): puts stuff in ./node_modules of the current package root." |
| Node | Next.js 빌드 캐시 | `<project>/.next/cache` | 다음 `next build`가 다시 만듦 | 예 | 확인 필요 | A | 크기, mtime, `next` 의존성 | `next dev`/`build` 실행 중. 커밋 안 된 변경. `.next` 전체는 대상 아님 | https://nextjs.org/docs/app/guides/ci-build-caching — "Next.js saves a cache to .next/cache that is shared between builds." |
| Node | Turborepo 로컬 캐시 | `<repo>/.turbo/cache` (`turbo.json`의 `cacheDir`) | 캐시 miss 시 태스크 재실행 | 예 | 확인 필요 | A | 크기, mtime, `turbo.json` | turbo 실행 중. `cache`만 대상 | https://turborepo.dev/docs/crafting-your-repository/caching — "Turborepo stores the results of tasks in the .turbo/cache directory" |
| Node | Playwright 브라우저 | `~/Library/Caches/ms-playwright` (`PLAYWRIGHT_BROWSERS_PATH`) | `npx playwright install`(용량 큼) | 둘 다 — 권장: 버전 폴더 단위 휴지통. `npx playwright uninstall`은 복구 불가 | 없음 | B | 버전 폴더별 크기·mtime | 테스트 실행 중. 스캔 루트의 프로젝트가 그 버전 사용 | https://playwright.dev/docs/browsers — "~/Library/Caches/ms-playwright on macOS" |
| Node | Cypress 바이너리 | `~/Library/Caches/Cypress` (`CYPRESS_CACHE_FOLDER`) | `cypress install` | 둘 다 — 권장: 구버전 폴더만 휴지통. `cypress cache prune`은 복구 불가이고 다른 프로젝트용 버전도 지움 | 없음 | B | `cypress cache list --size` | Cypress 실행 중. 다른 프로젝트가 쓰는 버전 | https://docs.cypress.io/app/references/advanced-installation — macOS 기본값 "~/Library/Caches/Cypress" |
| Node | nvm으로 설치한 Node 버전 | `~/.nvm/versions/node/vX.Y.Z` (`NVM_DIR`) | `nvm install` 후 전역 패키지 재설치 | 둘 다 — 권장: 휴지통. `nvm uninstall`은 복구 불가 | 없음 | C | 버전 폴더 크기·mtime, default alias, `.nvmrc` | default/사용 중 버전. 실행 중 프로세스. `.nvmrc` 참조 | https://github.com/nvm-sh/nvm#readme |
| Node | nvm 다운로드 캐시 | `~/.nvm/.cache` | 다음 `nvm install` 때 다시 받음 | 둘 다 — 권장: 휴지통 | 없음 | A | 크기 | `nvm install` 진행 중 | https://github.com/nvm-sh/nvm — "nvm cache clear to delete cached node downloads" |
| Node | fnm으로 설치한 Node 버전 | `~/.local/share/fnm/node-versions/<ver>` (`FNM_DIR`; 기존 `~/.fnm` 우선) | `fnm install` 후 전역 패키지 재설치(추론) | 둘 다 — 권장: 휴지통 | 없음 | C | 버전 폴더 크기·mtime, default alias | default 버전. 실행 중 프로세스. `.nvmrc`/`.node-version` 참조 | https://github.com/Schniz/fnm (소스 `src/config.rs`, `src/directories.rs`) |
| Node | Corepack 캐시 | `~/.cache/node/corepack` (`COREPACK_HOME`) | 다시 호출 시 해당 버전을 받음 | 둘 다 — 권장: 휴지통 | 없음 | A | 크기 | 오프라인 캐시로 쓰는 경우 | https://github.com/nodejs/corepack#readme |

## 2. Python/ML

| 생태계 | 먹이 | 기본 경로/대상 (macOS) | 재생성 방법 | 휴지통 가능 | 권한 필요 | 안전 등급 | 감지 신호 | 제외 조건 | 출처 |
|---|---|---|---|---|---|---|---|---|---|
| Python | pip 캐시 | `~/Library/Caches/pip` (`PIP_CACHE_DIR`, `pip cache dir`) | 다음 `pip install` 때 다시 받음 | 둘 다 — 권장: 휴지통. `pip cache purge`는 복구 불가 | 없음 | A | 크기, mtime | pip 실행 중 | https://pip.pypa.io/en/stable/topics/caching/ — macOS "~/Library/Caches/pip" |
| Python | uv 캐시 | `~/.cache/uv` (`UV_CACHE_DIR`, `uv cache dir`) | 다음 `uv sync`/`uvx` 때 다시 받음 | **아니오 — 도구 명령: `uv cache prune`**(권장, 안 쓰는 항목만) / `uv cache clean`(전체). 복구 불가 | 없음 | A | 크기 | uv 실행 중. `link-mode = symlink` 사용자는 `clean` 금지 | https://docs.astral.sh/uv/concepts/cache/ — "it's never safe to modify the cache directly (e.g., by removing a file or directory)" |
| Python | Poetry 패키지 캐시 | `~/Library/Caches/pypoetry`에서 **`virtualenvs/`를 뺀** 하위 항목 (`POETRY_CACHE_DIR`) | 다음 `poetry install` 때 다시 받음 | 둘 다 — 권장: `poetry cache clear --all`(복구 불가) | 없음 | A | `poetry cache list`, 크기 | poetry 실행 중. `virtualenvs/` 절대 포함 금지 | https://python-poetry.org/docs/configuration/ — cache-dir macOS "~/Library/Caches/pypoetry" |
| Python | Poetry 중앙 가상환경 | `~/Library/Caches/pypoetry/virtualenvs/*` | 해당 프로젝트에서 `poetry install` | 둘 다 — 권장: 휴지통 | 없음 | B | `pyvenv.cfg`, mtime | 실행 중 프로세스가 env 안의 실행 파일 사용. 원래 프로젝트에 커밋 안 된 변경 | https://python-poetry.org/docs/configuration/ — virtualenvs.path "{cache-dir}/virtualenvs" |
| Python | conda/mamba 패키지 캐시 | `<conda 루트>/pkgs` (`CONDA_PKGS_DIRS`, `conda info`) | 다음 `conda install` 때 다시 받음 | **아니오 — 도구 명령: `conda clean --all`**(`--dry-run`으로 미리보기). 환경과 하드링크로 공유돼 휴지통 비권장 | 없음 | A | `conda clean --all --dry-run` | conda 실행 중. `--force-pkgs-dirs` 사용 금지 | https://docs.conda.io/projects/conda/en/latest/commands/clean.html |
| Python | pyenv로 설치한 Python 버전 | `~/.pyenv/versions/<ver>` (`PYENV_ROOT`) | `pyenv install`(소스 빌드) 후 패키지 재설치 | 둘 다 — 권장: 휴지통 | 없음 | C | 버전 폴더 크기·mtime, `pyenv global`, `.python-version` | global/local 지정 버전. `pyvenv.cfg`의 `home`이 가리키는 venv 존재. 실행 중 프로세스 | https://github.com/pyenv/pyenv#readme — "simply rm -rf the directory" |
| Python | 프로젝트 가상환경 | `<project>/.venv`, `<project>/venv` (`pyvenv.cfg` 존재) | `python -m venv` + 의존성 재설치, `uv sync`, `poetry install` | 예 | 확인 필요 | B | `pyvenv.cfg`, 크기, lock/requirements 존재, 마지막 수정·커밋 | 커밋 안 된 변경. 실행 중 프로세스. lock/requirements 없으면 C. git이 추적 | https://docs.python.org/3/library/venv.html — "Considered as disposable – it should be simple to delete and recreate it from scratch." |
| Python/ML | Hugging Face Hub 캐시 | `~/.cache/huggingface/hub` (`HF_HUB_CACHE` > `$HF_HOME/hub`) | 다음 `from_pretrained` 때 다시 받음(GB 단위) | 둘 다 — 권장: repo 폴더(`models--org--name`) 단위 휴지통. `hub/blobs/` 직접 조작 금지 | 없음 | B | repo별 크기·접근 시각, `.incomplete` 파일 | 다운로드 중. `HF_HUB_OFFLINE=1` 사용자. 학습·추론 중 | https://huggingface.co/docs/huggingface_hub/guides/manage-cache — "The default <CACHE_DIR> is ~/.cache/huggingface/hub" |
| Python/ML | Hugging Face datasets 캐시 | `~/.cache/huggingface/datasets` (`HF_DATASETS_CACHE`) | 다음 `load_dataset` 때 다시 변환 | 예 | 없음 | B | 크기, mtime | datasets 작업 중 | https://huggingface.co/docs/datasets/cache |
| Python/ML | HF 토큰 | `~/.cache/huggingface/token` (`HF_TOKEN_PATH`) | — | 아니오 | 없음 | **X** | 파일 존재 | 항상 제외. `~/.cache/huggingface`를 통째로 먹이로 묶지 말 것 | https://huggingface.co/docs/huggingface_hub/package_reference/environment_variables |
| Python/ML | HF Xet 업로드 캐시 | `~/.cache/huggingface/xet` (`HF_XET_CACHE`) | 업로드 시 다시 생김 | 예 | 없음 | A | 크기 | 업로드 진행·재개 대기 중 | https://huggingface.co/docs/huggingface_hub/guides/manage-cache — "simply remove the xet cache entirely" |
| Python/ML | PyTorch Hub 캐시·가중치 | `~/.cache/torch/hub` (`TORCH_HOME`) | 다음 로드 때 다시 받음 | 예 | 없음 | B | 크기, `checkpoints` mtime | 학습·추론 중. 오프라인 작업 | https://docs.pytorch.org/docs/stable/hub.html |
| Python | 설정·인증 파일 | pip: `~/Library/Application Support/pip/pip.conf`, `~/.config/pip/pip.conf`, `~/.pip/pip.conf`. Poetry: `~/Library/Application Support/pypoetry`. conda: `~/.condarc` 등 | — | 아니오 | 없음 | **X** | 파일 존재 | 항상 제외. 인덱스·프록시 설정, Poetry `auth.toml` 비밀번호 | https://pip.pypa.io/en/stable/topics/configuration/ · https://python-poetry.org/docs/repositories/ |
| Python | 전역 도구·인터프리터·환경 | pipx `~/Library/Application Support/pipx`, uv `~/.local/share/uv/python`·`tools`, conda `<루트>/envs/*`, `~/.local/bin` | — | 아니오 | 없음 | **X** | 디렉터리 존재 | 항상 제외. 사용자가 의존하는 전역 CLI·인터프리터 | https://pipx.pypa.io/stable/reference/environment-variables.html · https://docs.astral.sh/uv/reference/storage/ |

### 1·2절 조사에서 나온 주의할 점

1. **캐시 폴더 안에 상태·비밀이 섞여 있다**: `pypoetry/virtualenvs`, `huggingface/token`, `bun/install/cache/links/`, git이 추적하는 `.yarn/cache`. 부모 폴더를 먹이 하나로 정의하지 말고 하위 경로 단위로 정의한다.
2. **보이는 크기와 실제 확보량이 다르다**: pnpm·Bun·uv(clone), conda(하드링크)는 블록을 공유한다. UI에 "회수 예상량"과 "휴지통을 비워야 확보됨"을 나눠 보여 준다(US-DIGEST-003).
3. **직접 지우면 안 되는 캐시**: uv는 공식 문서가 직접 수정을 금지한다 → 도구 명령형. conda `--force-pkgs-dirs`는 환경을 깨뜨린다.
4. **버전 매니저는 연쇄로 깨진다**: Node·Python 버전을 지우면 그 버전 위의 전역 패키지와 venv가 깨진다. 제안 전 `pyvenv.cfg`의 `home`을 역조회한다. 메뉴바 앱은 셸의 `VIRTUAL_ENV`를 볼 수 없으므로 "사용 중" 판정은 실행 중 프로세스의 실행 파일 경로로 한다.
5. **lockfile이 없으면 재현이 보장되지 않는다**: lockfile 없는 `node_modules`·venv는 C로 내린다.

### 1·2절에서 확인하지 못한 것

- Yarn Berry 전역 캐시의 정확한 하위 폴더 이름(`yarn config get cacheFolder`로 런타임 확인 필요)
- Volta 하위 경로, `pipx run` 캐시 기본 경로, conda·mamba 기본 설치 루트
- `~/Documents`·`~/Desktop`·iCloud Drive를 스캔 루트로 쓸 때 macOS 권한 프롬프트 여부
- Bun `bun pm cache prune` 범위, nvm alias 저장 위치, Poetry `virtualenvs/` 이름 규칙
- 용량이 작아 뺀 것: Vite `node_modules/.vite`, `__pycache__`. 조사하지 않은 것: Parcel, node-gyp, Electron 캐시

## 3. iOS/Xcode · Android

| 생태계 | 먹이 | 기본 경로/대상 (macOS) | 재생성 방법 | 휴지통 가능 | 권한 필요 | 안전 등급 | 감지 신호 | 제외 조건 | 출처 |
|---|---|---|---|---|---|---|---|---|---|
| iOS/Xcode | DerivedData | `~/Library/Developer/Xcode/DerivedData/<Workspace>-<hash>/`. Xcode Settings › Locations에서 Relative로 바꾸면 **프로젝트 폴더 안**에 생김. `xcodebuild -derivedDataPath` | 다음 빌드·인덱싱 때 재생성(풀 리빌드·재인덱싱) | 예 (프로젝트별 하위 폴더 단위) | 없음 (Relative/Custom이 보호 폴더면 확인 필요) | C (**출처 미확인으로 B → C**) | 하위 폴더별 크기·mtime, `info.plist`의 `WorkspacePath`가 가리키는 프로젝트가 없음(추론) | Xcode·xcodebuild 실행 중, 해당 워크스페이스가 열려 있음 | **미확인** — Apple 공식 문서에서 경로를 찾지 못함 |
| iOS/Xcode | Archives(`.xcarchive`, dSYM 포함) | `~/Library/Developer/Xcode/Archives/<날짜>/` (경로 미확인) | **재생성 불가** | 예 | 없음 | C | 날짜, 크기. 배포 여부를 앱이 알 수 없음 | 배포한 빌드의 아카이브. 자동 제안 금지 | https://developer.apple.com/documentation/xcode/building-your-app-to-include-debugging-information — "You must retain the Xcode archive for each build of your app you distribute." |
| iOS/Xcode | DeviceSupport(OS 심볼) | `~/Library/Developer/Xcode/<iOS\|watchOS\|tvOS\|visionOS> DeviceSupport/<버전> (<빌드>)/` | 해당 OS 기기를 연결하면 다시 복사. 그 OS 기기가 없으면 사실상 재생성 불가 | 예 | 없음 | B | 폴더명 OS 버전이 현재 쓰는 기기와 맞지 않음, mtime, 크기 | 연결·페어링된 기기의 OS 버전. 그 OS 크래시 리포트를 심볼리케이트해야 함. Xcode 실행 중 | https://developer.apple.com/documentation/xcode/acquiring-operating-system-symbol-information — "the system saves them in ~/Library/Developer/Xcode/<iOS\|watchOS\|tvOS\|visionOS> DeviceSupport" |
| iOS/Xcode | 시뮬레이터 디바이스(unavailable 포함) | `~/Library/Developer/CoreSimulator/Devices/<UDID>/` (`device_set.plist`는 절대 건드리지 않음) | **재생성 불가**(설치 앱·데이터 사라짐) | 둘 다 — 권장: **`xcrun simctl delete unavailable`** 또는 `xcrun simctl delete <UDID>`(복구 불가). 폴더를 휴지통에 넣으면 CoreSimulatorService 레지스트리와 어긋날 수 있음(추론) | 없음 | C | `xcrun simctl list devices -j`의 `isAvailable`, 폴더 크기 | Simulator·CoreSimulatorService 실행 중, Booted, 테스트 실행 중 | Apple DTS 포럼 https://developer.apple.com/forums/thread/812408 — "For CoreSimulator, it's probably best to not monkey with that yourself" (경로 자체는 미확인) |
| iOS/Xcode | 시뮬레이터 런타임 | SIP로 보호되는 시스템 관리 저장소의 디스크 이미지(경로 미확인). 휴지통 대상 아님 | Xcode › Settings › Components 또는 `xcodebuild -downloadPlatform iOS`(대용량) | **아니오 — 도구 명령: `xcrun simctl runtime delete <identifier>`** 또는 Xcode Components › Delete | 확인 필요 (관리자 권한 필요 여부 미확인) | B | `xcrun simctl runtime list -j`, 쓰는 디바이스 수, 현재 Xcode SDK와 일치 여부 | 현재 Xcode 기본 SDK 런타임, Booted 디바이스가 쓰는 런타임, Simulator 실행 중, 이미 "Deleting" 상태 | https://developer.apple.com/documentation/xcode-release-notes/xcode-14-release-notes — "Disk images are added to a system-managed storage location protected by System Integrity Protection" |
| iOS/Xcode | CoreSimulator Caches | `~/Library/Developer/CoreSimulator/Caches/` (내용 미확인) | 필요할 때 다시 생성(추론) | 예 (CoreSimulatorService가 멈춰 있을 때만) | 없음 | C (미확인으로 하향) | 크기 | Simulator·CoreSimulatorService 실행 중 | **미확인.** DTS는 CoreSimulator 직접 조작을 권하지 않음 |
| iOS/Xcode | Xcode Previews 데이터 | `~/Library/Developer/Xcode/UserData/Previews/` (이 하위 폴더만. 나머지 UserData는 X) | 다음 프리뷰 때 재생성(추론) | 둘 다 — 권장: Xcode 종료 후 `xcrun simctl --set previews delete all`(복구 불가) | 없음 | C (미확인으로 하향) | 크기, mtime | Xcode 실행 중 | **미확인** (포럼 2차 전언만) |
| iOS/Xcode | DocumentationCache/Index | `~/Library/Developer/Xcode/DocumentationCache/`, `DocumentationIndex/` | Xcode가 다시 만듦 | 예 (두 폴더 함께) | 없음 | B (DTS 포럼 답변이라 A에서 하향) | 크기 | Xcode 실행 중 | Apple DTS 포럼 https://developer.apple.com/forums/thread/812408 — "You can delete it, but Xcode will have to rebuild it" |
| iOS/Xcode | 구버전 Xcode 앱 | `/Applications/Xcode*.app` | developer.apple.com에서 다시 받음(대용량) | 예 | 확인 필요 (`/Applications` 쓰기 권한) | B | `xcode-select -p`와 다른 Xcode, 번들 버전 | `xcode-select -p`·`DEVELOPER_DIR`가 가리키는 Xcode, 실행 중, 일부러 둔 버전(사용자 확인) | https://developer.apple.com/documentation/xcode/downloading-and-installing-additional-xcode-components (여러 Xcode 병행 설치 전제) |
| iOS/Xcode | SwiftPM 전역 캐시 | `~/Library/Caches/org.swift.swiftpm/` (`--cache-path`) | 다음 resolve 때 다시 받음 | 둘 다 — 권장: 휴지통. `swift package purge-cache`는 복구 불가 | 없음 | A | 크기, mtime | `swift build`/resolve 중, Xcode가 패키지 resolve 중 | SwiftPM 소스 `Sources/Basics/FileSystem/FileSystem+Extensions.swift` — "SwiftPM cache directory under user's caches directory" |
| iOS/Xcode | SwiftPM 프로젝트 `.build` | `<패키지 루트>/.build/` (`--scratch-path`) | 다음 `swift build` | 둘 다 — 권장: 휴지통 | 확인 필요 | B | `Package.swift` 옆, 크기, mtime | 커밋 안 된 변경, 빌드 중 | SwiftPM 소스 `Sources/CoreCommands/Options.swift` — "(default .build)" |
| iOS/Xcode | CocoaPods 다운로드 캐시 | `~/Library/Caches/CocoaPods/` (`CP_CACHE_DIR`) | 다음 `pod install` | 둘 다 — 권장: 휴지통. `pod cache clean --all`은 복구 불가 | 없음 | A | 크기, `pod cache list` | `pod install`/`update` 중 | https://www.rubydoc.info/gems/cocoapods/Pod/Config · https://guides.cocoapods.org/terminal/commands.html |
| iOS/Xcode | CocoaPods spec repos | `~/.cocoapods/repos/<이름>/` | `pod repo add`/`update`로 다시 클론 | 둘 다 — 권장: `pod repo list`로 URL 기록 후 휴지통 | 없음 | B | repo별 크기, 마지막 fetch | 사설 spec repo의 push 안 한 로컬 변경, pod 실행 중 | https://guides.cocoapods.org/terminal/commands.html |
| iOS/Xcode | Carthage 캐시 | `~/Library/Caches/org.carthage.CarthageKit/` (쓰기 불가면 `~/.carthage`) | 다음 `carthage bootstrap`/`update` | 예 | 없음 | A | 크기, mtime | carthage 실행 중 | https://github.com/Carthage/Carthage/releases/tag/0.9.2 |
| iOS/Xcode | UserData·프로비저닝 프로파일 | `~/Library/Developer/Xcode/UserData/` 전체(`Previews/`만 예외), 특히 `UserData/Provisioning Profiles/`, 레거시 `~/Library/MobileDevice/Provisioning Profiles/` | — | 아니오 | 없음 | **X** | DerivedData·Archives·Previews와 부모 폴더가 같음 | 항상 제외 | https://developer.apple.com/documentation/xcode-release-notes/xcode-16-release-notes — "~/Library/Developer/Xcode/UserData/Provisioning Profiles" |
| iOS/Xcode | SwiftPM 설정·보안 | `~/Library/org.swift.swiftpm/configuration/`, `security/`, `~/.swiftpm/` | — | 아니오 | 없음 | **X** | `~/Library/Caches/org.swift.swiftpm`(A)와 이름이 거의 같음 → 경로 전체 일치로 구분 | 항상 제외 | SwiftPM 소스 `FileSystem+Extensions.swift` |
| Android | SDK system-images | `$ANDROID_HOME/system-images/android-<API>/<variant>/<arch>/` (기본 SDK 루트 `~/Library/Android/sdk`) | SDK Manager·`sdkmanager`로 다시 받음(대용량) | 둘 다 — 권장: **SDK Manager 또는 `sdkmanager --uninstall "system-images;…"`**(복구 불가). 휴지통 후 SDK Manager가 미설치로 인식하는지 미확인 | 없음 | B | 어느 AVD도 참조하지 않음(추론), 크기, 접근 시각 | AVD가 참조, 에뮬레이터 실행 중, SDK Manager 작업 중 | https://developer.android.com/studio/run/emulator-commandline — "~/Library/Android/sdk/system-images/android-apiLevel/variant/arch/" |
| Android | 오래된 SDK 패키지(build-tools, platforms, NDK) | `$ANDROID_HOME/build-tools/<ver>/`, `platforms/android-<API>/`, `ndk/<ver>/`, `ndk-bundle/` | SDK Manager로 재설치. licenses가 있으면 Gradle이 자동으로 받음 | 둘 다 — 권장: **SDK Manager 또는 `sdkmanager --uninstall`**(복구 불가) | 없음 | B | 스캔 루트 프로젝트의 `buildToolsVersion`·`ndkVersion`·`compileSdk`가 안 쓰는 버전(추론) | 프로젝트가 참조, `local.properties`의 `ndk.dir`, 빌드 중 | https://developer.android.com/tools/releases/build-tools · https://developer.android.com/studio/projects/install-ndk |
| Android | AVD(에뮬레이터 가상 기기) | `~/.android/avd/<name>.avd/` + `<name>.ini` (`ANDROID_AVD_HOME`) | **재생성 불가**(설치 앱·데이터·설정) | 둘 다 — 권장: `.ini`와 `.avd`를 **한 묶음으로** 휴지통. 한쪽만 옮기면 깨짐. `avdmanager delete avd`는 복구 불가 | 없음 | C | `emulator -list-avds`, `.avd` 크기, mtime, 참조 system image 미설치 | 에뮬레이터 실행 중, 최근 사용 | https://developer.android.com/studio/run/emulator-commandline — "contains all modifiable data for the AVD" |
| Android | 에뮬레이터 스냅샷 | AVD 데이터 디렉터리 안 `snapshots/`로 추정(경로 미확인) | Quick Boot는 다음 종료 때 재저장. 이름 붙인 스냅샷은 재생성 불가 | 둘 다 — 권장: 에뮬레이터 Snapshots 창에서 삭제하도록 **안내** | 없음 | C | 업데이트 후 무효가 된 스냅샷, 크기 | 에뮬레이터 실행 중, 이름 붙인 스냅샷 | https://developer.android.com/studio/run/emulator-snapshots |
| Android | Android Studio 현재 버전 캐시·로그 | `~/Library/Caches/Google/AndroidStudio<ver>/`, `~/Library/Logs/Google/AndroidStudio<ver>/` | 다음 실행 때 재인덱싱 | 예 | 없음 | B (Logs만은 A) | 크기, mtime | Android Studio 실행 중 | https://developer.android.com/studio/troubleshoot — "~/Library/Caches/Google/AndroidStudioVERSION" |
| Android | Android Studio 구버전 잔여 디렉터리 | `~/Library/{Application Support,Caches,Logs}/Google/AndroidStudio<ver>/`, 구 형식 `~/Library/{Preferences,Caches,Logs,Application Support}/AndroidStudio[Preview]X.Y` | 필요 없음(그 버전 미설치) | 예 | 없음 | C (Application Support에 설정·플러그인. Caches·Logs만은 A) | 대응하는 설치가 없는 버전 폴더, mtime | 그 버전이 설치됨(Toolbox 포함), 실행 중 | https://developer.android.com/studio/releases/past-releases/as-3-3-0-release-notes |
| Android | 프로젝트 `build/`·`.cxx/` | `<project>/build/`, `<module>/build/`, `<module>/.cxx/` | 다음 Gradle 빌드(네이티브는 비쌈) | 둘 다 — 권장: 휴지통. `./gradlew clean`은 복구 불가 | 확인 필요 | B | `build.gradle(.kts)` 옆, 크기, mtime | 커밋 안 된 변경, 빌드 중, `build/`가 git 추적. 서명 키(`*.jks`, `*.keystore`)는 항상 제외 | https://developer.android.com/build/building-cmdline · https://developer.android.com/ndk/guides/cmake |
| Android | 디버그 키스토어·adb 키 | `~/.android/debug.keystore`, `~/.android/adbkey`, `adbkey.pub` | 다시 생성되지만 내용이 바뀜(debug SHA-1 변경, USB 디버깅 재승인) | 아니오 | 없음 | **X** | `~/.android/avd`(C) 바로 옆 | 항상 제외 | https://developer.android.com/studio/publish/app-signing — "the IDE automatically creates the debug keystore and certificate in $HOME/.android/debug.keystore" |
| Android | SDK licenses | `$ANDROID_HOME/licenses/` | 라이선스 재수락 필요 | 아니오 | 없음 | **X** | SDK 루트에 함께 있음 | 항상 제외 | https://developer.android.com/studio/intro/update — "This licenses directory is necessary for Gradle to auto-download missing packages." |

### 3절 조사에서 나온 주의할 점

1. **DerivedData의 C는 출처 규칙 때문이다**: 실제 위험은 B 수준이다. Apple 공식 근거를 찾으면 B로 되돌린다. Locations를 Relative로 쓰면 프로젝트 안에 생기므로 기본 경로만 봐서는 안 된다.
2. **CoreSimulator는 파일 시스템으로 다루지 않는다**: 런타임은 SIP로 보호된 시스템 저장소에 마운트돼 있다. 런타임을 지우면 그 위 디바이스가 unavailable이 되므로 ① `xcrun simctl runtime delete` ② `xcrun simctl delete unavailable` 두 단계로 보여 준다. 어떤 작업이든 Xcode·Simulator 종료가 먼저다.
3. **이웃 경로는 경로 전체 일치로 구분한다**: `UserData/Previews`(먹이) vs `UserData/Provisioning Profiles`(X), `~/Library/Caches/org.swift.swiftpm`(A) vs `~/Library/org.swift.swiftpm/security`(X), `~/.android/avd`(C) vs `debug.keystore`·`adbkey`(X).
4. **재생성 가능해 보여도 사실상 불가한 것**: 오래된 OS의 DeviceSupport(그 OS 기기가 없으면), 배포 빌드의 Archives(Apple이 보관 요구), AVD·시뮬레이터 디바이스·이름 붙인 스냅샷(앱·데이터 손실).

### 3절에서 확인하지 못한 것

- DerivedData·Archives·`CoreSimulator/Devices`·`Caches`·`UserData/Previews`·DocumentationCache 경로의 Apple 공식 근거(DTS 포럼·커뮤니티만 있음)
- `simctl` 하위 명령의 온라인 공식 문서(`xcrun simctl help`, `man xcodebuild`로만 확인 가능), `simctl runtime delete`의 관리자 권한 필요 여부와 재부팅 후 재마운트 보고(포럼)
- 근거가 없어 뺀 것: `XCTestDevices`, `~/Library/Caches/com.apple.dt.Xcode`, `iOS Device Logs`, Xcode 16 developer disk images
- AVD `.ini`의 `path=`·`config.ini`의 `image.sysdir.1` 키, 스냅샷 경로, SDK 패키지를 휴지통에 넣은 뒤 SDK Manager 인식 여부
- `sdkmanager`·`avdmanager`는 deprecated, 후속 `android sdk remove`·`android emulator remove`의 인자 형식

---

## 4. 백엔드 (JVM · Go · Rust) · Docker

| 생태계 | 먹이 | 기본 경로/대상 (macOS) | 재생성 방법 | 휴지통 가능 | 권한 필요 | 안전 등급 | 감지 신호 | 제외 조건 | 출처 |
|---|---|---|---|---|---|---|---|---|---|
| JVM (Gradle) | 의존성 캐시 | `$GRADLE_USER_HOME/caches/modules-2` (기본 `~/.gradle/…`, Android·백엔드 공용) | 다음 빌드 때 원격에서 다시 받음 | 예 | 없음 | B | 크기, mtime. Gradle 자동 정리는 Gradle을 실행할 때만 동작 → 오래 안 쓴 Gradle 홈이 가장 큰 먹이 | 데몬 실행 중(`gradle --status`), 빌드 중, 오프라인 작업 예정 | https://docs.gradle.org/current/userguide/directory_layout.html — "Downloaded module dependencies (POMs, JARs, metadata)." |
| JVM (Gradle) | 로컬 파생 캐시 | `~/.gradle/caches/build-cache-1`, `caches/jars-*`, `caches/transforms-*`, 버전별 `caches/<gradle-version>/` | 다음 빌드 때 로컬에서 다시 계산 | 예 | 없음 | A | 크기, 대응하는 `wrapper/dists`가 없는 버전 | 데몬 실행 중 | 위 문서 — "Default retention for unused build cache entries is 7 days." |
| JVM (Gradle) | 오래된 Wrapper 배포판 | `~/.gradle/wrapper/dists/gradle-<버전>-bin\|-all` | 그 버전 프로젝트에서 `./gradlew` 실행 시 다시 받음 | 예 | 없음 | B | 버전별 크기, 스캔 루트 프로젝트의 `gradle-wrapper.properties`가 참조하지 않는 버전 | 참조 중인 버전, 실행 중 데몬 버전 | 위 문서 — "Distributions downloaded by the Gradle Wrapper." |
| JVM (Gradle) | 데몬 로그 | `~/.gradle/daemon/<ver>/daemon-<pid>.out.log` (로그 파일만) | 새 데몬이 새 로그를 씀 | 예 | 없음 | A | 크기, 14일 초과 | 해당 pid 데몬 살아 있음. `daemon/`의 로그 외 파일(레지스트리)은 대상 아님 | 위 문서 — "Log files older than 14 days are automatically deleted" |
| JVM (Gradle) | 자동 프로비저닝 JDK | `~/.gradle/jdks/<jdk>` | auto-download가 켜져 있으면 다시 받음, 꺼져 있으면 빌드 실패 | 예 (공식 문서가 수동 삭제를 안내) | 없음 | B | 크기, toolchain 버전 불일치 | 데몬 실행 중, auto-download 비활성 | https://docs.gradle.org/current/userguide/toolchains.html — "remove the relevant toolchain located in the /jdks directory" |
| JVM (Gradle) | 설정 | `~/.gradle/gradle.properties`, `~/.gradle/init.d/`, `daemon/` 레지스트리 | — | 아니오 | 없음 | **X** | — | 항상 제외 | directory_layout — "Global Gradle configuration properties." |
| JVM (Gradle·Maven) | 프로젝트 빌드 산출물 | 스캔 루트 안 Gradle `build/`·프로젝트 `.gradle/`(`settings.gradle(.kts)`/`build.gradle(.kts)` 옆), Maven `target/`(`pom.xml` 옆) | 다음 빌드 때 재생성 | 둘 다 — 권장: 휴지통. `clean` 명령은 복구 불가 | 확인 필요 (Desktop/Documents/Downloads는 '파일 및 폴더' 권한 대상) | B | 크기, mtime, 마지막 커밋 | 커밋 안 된 변경(D5), 빌드·IDE·데몬 실행 중, 빌드 디렉터리 위치를 바꾼 프로젝트, 마커 파일 없는 `build/`·`target/` | directory_layout · https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html · https://support.apple.com/guide/mac-help/mchld5a35146/mac |
| JVM (Maven) | 로컬 저장소 | `~/.m2/repository` (`<localRepository>`) | 원격에서 다시 받음. 단 `mvn install`로 넣은 로컬 산출물은 다시 빌드해야 함 | 예 | 없음 | B | 크기, artifact별 mtime·다중 버전 | Maven 빌드 중. 로컬 install 산출물 의존 프로젝트는 개별 확인 | https://maven.apache.org/guides/introduction/introduction-to-repositories.html — "erase it completely if you are willing to download everything again" |
| JVM (Maven) | 설정·자격증명 | `~/.m2/settings.xml` 등 `~/.m2` 바로 아래 파일 전부 | — | 아니오 | 없음 | **X** | — | 항상 제외. `repository/`만 대상 | https://maven.apache.org/settings.html |
| JVM (SDKMAN) | 오래된 JDK | `~/.sdkman/candidates/java/<버전>` (`sdk home java <버전>`) | `sdk install java <버전>` | **아니오 — 도구 명령: `sdk uninstall java <버전>`** | 없음 | C (경로가 예시 출력에서만 확인돼 하향) | 설치 목록 vs `sdk current java` | 현재·기본 버전. 로컬 등록 버전 | https://sdkman.io/usage |
| Go | 모듈 캐시 | `GOMODCACHE` (기본 `~/go/pkg/mod`, `go env GOMODCACHE`) | 다음 `go build`/`go mod download` | 둘 다 — 권장: **`go clean -modcache`**(복구 불가). 파일이 읽기 전용이라 휴지통 경로가 까다로움 | 없음 | B | 크기. 자동 정리 없음 | go 빌드·테스트 중, 오프라인 작업, 인증 필요한 사설 모듈 | https://go.dev/ref/mod#module-cache — "The module cache has no maximum size, and the go command does not remove its contents automatically." |
| Go | 빌드·테스트 캐시 | `GOCACHE` (기본 `~/Library/Caches/go-build`) | 다음 빌드·테스트 때 재생성 | 둘 다 — 권장: 휴지통. `go clean -cache`는 복구 불가 | 없음 | A | 크기, mtime | 빌드·테스트 중. 퍼징 사용자는 개별 확인(퍼즈 입력이 같은 캐시 안) | https://pkg.go.dev/cmd/go — "The go command periodically deletes cached data that has not been used recently." |
| Go | 설치 바이너리·설정 | `$GOBIN`/`~/go/bin`, `go env GOENV`. `~/go` 루트 전체 | — | 아니오 | 없음 | **X** | — | 항상 제외. `pkg/mod`만 대상 | https://pkg.go.dev/cmd/go |
| Rust (Cargo) | 풀어둔 소스·git 체크아웃 | `$CARGO_HOME/registry/src`, `git/checkouts` (기본 `~/.cargo/…`) | 로컬 `.crate`·`git/db`에서 다시 풂(네트워크 불필요) | 예 | 없음 | A | 크기 | cargo 실행 중 | https://doc.rust-lang.org/cargo/guide/cargo-home.html — "you can always remove any part of the cache and Cargo will do its best to restore sources" |
| Rust (Cargo) | 다운로드 캐시 | `$CARGO_HOME/registry/cache`, `registry/index`, `git/db` | 네트워크에서 다시 받음 | 예 | 없음 | B | 크기. 1.88부터 3개월 미사용 자동 GC(cargo 실행 시) | cargo 실행 중, 오프라인 작업 | https://doc.rust-lang.org/cargo/reference/config.html#cache |
| Rust (Cargo) | 설정·자격증명·설치물 | `~/.cargo/bin`, `config.toml`, `credentials.toml`, `.crates.toml`, `.crates2.json` | — | 아니오 | 없음 | **X** | — | 항상 제외 | cargo-home 문서 — `credentials.toml`: "Private login credentials from cargo login" |
| Rust (Cargo) | 프로젝트 `target/` | 워크스페이스 루트 `target/`(`Cargo.toml` 옆, `CARGO_TARGET_DIR`) | 다음 `cargo build` 때 전체 재컴파일 | 둘 다 — 권장: 휴지통. `cargo clean`은 복구 불가 | 확인 필요 (보호 폴더 스캔 루트) | B | 크기, mtime, 마지막 커밋. 자동 GC 대상 아님 | 커밋 안 된 변경, 빌드 중, 공유 target 디렉터리는 개별 확인 | https://doc.rust-lang.org/cargo/commands/cargo-clean.html — "cargo clean will delete the entire target directory." |
| Rust (rustup) | 안 쓰는 툴체인 | `$RUSTUP_HOME`(기본 `~/.rustup`)의 툴체인 (하위 구조 미확인) | `rustup toolchain install` | **아니오 — 도구 명령: `rustup toolchain uninstall <toolchain>`** | 없음 | B | 설치 목록 vs default·`rust-toolchain.toml`, 오래된 nightly | default, override 참조, `toolchain link`로 연결한 커스텀 | https://rust-lang.github.io/rustup/examples.html |
| Docker | 댕글링 이미지 | VM 디스크 안 | 재생성 불필요 | **아니오 — 도구 명령: `docker image prune`** | 없음 | A | `docker system df` RECLAIMABLE, `<none>` 이미지 | Docker Desktop 꺼짐(측정 불가), 빌드 중 | https://docs.docker.com/engine/manage-resources/pruning/ — "By default, docker image prune only cleans up dangling images." |
| Docker | 안 쓰는 태그 이미지 | VM 디스크 안 | `docker pull` 또는 재빌드 | **아니오 — 도구 명령: `docker image prune -a`** (`--filter until=`) | 없음 | B | `docker system df -v` CONTAINERS 0, 오래된 CREATED | push 안 한 로컬 빌드 이미지, 레지스트리 접근 불가 | 위 pruning 문서 |
| Docker | 빌드 캐시 | VM 디스크 안(BuildKit) | 다음 `docker build` 때 재생성 | **아니오 — 도구 명령: `docker builder prune`** (`-a`는 사실상 B) | 없음 | A | Build Cache 행 표시 여부 실측 필요 | 빌드 중 | https://docs.docker.com/reference/cli/docker/builder/prune/ |
| Docker | 정지된 컨테이너 | VM 디스크 안 | **재생성 불가** — 쓰기 레이어 변경분 영구 손실 | **아니오 — 도구 명령: 개별 `docker rm <id>`** (일괄 prune 금지) | 없음 | C | `docker system df -v` STATUS Exited, SIZE | 자동 제안 금지. 볼륨 없이 띄운 DB 등 | 위 pruning 문서 — "A stopped container's writable layers still take up disk space." |
| Docker | 볼륨 | VM 디스크 안 | **재생성 불가**(DB 등 사용자 데이터) | **아니오 — 도구 명령: 개별 `docker volume rm <이름>`** | 없음 | C | `docker volume ls -f dangling=true`, LINKS | 자동 제안 금지. 어떤 컨테이너든(정지 포함) 참조 중이면 제외 | 위 pruning 문서 — "Volumes are never removed automatically, because to do so could destroy data." |
| Docker | VM 디스크 이미지 파일 | `~/Library/Containers/com.docker.docker/Data/vms/0/data/Docker.raw` (위치 변경 가능, 하드코딩 금지) | — | 아니오 — 절대 이동 금지. 공간 회수는 prune 명령으로 | 확인 필요 (아래 주의 3) | **X** | 할당 크기(`du`). 논리 크기는 최대 디스크 크기라 변하지 않음 | 항상 제외 | https://docs.docker.com/desktop/troubleshoot-and-support/faqs/macfaqs/ — "Do not move the file directly in Finder as this can cause Docker Desktop to lose track of the file." |
| Docker | CLI 설정·자격증명 | `~/.docker/config.json` 및 `~/.docker/` 전체 | — | 아니오 | 없음 | **X** | — | 항상 제외 | https://docs.docker.com/reference/cli/docker/login/ |

### 4절 조사에서 나온 주의할 점

1. **Docker 공간은 두 단계로 돌아온다**: prune은 VM 디스크 안을 비우고, 호스트의 `Docker.raw`는 몇 분 뒤 줄어든다. Finder·`ls -l`은 최대 크기를 보여 주므로 전후 비교는 할당 크기(`du`)나 `docker system df`로 한다.
2. **`docker system prune` 한 방 버튼을 두지 않는다**: 정지된 컨테이너의 쓰기 레이어(볼륨 없는 DB)까지 지우고 `--volumes`면 볼륨도 지운다. 이미지·빌드 캐시·컨테이너·볼륨을 행별로 나눠 각각 2차 확인한다(US-DIGEST-002).
3. **다른 앱 데이터 컨테이너 접근 프롬프트**: Apple WWDC23 — "macOS Sonoma will ask for permission when your app accesses a file in another app's data container." (https://developer.apple.com/videos/play/wwdc2023/10053/) `com.docker.docker`가 보호 대상인지는 미확인. 그래서 Docker 크기는 `~/Library/Containers`를 열거하지 않고 `docker system df`로 잰다(R-08).
4. **즉시 확보되는 것은 도구 명령뿐이다**: `go clean`, `rustup`, `sdk`, Docker prune은 즉시 공간이 돌아오지만 복구할 수 없다. 휴지통 먹이는 비워야 확보된다(C2).
5. **같은 이름의 디렉터리와 경로 재지정**: `target/`은 Maven·Cargo가 함께 쓰고, `build/`는 여러 도구가 쓴다 → 마커 파일(`pom.xml`, `Cargo.toml`, `build.gradle(.kts)`)로 판정한다. 경로는 도구에 물어 확정한다(`go env GOMODCACHE GOCACHE` 등). Gradle·Cargo 자동 GC는 그 도구를 실행할 때만 돌기 때문에 오래 안 쓴 도구의 캐시가 가장 큰 먹이일 가능성이 높다.

### 4절에서 확인하지 못한 것

- `com.docker.docker`가 앱 데이터 컨테이너 보호 대상인지, `Docker.raw` 자동 축소 원리, 빌드 캐시가 `docker system df`에 나오는지
- `GOTOOLCHAIN`으로 받은 툴체인이 모듈 캐시에 들어가는지(`go clean -modcache`가 툴체인도 지우는지), 퍼즈 캐시 경로, Go 모듈 캐시 휴지통 이동 후 비우기 시 권한 문제(추론)
- rustup 툴체인 하위 구조, Maven 로컬 install 산출물 구분 방법, Maven Wrapper 배포판 경로, SDKMAN `sdk flush` 범위
- Cargo·Gradle이 `CACHEDIR.TAG`를 만드는지(확인되면 좋은 감지 신호)
- `~/.gradle`·`~/.m2`·`~/go`·`~/.cargo`가 macOS 권한 보호 대상인지(근거 없어 `없음`으로 기록)

---

## 5. IDE · AI 도구 · 공통 개발 도구

| 생태계 | 먹이 | 기본 경로/대상 (macOS) | 재생성 방법 | 휴지통 가능 | 권한 필요 | 안전 등급 | 감지 신호 | 제외 조건 | 출처 |
|---|---|---|---|---|---|---|---|---|---|
| JetBrains | 현재 버전 system(캐시) 디렉터리 | `~/Library/Caches/JetBrains/<Product><version>` | IDE가 캐시·인덱스 재생성(재인덱싱). **Local History는 재생성 안 됨** | 둘 다 — 권장: IDE의 Invalidate Caches(Local History 기본 보존). 폴더 휴지통은 Local History까지 사라짐 | 없음 | C | 크기, 디렉터리명 버전 = 설치 버전 | IDE 실행 중. 자동 제안 금지 | https://www.jetbrains.com/help/idea/directories-used-by-the-ide-to-store-settings-caches-plugins-and-logs.html — "The IntelliJ IDEA system directory contains caches and local history files." |
| JetBrains | 구버전 system(캐시) 디렉터리 | `~/Library/Caches/JetBrains/<Product><구버전>` | 재생성 안 됨(현재 버전은 안 씀) | 예 | 없음 | B | 설치 버전보다 낮은 버전명, 오래된 mtime(JetBrains 자체 기준 180일) | 그 버전이 아직 설치됨(Toolbox 롤백 포함). 실행 중 | 위 문서 — 새 메이저 버전이 "deletes the caches and logs directories for older versions … not been updated in the last 180 days" |
| JetBrains | 로그 디렉터리 | `~/Library/Logs/JetBrains/<Product><version>` | IDE가 새 로그를 씀 | 예 | 없음 | A | 크기, 구버전 이름, mtime | 현재 버전 IDE 실행 중 | 위 문서 — "contains product logs and thread dumps" |
| JetBrains | 구버전 설정·플러그인 | `~/Library/Application Support/JetBrains/<Product><구버전>` | 재생성 안 됨 | 예 (IDE의 Delete Leftover IDE Directories도 가능) | 없음 | C | 설치되지 않은 낮은 버전명 | 그 버전 설치됨. 새 버전을 아직 실행 안 함(설정 이전 전일 수 있음 — 추론) | 위 문서 — "The configuration and plugins directories will remain forever, unless you remove them manually." |
| JetBrains | 현재 버전 설정·플러그인 | `~/Library/Application Support/JetBrains/<Product><version>` | 재생성 안 됨 | 아니오 | 없음 | **X** | 버전명 = 설치 버전 | 항상 제외 | 위 문서 — "contains user-defined IDE settings, such as keymaps, color schemes" |
| JetBrains Toolbox | 롤백용 이전 IDE 버전, 다운로드 캐시 | **경로 미확인**. 확인된 것은 설정 `~/Library/Application Support/JetBrains/Toolbox/.settings.json`(X), 로그 `~/Library/Logs/JetBrains/Toolbox/` | Toolbox가 다시 받음. 즉시 롤백은 사라짐 | 아니오 — CLI 없음. Toolbox 설정에서 정리하도록 **안내만** | 없음 | C (미확인으로 하향) | Toolbox "Keep previous versions" 설정 | Toolbox·IDE 실행 중 | https://www.jetbrains.com/help/toolbox-app/frequently-asked-questions.html |
| VS Code | 앱 캐시(`Cache`, `Code Cache`, `CachedData`, `CachedExtensionVSIXs`, `GPUCache`) | `~/Library/Application Support/Code/<하위 폴더>` | VS Code가 다시 만든다고 커뮤니티가 설명 | 예 | 없음 | B (A → 미확인으로 하향) | 크기, mtime | VS Code 실행 중. `Code/`·`Code/User/` 통째 금지 | **미확인.** 공식 문서는 이 폴더 전체를 사용자 데이터로 다룸: https://code.visualstudio.com/docs/setup/uninstall |
| VS Code (C/C++ 확장) | IntelliSense 캐시 | `~/Library/Caches/vscode-cpptools/` (`C_Cpp.intelliSenseCachePath`) | 확장이 다시 채움 | 예 | 없음 | A | 크기, mtime | C/C++ 작업 중 | https://code.visualstudio.com/docs/cpp/faq-cpp — "`$HOME/Library/Caches/vscode-cpptools/` on macOS" |
| VS Code | 사용자 설정·프로필·설치된 확장 | `~/Library/Application Support/Code/User/`, `~/.vscode/extensions`, `~/.vscode-shared` | 재생성 안 됨 | 아니오 | 없음 | **X** | — | 항상 제외 | https://code.visualstudio.com/docs/configure/settings · https://code.visualstudio.com/docs/configure/extensions/extension-marketplace |
| Ollama | 로컬 모델 | `~/.ollama/models` (`OLLAMA_MODELS`, macOS 앱은 `launchctl`로 설정) | `ollama pull`(수 GB) | 둘 다 — 권장: `ollama rm <model>`(복구 불가, 2차 확인). 개별 blob 휴지통 금지 | 없음 | B | `ollama ls` 크기·수정일, `ollama ps`에 없음 | `ollama ps`에 로드됨. 앱/서버 실행 중 폴더 이동 | https://docs.ollama.com/faq — "macOS: ~/.ollama/models" · https://docs.ollama.com/cli |
| Ollama | 모델 외 파일(키) | `~/.ollama/id_ed25519*` — **미확인** | 다시 만들면 계정 연결이 끊길 수 있음(서드파티 정보) | 아니오 | 없음 | **X** | — | `~/.ollama`를 통째로 제안하지 않음. 대상은 `models/`뿐 | 미확인 |
| LM Studio | 다운로드한 모델 | `~/.lmstudio/models/<publisher>/<model>/` (앱 My Models에서 변경) | 앱에서 다시 받음(수 GB) | 예 | 없음 | B | `lms ls`, 크기, 접근 시각 | LM Studio 실행 중·모델 로드 중 | https://lmstudio.ai/docs/app/advanced/import-model · https://lmstudio.ai/docs/app/basics/download-model |
| Claude Code | 워크트리(`--worktree`, 서브에이전트, Desktop) | `<repo>/.claude/worktrees/<name>/`, 브랜치 `worktree-<name>` (Desktop 설정으로 위치 변경) | 재생성 안 됨 | 둘 다 — 권장: 아래 git 공통 규칙. 브랜치 삭제는 캐슈가 하지 않음 | 확인 필요 | C | `locked` 없음, clean, push 안 된 커밋 없음 | `locked`(실행 중). dirty·untracked·미푸시. 재개 가능한 세션 | https://code.claude.com/docs/en/worktrees — "the worktree is created under `.claude/worktrees/<name>/`" |
| Claude Code | 로그·레거시 부산물 | `~/.claude/debug/`, `session-env/`, `shell-snapshots/`, 레거시 `todos/`, `statsig/`, `logs/`, `image-cache/` (`CLAUDE_CONFIG_DIR`) | 필요할 때 다시 씀. 레거시는 더 안 씀 | 예 | 없음 | A | 크기, `cleanupPeriodDays`(기본 30일)보다 오래됨 | Claude Code 실행 중 | https://code.claude.com/docs/en/claude-directory.md — "Nothing user-facing" / "Legacy directories not written by current versions." |
| Claude Code | 대화 기록·체크포인트 | `~/.claude/projects/<project>/`, `~/.claude/file-history/` | 재생성 안 됨 | 둘 다 — 권장: 휴지통. `claude purge`는 복구 불가 | 없음 | C | 크기, 30일 넘은 파일 | **`projects/<p>/memory/`는 항상 제외.** resume/rewind 필요 | 위 문서 — 삭제 시 "Resume, continue, and rewind for past sessions, and auto memory"를 잃음 |
| Claude Code | 설정·자격증명·상태 | `~/.claude.json`, `~/.claude/settings.json`, `plugins/`, `.credentials.json`, `agent-memory/`, `jobs/`, `daemon/`, `CLAUDE.md`·`skills/`·`agents/`, 프로젝트 `.claude/settings*.json` | 재생성 안 됨 | 아니오 | 없음 | **X** | — | 항상 제외 | 위 문서 — "Don't delete `~/.claude.json`, `~/.claude/settings.json`, or `~/.claude/plugins/`" |
| OpenAI Codex | 앱 관리 워크트리 | `$CODEX_HOME/worktrees` (기본 `~/.codex/worktrees`) | Codex가 지울 때만 스냅샷 복원 가능. 캐슈가 지우면 재생성 안 됨 | 둘 다 — 권장: **Codex에서 채팅 보관(archive)하도록 안내**. 직접 처리는 git 공통 규칙 | 없음 | C | git 신호, 기본 보관 수(15개) 초과 | 고정 채팅, 진행 중, 영구 워크트리, dirty·미푸시 | https://learn.chatgpt.com/docs/environments/git-worktrees — "Before deleting a Codex-managed worktree, Codex saves a snapshot" |
| OpenAI Codex | 로그 | `$CODEX_HOME/log` (`log_dir`) | 새 로그를 씀 | 예 | 없음 | A | 크기, mtime | Codex 실행 중 | https://learn.chatgpt.com/docs/config-file/config-reference |
| OpenAI Codex | 설정·자격증명·기록 | `~/.codex/config.toml`, `auth.json`, `history.jsonl`, SQLite 상태 | 재생성 안 됨 | 아니오 | 없음 | **X** | — | 항상 제외 | https://learn.chatgpt.com/docs/auth — "treat `~/.codex/auth.json` like a password" |
| Windsurf (Devin Desktop) | Cascade 워크트리 | `~/.windsurf/worktrees/<repo_name>/` | 재생성 안 됨 | 둘 다 — 권장: **앱에서 대화 삭제하도록 안내**(앱이 워크트리도 지움) | 없음 | C | git 신호, 마지막 접근, 워크스페이스당 20개 초과 | 대화 남아 있음·앱 실행 중. dirty·미푸시 | https://docs.devin.ai/desktop/cascade/worktrees |
| Conductor | 워크스페이스(워크트리) | 신규 `~/conductor/workspaces/`, 구버전 `<repo>/.conductor/<name>` | archive하면 Conductor가 git 상태 저장 | 둘 다 — 권장: **Conductor에서 archive하도록 안내** | 신규 없음, 구버전 확인 필요 | C | git 신호, mtime | Conductor 실행 중. archive 안 됨. dirty·미푸시 | https://www.conductor.build/docs/tips/nesting-issues · https://www.conductor.build/changelog/0.33.5-better-archiving |
| Homebrew | 다운로드 캐시 | `brew --cache` (기본 `~/Library/Caches/Homebrew`, `HOMEBREW_CACHE`) | install/upgrade 때 다시 받음 | 둘 다 — 권장: 휴지통. `brew cleanup --prune=all`은 복구 불가 | 없음 | A | 크기, 120일 넘은 파일, `brew cleanup -n` | brew 실행 중(lock) | https://docs.brew.sh/Manpage — HOMEBREW_CACHE "macOS: `~/Library/Caches/Homebrew`" |
| Homebrew | 설치된 포뮬러 구버전 | `brew cleanup -n`이 보고하는 Cellar 이전 버전 | 구버전 재설치가 보장되지 않음(추론) | **아니오 — 도구 명령: `brew cleanup`**(복구 불가) | 없음 | B | `brew cleanup -n` | brew 실행 중. `HOMEBREW_NO_INSTALL_CLEANUP` 설정 | https://docs.brew.sh/Manpage — "remove old versions of installed formulae" |
| git (공통) | 오래된 linked 워크트리 | 최상위에 `.git` **파일**(`gitdir: …/.git/worktrees/<id>`)이 있는 디렉터리. 소속 repo에서 `git worktree list --porcelain` | 재생성 안 됨. ignored 파일(.env, 산출물)은 사라짐 | 둘 다 — 권장: 휴지통 이동 후 `git worktree prune -n`으로 대상이 그 항목 하나일 때만 prune. `git worktree remove`(`--force` 금지)는 ignored 파일까지 영구 삭제 | 확인 필요 | C | ① `status --porcelain=v2 --branch --untracked-files=all`에 변경·untracked 없음 ② upstream 있고 ahead 0 ③ `--ignored` 결과를 사용자에게 보여 줌 ④ 브랜치가 기본 브랜치에 merge됐거나 원격 삭제 ⑤ 오래 수정 안 됨 | dirty·untracked(하드 룰). ahead>0 또는 upstream 없음. `locked`. 다른 곳에 없는 detached HEAD. 서브모듈. IDE·에이전트가 열어 둠 | https://git-scm.com/docs/git-worktree — "Only clean worktrees (no untracked files and no modification in tracked files) can be removed." |
| git (공통) | 메인 워크트리, locked·bare | `.git`이 **디렉터리**인 저장소 루트, porcelain `locked`/`bare` | 재생성 안 됨 | 아니오 | 확인 필요 | **X** | `.git`이 디렉터리, `locked`/`bare` | 항상 제외. 메인을 지우면 linked 워크트리 전부가 연결을 잃음 | https://git-scm.com/docs/git-worktree — "The main worktree cannot be removed." |

### 5절 조사에서 나온 주의할 점

1. **git의 clean 판정은 D5보다 좁다**: git은 tracked 수정과 untracked만 본다. push 안 된 커밋, upstream 없음, detached HEAD, gitignore된 파일(예: 에이전트가 복사한 `.env`)은 보지 않는다 → 캐슈가 별도로 검사하고, upstream이 없으면 전부 미푸시로 간주한다.
2. **휴지통 방식은 git 메타데이터 정리가 따로 필요하다**: 휴지통으로 옮기면 `$GIT_DIR/worktrees/<id>`가 남아 `prunable`이 된다. `git worktree prune`은 그 repo에서 사라진 워크트리를 **전부** 정리하므로(언마운트된 외장 디스크 위 워크트리 포함) 반드시 `-n`으로 먼저 확인한다.
3. **도구가 관리하는 워크트리는 도구의 정리 경로가 더 안전하다**: Codex(스냅샷), Conductor(archive 시 미커밋 포함 저장), Windsurf(대화 삭제), Claude Desktop(보관 후 자동 제거). 캐슈가 먼저 지우면 이 복원 경로가 사라진다.
4. **도구들도 같은 대상을 스스로 정리한다**: Cursor(머신당 25개), Windsurf(워크스페이스당 20개), Claude Code(`cleanupPeriodDays`) → 스캔 결과가 금방 낡으므로 실행 직전 재검증이 필수다(NFR-S3).
5. **도구가 알려 주는 경로를 하드코딩 경로보다 우선한다**: `brew --cache`, `ollama ls`, `lms ls`, `git worktree list --porcelain`. 위치 변경 설정이 많다(`OLLAMA_MODELS`, `CODEX_HOME`, `CLAUDE_CONFIG_DIR` 등).

### 5절에서 확인하지 못한 것

- Cursor 워크트리 경로(`~/.cursor/worktrees`는 포럼 답변에만 있음 → git 공통 규칙으로 감지)
- VS Code 하위 캐시 폴더의 공식 설명, Cursor·Windsurf 설정 경로, JetBrains Toolbox 롤백·다운로드 캐시 경로
- Ollama `id_ed25519`(서드파티 정보만), blob 공유 여부(추론), LM Studio 공식 삭제 명령
- Codex `sessions/`·스냅샷 위치, Conductor archive 시 디렉터리 삭제 여부
- Cursor·Windsurf 자동 정리가 dirty 워크트리를 보호하는지 (`git worktree remove`의 ignored 파일 처리는 아래 "실험 확인" E-1에서 확인함)
- ~/Desktop·~/Documents·iCloud·외장 볼륨 repo 접근 시 macOS 권한 프롬프트, Homebrew pinned 포뮬러의 cleanup 제외 여부

---

## 카탈로그 공통 규칙

다섯 묶음의 "주의할 점"에서 겹치는 것을 스캐너·실행기 규칙으로 모았다. NFR·스토리는 이 ID를 참조한다.

| ID | 규칙 | 나온 곳 | 반영 |
|---|---|---|---|
| CR-1 | **경로 전체 일치.** 접두사 매치를 쓰지 않는다. 먹이와 X가 이웃한 예: `UserData/Previews` vs `UserData/Provisioning Profiles`, `~/Library/Caches/org.swift.swiftpm` vs `~/Library/org.swift.swiftpm/security`, `~/.android/avd` vs `~/.android/debug.keystore`, `huggingface/hub` vs `huggingface/token` | 1·2절 1, 3절 3 | NFR-S8 |
| CR-2 | **하위 경로 단위로 정의한다.** 부모 폴더 하나를 먹이로 두지 않는다(`pypoetry/virtualenvs`, `bun/install/cache/links/` 같은 상태가 섞여 있음) | 1·2절 1 | NFR-S1 |
| CR-3 | **도구가 알려 주는 경로를 먼저 쓴다.** `brew --cache`, `go env GOMODCACHE GOCACHE`, `pnpm store path`, `uv cache dir`, `git worktree list --porcelain`, `xcrun simctl list -j`, `docker system df`, `ollama ls`. 하드코딩한 기본 경로는 도구가 없을 때만 쓰고, 둘이 다르면 도구 쪽을 믿는다 | 4절 5, 5절 5 | R-04 |
| CR-4 | **마커 파일로 판정한다.** 같은 이름의 디렉터리(`target/`, `build/`, `.build/`)는 옆의 마커(`pom.xml`, `Cargo.toml`, `build.gradle(.kts)`, `Package.swift`)로 어느 도구 것인지 정하고, 마커가 없으면 제안하지 않는다 | 4절 5 | US-FORAGE-002 |
| CR-5 | **"사용 중" 판정은 실행 중인 프로세스의 실행 파일 경로로 한다.** 메뉴바 앱은 셸의 `VIRTUAL_ENV`·`NVM_DIR` 같은 환경을 볼 수 없다 | 1·2절 4 | US-FORAGE-004 |
| CR-6 | **표시 크기와 확보량을 나눈다.** pnpm·Bun·uv(clone)·conda(하드링크)는 블록을 공유하고, 휴지통 먹이는 비워야 확보된다. "최대 N GB"로 보여 주고, 실제 확보량은 먹기 전후 볼륨 여유 공간 차이로 기록한다 | 1·2절 2, 4절 1·4 | US-DIGEST-003, R-14 |
| CR-7 | **도구가 관리하는 대상은 도구로 처리한다.** CoreSimulator(`xcrun simctl`), Android SDK(SDK Manager·`sdkmanager`), Docker(prune 명령, 행별), uv(`uv cache prune`), Homebrew 구버전(`brew cleanup`). 휴지통에 넣을 수 없거나, 넣으면 도구 상태와 어긋난다 | 1·2절 3, 3절 2, 4절 2 | US-DIGEST-002 |
| CR-8 | **도구가 관리하는 워크트리는 그 도구의 정리 경로를 안내한다.** Codex(보관), Conductor(archive), Windsurf(대화 삭제), Claude Desktop. 캐슈가 먼저 지우면 그 도구의 복원 경로가 사라진다 | 5절 3 | US-FORAGE-006, R-15 |
| CR-9 | **git 워크트리의 clean은 D5보다 넓게 본다.** tracked 수정·untracked 외에 ahead > 0, upstream 없음, `locked`, 다른 곳에 없는 detached HEAD, 서브모듈도 제외한다. gitignore된 파일은 목록으로 보여 준다 | 5절 1 | US-FORAGE-002, US-FORAGE-006 |
| CR-10 | **Docker는 한 방 버튼을 두지 않는다.** 이미지·빌드 캐시·정지 컨테이너·볼륨을 행별로 나눠 각각 두 번째 확인을 받는다. 크기는 `~/Library/Containers`를 열거하지 않고 `docker system df`로 잰다 | 4절 2·3 | US-DIGEST-002, R-08 |

## 검증

### 기계 검증 (2026-10-30)

- 표 5개, 데이터 행 **106개**. 모든 행이 10열이고(`\|` 이스케이프 반영), 출처 칸이 빈 행은 없다.
- 출처가 `미확인`이거나 포럼 전언뿐인 행 7개(DerivedData, 시뮬레이터 디바이스, CoreSimulator Caches, Xcode Previews, JetBrains Toolbox 롤백, VS Code 앱 캐시, Ollama 키)는 모두 한 단계 낮췄거나 원래 C·X다.

| 절 | A | B | C | X | 합계 |
|---|---|---|---|---|---|
| 1. 프론트/Node | 10 | 3 | 3 | 1 | 17 |
| 2. Python/ML | 5 | 5 | 1 | 3 | 14 |
| 3. iOS/Xcode · Android | 3 | 10 | 8 | 4 | 25 |
| 4. 백엔드 · Docker | 6 | 10 | 3 | 6 | 25 |
| 5. IDE · AI 도구 · 공통 | 5 | 5 | 9 | 6 | 25 |
| **합계** | **29** | **33** | **24** | **20** | **106** |

### 출처 대조 (원문 직접 확인)

각 묶음 조사와 별개로, 판단을 바꾸는 근거 13개를 이번 세션에 원문에서 다시 열어 인용문과 앞뒤 문맥을 확인했다. Apple 문서는 문서 JSON(`developer.apple.com/tutorials/data/documentation/...`)으로 읽었다.

| # | 행 | 출처 | 확인한 문구 | 결과 | 판단에 주는 영향 |
|---|---|---|---|---|---|
| V-1 | uv 캐시 | https://docs.astral.sh/uv/concepts/cache/ | "it's never safe to modify the cache directly (e.g., by removing a file or directory)" | 일치 | 휴지통 불가 → 도구 명령형 |
| V-2 | pnpm 스토어 | https://pnpm.io/settings/store | "On macOS: ~/Library/pnpm/store" | 일치 | 기본 경로 |
| V-3 | pnpm 메타데이터 캐시 | https://pnpm.io/settings/other | "On macOS: ~/Library/Caches/pnpm" | 일치 | 기본 경로 |
| V-4 | pnpm 스토어 | https://pnpm.io/cli/store | "Running pnpm store prune is not harmful and has no side effects on your projects." | 일치 | 권장 처리 방식 |
| V-5 | Docker VM 디스크 | https://docs.docker.com/desktop/troubleshoot-and-support/faqs/macfaqs/ | `~/Library/Containers/com.docker.docker/Data/vms/0/data`의 `Docker.raw` 예시와 할당 크기·최대 크기 설명 | 일치 | X 등급, 크기는 `du`·`docker system df` |
| V-6 | git 워크트리 | https://git-scm.com/docs/git-worktree | "Only clean worktrees (no untracked files and no modification in tracked files) can be removed." | 일치 | git의 clean에 미푸시 커밋이 없음 → CR-9 |
| V-7 | 메인 워크트리 | https://git-scm.com/docs/git-worktree | "The main worktree cannot be removed." | 일치 | X 등급 |
| V-8 | Android SDK licenses | https://developer.android.com/studio/intro/update | "This licenses directory is necessary for Gradle to auto-download missing packages." | 일치 | X 등급 |
| V-9 | Android system-images | https://developer.android.com/studio/run/emulator-commandline | "~/Library/Android/sdk/system-images/android-apiLevel/variant/arch/" | 일치 | 기본 경로 |
| V-10 | Claude Code 워크트리 | https://code.claude.com/docs/en/worktrees | "the worktree is created under .claude/worktrees/<name>/ … on a new branch named worktree-<name>" | 일치 | 감지 경로 |
| V-11 | DeviceSupport | https://developer.apple.com/documentation/xcode/acquiring-operating-system-symbol-information | "~/Library/Developer/Xcode/<iOS\|watchOS\|tvOS\|visionOS> DeviceSupport" | 일치 | 기본 경로 |
| V-12 | Xcode Archives | https://developer.apple.com/documentation/xcode/building-your-app-to-include-debugging-information | "You must retain the Xcode archive for each build of your app you distribute." | 일치 | C 등급, 자동 제안 금지 |
| V-13 | 다른 앱 데이터 컨테이너 (4절 주의 3, R-08) | https://developer.apple.com/videos/play/wwdc2023/10053/ (세션 스크립트) | "macOS Sonoma will ask for permission when your app accesses a file in another app's data container. The permission is valid for as long as your app remains open, and once the app is quit, the permission resets." 보호 대상은 "an application data container from a different developer" | 일치 | 컨테이너는 열거하지 않고 도구 명령으로 잰다(CR-10). 권한은 앱을 다시 켜면 초기화된다 |

결과: 13건 모두 일치, 불일치 0건.

### 실험 확인 (2026-10-30, git 2.54.0 Apple Git-157)

임시 폴더에 저장소를 만들어 `git worktree remove`(`--force` 없이)를 실행했다. 실험 후 임시 폴더는 지웠다.

| # | 워크트리 상태 | remove 결과 | 반영 |
|---|---|---|---|
| E-1 | `.gitignore` 대상 파일(`.env`)만 있음 | 성공. `.env`가 **경고 없이 함께 삭제됨** | 먹기 전에 ignored 파일 목록 표시 (CR-9, US-FORAGE-006) |
| E-2 | 브랜치에 push 안 한 커밋 | 성공. 커밋은 브랜치에 남음(손실 없음) | ahead > 0 제외는 데이터 손실 방지가 아니라 "아직 작업 중" 신호로 보는 보수적 규칙 |
| E-3 | detached HEAD에서 만든 커밋 | 성공. 그 커밋을 가리키는 ref가 없어져 `git fsck --unreachable`에 나옴 | 다른 곳에 없는 detached HEAD 제외 (CR-9) |

### 검증하지 못한 것

- 나머지 행의 출처는 각 묶음 조사에서 한 번 확인한 것이고, 이번 대조 대상이 아니다.
- 실제 Mac에서 경로 존재·크기를 재지 않았다(조사 조건: 로컬 파일 시스템 조회 금지). BOLT-01·02에서 실측한다.
- 각 절의 "확인하지 못한 것" 목록은 그대로 남아 있다. 특히 macOS 권한 프롬프트(R-08), 메뉴바 앱의 환경 변수 상속(R-04), 앱이 휴지통에 넣은 항목의 Finder 되돌려 놓기(R-07)는 Construction에서 실행으로 확인해야 한다.
