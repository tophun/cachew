---
artefact_type: intent
status: registered
created_at: 2026-10-30
---

# Intent

## 한 문장

개발자가 즐겁게 디스크를 확보하도록, 캐릭터 **캐슈(Cachew)**가 개발 환경의 불필요한 캐시와 파일을 찾아 제안하고 사용자가 승인한 것만 "먹는" macOS 메뉴바 앱을 만든다.

## Pathway

**green-field.** 레포 `tophun/cachew`에는 코드가 없고, 첫 커밋(`8136d4b`)은 네이밍 문서뿐이다.

## 1차 사용자

- 회사 동료 개발자. 동료가 "그거 깔아봐"라고 권할 수 있는 수준이 목표다.
- 주 타겟 직군: 프론트/Node, iOS, Android, 백엔드(JVM·Go·Rust), Python/ML, Docker, IDE, AI 도구

## 킥오프에서 확정된 결정 (2026-10-30, 사용자 브리프 원문 기준)

| # | 결정 |
|---|---|
| D1 | 플랫폼: macOS 네이티브, Swift + SwiftUI, 메뉴바 앱 |
| D2 | 주 타겟: 전 개발 직군(위 목록) |
| D3 | "자동"의 의미: 찾기와 제안만 자동. 삭제는 언제나 사용자 승인 후에만 |
| D4 | 먹은 것은 휴지통(복구 가능)으로 보낸다 |
| D5 | 커밋 안 된 변경이 있는 worktree는 제안하지 않는다 |
| D6 | LLM은 선택 사항: 없음(기본) / 사용자의 에이전트 연결 / 로컬 LLM. 파일 내용은 절대 보내지 않는다 |
| D7 | 성장 요소: 먹은 용량만큼 경험치, 레벨 대신 코스튬이 열린다 |
| D8 | 이름: 캐슈 / Cachew (cache + chew, 다람쥐 캐릭터) — `00-naming.md` |

## Traceability

- 이름·캐릭터 근거: [`../00-naming.md`](../00-naming.md)
- 다음 단계 계획: [`../plans/level1_plan.md`](../plans/level1_plan.md), [`../plans/inception_plan.md`](../plans/inception_plan.md)
