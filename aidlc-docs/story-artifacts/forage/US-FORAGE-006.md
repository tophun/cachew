---
artefact_type: user_story
id: US-FORAGE-006
unit: UNIT-FORAGE
intent: requirements/intent.md
priority: medium
status: draft
linked_domain_elements: []
linked_adrs: []
---

# US-FORAGE-006: 다 쓴 git 워크트리 먹기

## Story
**As a** AI 에이전트로 워크트리를 많이 만드는 개발자,
**I want** 일이 끝나 다시 안 쓰는 linked 워크트리를 찾아 주기를,
**so that** 에이전트가 남긴 워크트리 사본(각자 `node_modules`·빌드 산출물 포함)이 쌓이지 않는다.

## Acceptance Criteria

- [ ] Given 스캔 루트 아래에서, When 최상위에 `.git` **파일**(`gitdir: …/.git/worktrees/<id>`)이 있는 디렉터리를 찾으면, Then 소속 저장소의 `git worktree list --porcelain`으로 확인된 것만 후보로 삼는다 (카탈로그 CR-3).
- [ ] Given 메인 워크트리(`.git`이 디렉터리), `bare`, `locked`이면, Then 어디에도 표시하지 않는다 (X 등급, 카탈로그 V-7).
- [ ] Given 아래 중 하나라도 해당하면, Then 제안하지 않는다 (D5 확장, 카탈로그 CR-9):
  - tracked 수정 또는 untracked 파일이 있음
  - upstream이 없거나 ahead > 0 (push 안 된 커밋)
  - 다른 브랜치·원격에 없는 detached HEAD
  - 서브모듈이 있음
  - IDE·에이전트 프로세스가 그 경로를 작업 디렉터리로 쓰고 있음 (카탈로그 CR-5)
- [ ] Given 후보이면, Then 안전 등급 C로 "직접 확인 필요"에만 하나씩 나오고(US-FORAGE-003), gitignore된 파일 목록(예: 에이전트가 복사한 `.env`)을 먹기 전에 보여 준다.
- [ ] Given 도구가 관리하는 워크트리(Codex `~/.codex/worktrees`, Conductor, Windsurf `~/.windsurf/worktrees`, Claude Desktop)이면, Then 캐슈가 직접 먹지 않고 그 도구에서 정리하는 방법을 안내한다 (카탈로그 CR-8, R-15).
- [ ] Given 사용자가 승인하면, Then 워크트리 디렉터리를 휴지통으로 보내고, `git worktree prune -n` 결과가 **그 항목 하나**일 때만 `git worktree prune`을 실행한다. 그 외에는 prune하지 않고 남은 메타데이터를 안내한다.
- [ ] Given 캐슈는, Then 브랜치를 지우지 않는다. `git worktree remove --force`를 쓰지 않는다.

## Out of Scope
- 브랜치 삭제, 원격 브랜치 정리
- 메인 워크트리(일반 클론) 통째로 먹기

## NFR 연관
- NFR-S3(재검증: git 상태·실행 중 프로세스), S5, S8, S9

## Risk 연관
- R-01, R-15

## Open Questions
- "오래됨" 기준(마지막 수정 후 N일, 브랜치가 기본 브랜치에 merge됨) 기본값
- ~~도구 관리 워크트리 처리~~ → 2026-10-30 승인: 안내만
