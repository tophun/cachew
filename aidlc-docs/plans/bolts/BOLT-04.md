---
artefact_type: bolt
id: BOLT-04
units: [DIGEST]
status: approved
estimate: 1일
---

# BOLT-04: 복구 불가 먹이 흐름

- Unit: DIGEST
- 예상 기간: 1일 (Bolt 단위)
- 대상 story: US-DIGEST-002

## 할 일
- [ ] "복구 불가" 표시와 별도 확인
- [ ] 명령 원문 미리보기 + 두 번째 확인
- [ ] 도구 명령 실행과 결과 기록
- [ ] 도구·데몬 미설치·미실행 시 제외
- [ ] Docker 행별 확인(CR-10), 순서 있는 명령의 단계 실행(CR-7)
- [ ] 셸 없이 실행 파일 절대 경로 + 인자 배열로 실행(NFR-S9)

## 완료 조건
- 대상 story의 acceptance criteria 중 이 Bolt 범위 항목이 테스트로 확인됨
- 사람 review 완료
