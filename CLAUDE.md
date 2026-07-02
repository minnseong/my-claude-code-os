# Claude OS — Habit Tracker

## 기본 규칙

1. 클로드 OS 관련 모든 파일은 반드시 프로젝트 안(`my-claude-code-os/.claude/`)에 만들 것
2. 대화 과정에서 AI와의 협업을 배울 수 있도록 양질의 설명 제공할 것

---

## 현재 프로젝트 설정

> 에이전트·스킬이 `{변수명}` 형태로 참조. **새 앱으로 전환 시 이 섹션만 수정한다.**

| 변수 | 현재 값 |
|------|--------|
| `{앱_루트}` | `habit-tracker` |
| `{정책_문서}` | `habit-tracker/docs/planning/POLICY.md` |
| `{소스_루트}` | `habit-tracker/src/main/java` |
| `{리소스_루트}` | `habit-tracker/src/main/resources` |
| `{문서_루트}` | `habit-tracker/docs` |
| `{기술_스택}` | `Java 17 / Spring Boot 3.2 / H2 / Thymeleaf / Gradle` |
| `{테스트_스택}` | `JUnit 5 / AssertJ / Mockito` |
| `{PR_대상_레포}` | `next-step/my-claude-code-os` |
| `{PR_대상_브랜치}` | `minnseong` |

---

## 에이전트

| 에이전트 | 역할 |
|----------|------|
| `planner` | 앱 정책(POLICY.md) 관리, 게이미피케이션 기획, PRD 작성 |
| `designer` | 화면 명세서 작성, 디자인 시스템 관리 |
| `backend-dev` | TDD 기반 Spring Boot 구현 (테스트 먼저) |
| `frontend-dev` | Thymeleaf/CSS/JS 화면 구현 |
| `qa-engineer` | 코드 정적 분석, 테스트 케이스, 버그 리포트 — `/plan`과 `/qa` 공유 |
| `security-auditor` | OWASP Top 10 취약점 점검, 보안 리포트 |

---

## 스킬

| 스킬 | 설명 |
|------|------|
| `/interview [요청]` | 분해 → 충돌감지 → 5라운드 인터뷰 → 요구사항 확정 → 로그 저장 |
| `/interview-analyze` | 인터뷰 로그 분석 → 모호함 패턴 → `/interview` 개선안 제안 |
| `/plan [기획요소]` | planner → designer → backend+frontend → qa 자동 파이프라인 |
| `/qa [기능명]` | qa-engineer 단독 실행, 테스트 케이스 + 버그 리포트 |
| `/security [범위]` | security-auditor 점검 → 취약점 수정 지시 자동화 |
| `/refactor [범위]` | backend + frontend에게 리팩토링 병렬 요청 |
| `/github-commit` | diff 분석 후 커밋 메시지 작성 (일반/빠른 모드) |
| `/submit-pr title="..." mission="..."` | {PR_대상_레포} {PR_대상_브랜치} 브랜치로 PR 생성 |
| `스킬 통계` | 프로젝트별 스킬 사용 통계 |
| `작업 로그 저장해줘` | 세션 작업 내역 마크다운 저장 |

---

## 훅

| 이벤트 | 동작 |
|--------|------|
| `UserPromptSubmit` | 프롬프트 JSONL 기록 (`log-prompt.py`) |
| `PostToolUse` (Agent·Write·Edit·Bash) | 도구 사용 이력 JSONL 기록 (`log-work.py`) |
| `Stop` | 미커밋 변경사항 안전망 커밋 (`auto-commit.sh`) |

---

## 개발 원칙

- **TDD**: backend-dev는 테스트 먼저 (Red → Green → Refactor)
- **에이전트 자체 커밋**: 각 에이전트가 작업 완료 후 직접 커밋 (`auto-commit.sh`는 안전망)
- **문서 우선**: PRD → 화면 명세서 → API 계약서 → 구현
- **qa-engineer·security-auditor는 코드 수정 안 함**: 발견만, 수정은 개발자 담당
- **정책 문서**: `{정책_문서}` — 모든 기능 추가·변경 전 planner가 먼저 읽는다
