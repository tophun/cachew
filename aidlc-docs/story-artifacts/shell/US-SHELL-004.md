---
artefact_type: user_story
id: US-SHELL-004
unit: UNIT-SHELL
intent: requirements/intent.md
priority: high
status: draft
linked_domain_elements: []
linked_adrs: []
---

# US-SHELL-004: brew 한 줄로 설치

## Story
**As a** 동료 개발자,
**I want** `brew install --cask` 한 줄로 캐슈를 설치하기를,
**so that** "그거 깔아봐" 한 마디로 시작할 수 있다.

## Acceptance Criteria

- [ ] Given 개인 Homebrew tap을 추가하면, Then cask 설치·업그레이드가 된다 (NFR-D2).
- [ ] Given 설치한 앱을 처음 열면, Then Gatekeeper 경고 없이 실행된다(서명·공증, NFR-D1).
- [ ] Given 첫 실행이면, Then 캐슈 소개와 스캔 루트 설정(선택)을 보여 준다.

## Out of Scope
- Mac App Store 배포(C1)

## NFR 연관
- NFR-D1, D2

## Risk 연관
- R-11

## Open Questions
- 어느 Apple Developer 계정(개인/회사)으로 서명할지(R-11)
