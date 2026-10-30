# aidlc-docs

이 폴더는 AI-Driven Development Lifecycle (AI-DLC) 산출물의 root다.

| 폴더 | 용도 |
|---|---|
| `plans/` | 단계별 plan md (checkbox 기반) |
| `plans/bolts/` | Bolt 단위 계획 |
| `requirements/` | intent, PRFAQ, measurement criteria |
| `story-artifacts/<unit>/` | user story와 acceptance criteria |
| `units/` | Unit 정의 |
| `design-artifacts/domain/` | Domain Model (DDD) |
| `design-artifacts/logical/` | Logical Design |
| `design-artifacts/adr/` | Architecture Decision Records |
| `design-artifacts/reverse/` | (brown-field) 역공학 산출물 |
| `risks/` | NFR, risk register matching |
| `tests/` | test plan, test result, validation report |
| `deployment/` | IaC, deployment unit manifest |
| `operations/runbook/` | 운영 시나리오별 절차 |
| `operations/incidents/` | incident 기록 |
| `operations/postmortem/` | 사후 분석 |
| `operations/anomaly_log/` | 이상 감지 로그 |
| `prompts.md` | 세션에서 사용된 표준 프롬프트 누적 |

본 구조는 `skills/ai-dlc/scripts/init_aidlc_workspace.sh`로 생성된다.
