---
artefact_type: risk_register
unit: global
status: draft
created_at: 2026-10-30
---

# Risk Register

가능성·영향은 H/M/L. "확인 필요"는 이번 Inception에서 검증하지 않은 가정이다.

| ID | 리스크 | 가능성 | 영향 | 대응 | 관련 |
|---|---|---|---|---|---|
| R-01 | **오삭제**: 사용자 데이터가 섞인 먹이(Docker 볼륨, 시뮬레이터 기기, Xcode Archives, push 안 된 커밋이 있는 worktree 등)를 지움 | M | H | 안전 등급 C는 자동 제안 금지·개별 확인, X는 deny-list, 실행 직전 재검증, 기본은 휴지통 | NFR-S1~S4, D5 |
| R-02 | **복구 불가 먹이 실행 후 후회**: 도구 명령형(C3)은 휴지통에 남지 않음 | M | H | 복구 불가 표시 + 명령 미리보기 + 2단계 확인, 경험치 0.5배(US-GROW-001, 확정) | NFR-S6, C3 |
| R-03 | **경로·형식 변경**: 도구 버전 업데이트로 캐시 경로나 구조가 바뀜 | M | M | 카탈로그를 데이터로 분리·버전 관리, 감지 신호가 맞지 않으면 제안하지 않음(모르면 안 먹는다) | NFR-O1 |
| R-04 | **환경 변수로 위치가 바뀐 캐시**(`GOMODCACHE`, `CARGO_HOME`, `HF_HOME` 등)를 놓치거나 잘못된 위치를 먹음 | M | M | 경로는 도구에 물어서 정한다(`brew --cache`, `go env GOMODCACHE` 등, 카탈로그 CR-3·NFR-O3). 단 메뉴바 앱은 셸의 `PATH`·환경 변수를 상속하지 않을 수 있어 도구 실행 파일을 못 찾거나 셸과 다른 값을 받을 수 있다(**확인 필요**, BOLT-01에서 `brew --cache`로 실측). 못 찾으면 기본 경로 + 사용자 설정 재정의 | 카탈로그 CR-3 |
| R-05 | **도구 실행 중 삭제로 인한 손상**(Xcode 빌드 중 DerivedData, Gradle 데몬, Docker 실행 중) | M | M | 관련 프로세스 실행 중이면 제외(카탈로그 제외 조건) | NFR-S3 |
| R-06 | **휴지통 공간 오해**: 먹었는데 디스크가 안 늘어나 실망 | H | M | "휴지통 비우면 N GB 확보" 안내(C2), 확보 예정 용량과 실제 확보 용량을 구분해 표시 | C2 |
| R-07 | **앱으로 휴지통에 보낸 항목의 Finder '되돌려 놓기' 동작 여부 불명** | M | M | **확인 필요.** 안 되면 먹은 기록의 휴지통 경로로 앱이 직접 되돌리기 제공. Construction 첫 Bolt에서 검증 | NFR-S7 |
| R-08 | **권한 프롬프트·거부**: 다른 앱 데이터 컨테이너(Docker Desktop 등)나 `~/Desktop`·`~/Documents`·iCloud 아래 스캔 루트에 접근하면 macOS가 권한을 물을 수 있음. 근거: Apple WWDC23 "macOS Sonoma will ask for permission when your app accesses a file in another app's data container." (https://developer.apple.com/videos/play/wwdc2023/10053/). 권한은 앱이 켜져 있는 동안만 유지되고 앱을 끄면 초기화된다(카탈로그 V-13). `com.docker.docker`가 보호 대상인지는 미확인 | M | L | 다른 앱 컨테이너는 열거하지 않고 도구 명령으로 잰다(카탈로그 CR-10). 필요할 때만 요청, 거부되면 해당 먹이만 건너뜀. 스캔 루트 프롬프트는 BOLT-03에서 확인 | NFR-A2 |
| R-09 | **symlink·마운트로 allowlist 밖이 이동됨** | L | H | symlink 미추적, 실제 경로 재검사 | NFR-S4 |
| R-10 | **성장 요소가 위험한 먹기를 부추김**(경험치 때문에 필요한 걸 지움) | M | M | 등급 C·복구 불가 먹이의 경험치는 0.5배(US-GROW-001, 확정) | D7 |
| R-11 | **배포 신뢰**: 서명·공증이 없으면 동료가 설치를 꺼림. Developer ID에는 Apple Developer Program 멤버십이 필요 | M | M | 서명·공증 필수(NFR-D1). **어느 계정(개인/회사)으로 서명할지 미정** — Open Question | NFR-D1 |
| R-12 | **외부 LLM 연결 시 메타데이터 노출**(경로에 프로젝트명·회사명 포함) | L | M | MVP 범위 밖. 도입 시 기본 꺼짐, 전송 미리보기, 파일 내용 전송 금지 | NFR-P2, D6 |
| R-13 | **대형 트리 스캔 부하**(수백만 파일의 node_modules)로 배터리·발열 | M | L | 낮은 QoS, 취소 가능, 결과 캐시 | NFR-F2, F4 |
| R-14 | **표시 크기와 실제 확보량의 차이**: pnpm·Bun·uv(clone)·conda(하드링크)는 블록을 공유해 폴더 크기만큼 공간이 돌아오지 않음 → 실망, M-2 과대 집계 | M | M | "최대 N GB"로 표시, 실제 확보량은 먹기 전후 볼륨 여유 공간 차이로 기록(카탈로그 CR-6) | US-DIGEST-003, M-2 |
| R-15 | **도구가 관리하는 워크트리를 캐슈가 먼저 지움**: Codex 스냅샷·Conductor archive 같은 도구의 복원 경로가 사라짐. 도구 자신의 자동 정리(Cursor 25개, Windsurf 20개 등)와 겹쳐 스캔 결과가 금방 낡음 | M | H | 도구 관리 워크트리는 그 도구에서 정리하도록 안내(카탈로그 CR-8), 실행 직전 재검증(NFR-S3) | US-FORAGE-006 |
| R-16 | **가장 큰 iOS 먹이가 자동 제안에서 빠짐**: DerivedData·CoreSimulator Caches·Xcode Previews는 Apple 공식 경로 근거가 없어 출처 규칙으로 C가 됨 → iOS 개발자의 첫 확보량(M-2)이 작아 보일 수 있음 | H | M | 개별 확인으로는 먹을 수 있음. Apple 공식 근거를 찾으면 B로 되돌림(카탈로그 3절 주의 1). 등급 규칙 자체는 바꾸지 않음 | M-2, 카탈로그 3절 |
