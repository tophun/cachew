---
artefact_type: bolt
id: BOLT-03
units: [FORAGE]
status: approved
estimate: 1~2일
---

# BOLT-03: 프로젝트 스캔 루트와 git 변경 판정

- Unit: FORAGE
- 예상 기간: 2일 (Bolt 단위)
- 대상 story: US-FORAGE-002, US-FORAGE-006

## 할 일
- [ ] 스캔 루트 설정 연동
- [ ] 프로젝트 산출물(node_modules, target, build, .venv 등) 탐지
- [ ] 커밋 안 된 변경이 있는 저장소·worktree 제외(D5, untracked 포함)
- [ ] 마커 파일 판정(CR-4)
- [ ] linked 워크트리 후보 판정(CR-9), 도구 관리 워크트리 안내(CR-8), 휴지통 후 `git worktree prune -n` 확인
- [ ] 스캔 루트가 `~/Documents`·`~/Desktop`일 때 권한 프롬프트 확인(R-08)

## 완료 조건
- 대상 story의 acceptance criteria 중 이 Bolt 범위 항목이 테스트로 확인됨
- 사람 review 완료
