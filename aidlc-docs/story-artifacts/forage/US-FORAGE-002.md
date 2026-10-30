---
artefact_type: user_story
id: US-FORAGE-002
unit: UNIT-FORAGE
intent: requirements/intent.md
priority: high
status: draft
linked_domain_elements: []
linked_adrs: []
---

# US-FORAGE-002: 프로젝트 폴더 안의 먹이 찾기

## Story
**As a** 개발자,
**I want** 내가 지정한 프로젝트 폴더(예: `~/dev`) 아래의 `node_modules`, `target`, `build`, `.venv` 같은 프로젝트 산출물을 찾기를,
**so that** 오래 안 연 프로젝트의 무거운 산출물을 정리할 수 있다.

## Acceptance Criteria

- [ ] Given 스캔 루트가 설정되지 않았으면, Then 프로젝트 먹이는 찾지 않는다(기본값: 스캔 루트 없음).
- [ ] Given 스캔 루트가 있으면, When 스캔하면, Then 프로젝트별로 묶어 크기와 마지막 수정 시각을 보여 준다.
- [ ] Given 프로젝트(또는 worktree)가 git 저장소이고 커밋 안 된 변경이 있으면, Then 그 프로젝트의 먹이는 제안하지 않는다 (D5). "커밋 안 된 변경"은 tracked 수정과 **untracked 파일**을 모두 포함한다(git의 clean 정의와 같음, 카탈로그 V-6).
- [ ] Given `target/`·`build/`·`.build/`처럼 여러 도구가 쓰는 이름이면, When 옆에 마커 파일(`pom.xml`, `Cargo.toml`, `build.gradle(.kts)`, `Package.swift` 등)이 없으면, Then 제안하지 않는다 (카탈로그 CR-4).
- [ ] Given `build/`가 git이 추적하는 폴더이면, Then 제안하지 않는다.
- [ ] Given 프로젝트가 git 저장소가 아니면, Then 먹이를 제안하되 "git으로 관리되지 않음"을 표시한다.

## Out of Scope
- git worktree 자체를 먹는 것 → US-FORAGE-006

## NFR 연관
- NFR-S3, S8, F2

## Risk 연관
- R-01, R-08(스캔 루트가 `~/Documents` 등일 때), R-13

## Open Questions
- ~~untracked 파일도 "커밋 안 된 변경"으로 볼지~~ → 포함으로 정리(git의 clean 정의와 같고 더 보수적인 쪽)
- 스캔 루트 아래 탐색 깊이 기본값
