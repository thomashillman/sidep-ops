# sidep-ops

**Superpowers의 개발 규율 + Oh-My-ClaudeCode의 오케스트레이션을 하나로 합친 Claude Code 통합 플러그인.**

3개 플러그인(Superpowers, OMC, Ralph Loop)의 기능 겹침을 제거하고, 각각의 장점만 취합했습니다.

| 항목 | 기존 (3개 플러그인) | sidep-ops |
|------|-------------------|-----------|
| 스킬 | ~50개 (겹침 다수) | **26개** |
| 에이전트 | 20개 | **15개** |
| 플러그인 수 | 3개 | **1개** |

---

## 설치

```bash
# 마켓플레이스 등록
/plugin marketplace add https://github.com/<your-username>/sidep-ops

# 설치
/plugin install sidep-ops

# 기존 플러그인 제거 (선택)
/plugin uninstall superpowers
/plugin uninstall oh-my-claudecode
/plugin uninstall ralph-loop
```

---

## 구조

```
sidep-ops/
├── .claude-plugin/plugin.json   # 플러그인 매니페스트
├── .mcp.json                    # MCP 서버 설정
├── agents/                      # 15개 에이전트 정의
├── skills/                      # 26개 스킬
├── hooks/hooks.json             # 11개 라이프사이클 훅
├── bridge/                      # MCP 서버 런타임 (LSP, AST, REPL)
├── scripts/                     # 훅 스크립트
└── dist/                        # 컴파일된 TypeScript
```

---

## 핵심 원칙

> **OMC의 기계** (hooks, agents, MCP, state management) **+ Superpowers의 규율** (TDD, 체계적 디버깅, 검증 게이트)

---

## 스킬 (26개)

### 설계 → 계획 → 실행

| 스킬 | 역할 | 출처 |
|------|------|------|
| `design` | 소크라테스식 Q&A + 모호성 점수 → 스펙 문서 | SP + OMC 병합 |
| `plan` | 2-5분 단위 태스크 + 합의 검증 | SP + OMC 병합 |
| `execute` | 태스크별 서브에이전트 + 2단계 리뷰 + 병렬 엔진 | SP + OMC 병합 |
| `autopilot` | 설계→계획→실행→QA→검증 자율 파이프라인 | OMC 기반 |
| `team` | N개 에이전트 협업 | OMC |

### 품질 게이트

| 스킬 | 역할 | 핵심 원칙 |
|------|------|----------|
| `tdd` | RED-GREEN-REFACTOR | "테스트 없이 프로덕션 코드 없다" |
| `verify` | 완료 전 증거 확인 | "should/probably 금지" |
| `review` | 코드리뷰 요청 + 수신 | "맹목적 동의 금지" |
| `qa` | 테스트→진단→수정 사이클 | 최대 5회 반복 |
| `debug` | 4단계 체계적 디버깅 | "3번 실패 시 아키텍처 의심" |

### 조사 / 분석

| 스킬 | 역할 |
|------|------|
| `trace` | 경쟁 가설 병렬 조사 |
| `deep-dive` | trace → interview 2단계 파이프라인 |
| `research` | 외부 웹 검색 / 문서 조회 |
| `science` | 병렬 과학자 에이전트 |

### Git 워크플로우

| 스킬 | 역할 |
|------|------|
| `worktree` | 격리된 작업 환경 생성 |
| `finish` | 머지 / PR / 유지 / 폐기 |

### 유틸리티

| 스킬 | 역할 |
|------|------|
| `meta` | 모든 행동 전 스킬 적용 확인 |
| `deslop` | AI 생성 코드 정리 |
| `ask` | Claude/Codex/Gemini 멀티모델 질의 |
| `ccg` | 3모델 결과 종합 |
| `learn` | 세션에서 스킬 추출 |
| `cancel` | 실행 중인 모드 취소 |
| `skill` `hud` `note` `init` | 관리 / 설정 |

---

## 에이전트 (15개)

| 에이전트 | 모델 | 역할 | SP 강화 |
|----------|------|------|---------|
| `executor` | Sonnet | 구현 | TDD + 아토믹 커밋 |
| `architect` | Opus | 아키텍처 (읽기전용) | 4단계 디버깅 |
| `critic` | Opus | 비평 (읽기전용) | 맹목적 동의 금지 |
| `code-reviewer` | Opus | 코드리뷰 (읽기전용) | 2단계 리뷰 |
| `planner` | Opus | 계획 | 2-5분 태스크 포맷 |
| `debugger` | Sonnet | 디버깅 | 근본 원인 우선 |
| `test-engineer` | Sonnet | 테스트 | TDD 필수법칙 |
| `verifier` | Sonnet | 검증 | 증거 게이트 |
| `explore` | Haiku | 탐색 | — |
| `analyst` | Opus | 요구사항 분석 | — |
| `qa-tester` | Sonnet | QA | — |
| `security-reviewer` | Opus | 보안 | — |
| `scientist` | Sonnet | 연구 | — |
| `document-specialist` | Sonnet | 외부 문서 | — |
| `tracer` | Sonnet | 인과 추적 | — |

---

## MCP 도구 (15개)

OMC에서 전량 보존:

- **LSP** (12개): hover, goto-definition, find-references, document-symbols, workspace-symbols, diagnostics, diagnostics-directory, prepare-rename, rename, code-actions, code-action-resolve, servers
- **AST** (2개): `ast_grep_search`, `ast_grep_replace`
- **Python REPL** (1개): `python_repl`

---

## 훅 (11개 라이프사이클)

```
UserPromptSubmit  → 키워드 감지 + 스킬 자동 주입
SessionStart      → 세션 초기화 + 프로젝트 메모리 로드
PreToolUse        → 스킬 규율 강제
PostToolUse       → 검증 게이트 + 메모리 업데이트
SubagentStart     → 서브에이전트 추적
SubagentStop      → 산출물 검증
PreCompact        → 컨텍스트 압축 전 상태 저장
Stop              → 컨텍스트 가드 + Ralph 지속 모드 + 코드 정리
SessionEnd        → 세션 정리
PermissionRequest → Bash 권한 처리
PostToolUseFailure → 도구 실패 처리
```

---

## 워크플로우

```
모호한 아이디어 → design (Q&A + 모호성 ≤20%) → plan (2-5분 태스크)
                                                    ↓
명확한 작업 ──────────────────────────────────→ execute (서브에이전트 + 2단계 리뷰)
                                                    ↓
                                                verify (증거 확인)
                                                    ↓
                                                finish (머지/PR)

버그        → debug (4단계) → tdd → execute → verify
자율 실행   → autopilot (전체 자동)
대규모 병렬 → team (N개 에이전트)
```

---

## 병합 출처

| 도메인 | Superpowers에서 | OMC에서 |
|--------|----------------|---------|
| 계획 | 소크라테스식 Q&A, 2-5분 태스크 | 합의 검증 (Planner→Architect→Critic) |
| 실행 | 2단계 리뷰 (스펙→품질) | 병렬 엔진, Ralph 지속 루프 |
| 코드리뷰 | 6점 리뷰, "맹목적 동의 금지" | 에이전트 로스터 |
| 디버깅 | 4단계 방법론, 3회 실패 차단 | trace 병렬 가설 |
| 검증 | 증거 게이트 | QA 사이클링 |
| TDD | RED-GREEN-REFACTOR 필수 | test-engineer 에이전트 |
| 인프라 | — | 훅 11개, MCP 15개, 상태관리 |

---

## 크레딧

이 플러그인은 다음 프로젝트의 코드와 아이디어를 기반으로 합니다:

- **[Superpowers](https://github.com/obra/superpowers)** by Jesse Vincent — TDD, 체계적 디버깅, 검증 게이트
- **[Oh-My-ClaudeCode](https://github.com/Yeachan-Heo/oh-my-claudecode)** by Yeachan Heo — 멀티에이전트 오케스트레이션, MCP 도구
- **[Ralph Loop](https://github.com/claude-plugins-official/ralph-loop)** — 자기참조 반복 루프 ([Ralph Wiggum 기법](https://ghuntley.com/ralph/))

## 라이선스

[MIT](LICENSE)
