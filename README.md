<p align="center">
  <img src="https://img.shields.io/badge/Claude_Code-Plugin-7C3AED?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSJub25lIiBzdHJva2U9IndoaXRlIiBzdHJva2Utd2lkdGg9IjIiIHN0cm9rZS1saW5lY2FwPSJyb3VuZCIgc3Ryb2tlLWxpbmVqb2luPSJyb3VuZCI+PHBhdGggZD0iTTEyIDJ2NGw0LTIiLz48cGF0aCBkPSJNMTIgMnY0bC00LTIiLz48cGF0aCBkPSJNMTIgMjJWMTgiLz48cGF0aCBkPSJNMjAgMTJoLTQiLz48cGF0aCBkPSJNNCA4bDQgMiIvPjxjaXJjbGUgY3g9IjEyIiBjeT0iMTIiIHI9IjQiLz48L3N2Zz4=&logoColor=white" alt="Claude Code Plugin" />
  <img src="https://img.shields.io/badge/version-1.0.0-blue?style=for-the-badge" alt="Version" />
  <img src="https://img.shields.io/badge/license-MIT-green?style=for-the-badge" alt="MIT License" />
</p>

<h1 align="center">sidep-ops</h1>

<p align="center">
  <strong>Superpowers' development discipline + Oh-My-ClaudeCode's orchestration</strong><br/>
  A unified Claude Code plugin that removes overlapping functionality from three plugins and keeps only the strengths of each
</p>

<p align="center">
  <a href="#-why-was-this-made">Why was this made</a> ·
  <a href="#-quick-start">Quick start</a> ·
  <a href="#-26-skills">Skills</a> ·
  <a href="#-15-agents">Agents</a> ·
  <a href="#-workflows">Workflows</a> ·
  <a href="#-credits">Credits</a>
</p>

---

## 🤔 Why was this made

**I made it just for my own use.**

After installing various Claude Code plugins, I ended up running all three at once: [Superpowers](https://github.com/obra/superpowers), [Oh-My-ClaudeCode](https://github.com/Yeachan-Heo/oh-my-claudecode), and [Ralph Loop](https://github.com/claude-plugins-official/ralph-loop). When I ran `/context`, I found dozens of similar skills loaded under different names.

They were eating context every turn, and I kept running into the same confusion, like "Should I use `brainstorming` or `deep-interview` for this task?" So I just merged them into one.

**Areas of overlap:**

| Domain | How they overlap |
|--------|------------------|
| Planning | `brainstorming` ↔ `deep-interview` ↔ `omc-plan` ↔ `ralplan` |
| Execution | `subagent-driven-dev` ↔ `ultrawork` ↔ `autopilot` ↔ `ralph` |
| Code review | `requesting-code-review` ↔ `code-reviewer` agent ↔ `critic` agent |
| Verification | `verification-before-completion` ↔ `verifier` agent ↔ `ultraqa` |
| Parallel work | `dispatching-parallel-agents` ↔ `ultrawork` ↔ `team` |

**sidep-ops** solves this problem:

| Item | Before (3 plugins) | sidep-ops | Reduction |
|------|--------------------|-----------|-----------|
| Skills | ~50 | **26** | -48% |
| Agents | 20 | **15** | -25% |
| Number of plugins | 3 | **1** | -67% |
| Context consumption | ~3k tokens/turn | ~1.6k tokens/turn | **~45%** |

---

## 🚀 Quick start

### Install

```bash
# 1. Register marketplace
/plugin marketplace add https://github.com/atmigtnca/sidep-ops

# 2. Install
/plugin install sidep-ops

# 3. Reload
/reload-plugins

Replace existing plugins (optional)

/plugin uninstall superpowers
/plugin uninstall oh-my-claudecode
/plugin uninstall ralph-loop

Verify installation

/context    # If you see sidep-ops skills in the Skills section, it worked


⸻

💡 Core principles

OMC provides the “how”, Superpowers provides the “why”.

OMC’s machinery (infrastructure)	Superpowers’ discipline (principles)
11 hooks	Mandatory TDD rule
15 MCP tools	Systematic debugging
State management	Verification gates
Parallel engine	Two-stage code review


⸻

📁 Project structure

sidep-ops/
│
├── .claude-plugin/
│   └── plugin.json            # Plugin manifest
├── .mcp.json                  # MCP server config
│
├── agents/                    # 🤖 15 agent definitions
│   ├── executor.md            #   Implementation (Sonnet, TDD-enhanced)
│   ├── architect.md           #   Architecture (Opus, read-only)
│   ├── critic.md              #   Critique (Opus, read-only)
│   ├── code-reviewer.md       #   Code review (Opus, 2-stage)
│   ├── planner.md             #   Planning (Opus)
│   ├── debugger.md            #   Debugging (Sonnet, 4-stage)
│   ├── test-engineer.md       #   Testing (Sonnet, TDD)
│   ├── verifier.md            #   Verification (Sonnet, evidence gate)
│   ├── explore.md             #   Exploration (Haiku, read-only)
│   ├── analyst.md             #   Requirements (Opus, read-only)
│   ├── qa-tester.md           #   QA (Sonnet)
│   ├── security-reviewer.md   #   Security (Opus, read-only)
│   ├── scientist.md           #   Research (Sonnet, read-only)
│   ├── document-specialist.md #   External docs (Sonnet, read-only)
│   └── tracer.md              #   Causal tracing (Sonnet)
│
├── skills/                    # 🛠️ 26 skills
│   ├── design/                #   Design (SP+OMC merged)
│   ├── plan/                  #   Planning (SP+OMC merged)
│   ├── execute/               #   Execution (SP+OMC merged)
│   ├── review/                #   Code review (SP merged)
│   ├── meta/                  #   Skill meta (SP rewritten)
│   ├── tdd/                   #   TDD (SP)
│   ├── debug/                 #   Debugging (SP)
│   ├── verify/                #   Verification (SP)
│   ├── worktree/              #   Git worktree (SP)
│   ├── finish/                #   Branch completion (SP)
│   ├── autopilot/             #   Autonomous execution (OMC)
│   ├── team/                  #   Team collaboration (OMC)
│   ├── qa/                    #   QA cycle (OMC)
│   ├── trace/                 #   Hypothesis tracing (OMC)
│   ├── deep-dive/             #   Deep analysis (OMC)
│   ├── deslop/                #   Code cleanup (OMC)
│   ├── research/              #   External search (OMC)
│   ├── science/               #   Scientific analysis (OMC)
│   ├── ask/                   #   Multi-model (OMC)
│   ├── learn/                 #   Skill extraction (OMC)
│   ├── cancel/                #   Cancel mode (OMC)
│   ├── skill/                 #   Skill management (OMC)
│   ├── hud/                   #   HUD config (OMC)
│   ├── ccg/                   #   3-model synthesis (OMC)
│   ├── note/                  #   Notes (OMC)
│   └── init/                  #   Initialisation (OMC)
│
├── hooks/
│   └── hooks.json             # ⚡ 11 lifecycle hooks
│
├── scripts/                   # Hook execution scripts
├── bridge/                    # MCP server runtime
└── dist/                      # Compiled TypeScript


⸻

🛠️ Skills (26)

Design → Plan → Execute pipeline

The five merged skills are the core. They combine best practices from the existing plugins into one.

graph LR
    A["design<br/><small>Q&A + ambiguity ≤20%</small>"] --> B["plan<br/><small>2-5 min tasks + consensus</small>"]
    B --> C["execute<br/><small>subagents + 2-stage review</small>"]
    C --> D["verify<br/><small>evidence required</small>"]
    D --> E["finish<br/><small>merge/PR/keep/discard</small>"]

    style A fill:#7C3AED,color:#fff
    style B fill:#6D28D9,color:#fff
    style C fill:#5B21B6,color:#fff
    style D fill:#4C1D95,color:#fff
    style E fill:#3B0764,color:#fff

Skill	Description	Flags
design	Uses Socratic Q&A to turn ideas into a spec doc. Keeps asking until ambiguity is ≤20%	--quick --deep
plan	Breaks the spec into 2 to 5 minute tasks. Includes TDD stages	--consensus --direct
execute	Per-task subagent + spec compliance review → code quality review	--parallel --persist
autopilot	Fully automated design → plan → execute → qa → verify → finish	—
team	N agents collaborate in parallel on shared tasks	—

Quality gates

The core discipline from Superpowers is preserved as-is.

graph LR
    subgraph Quality Gates
        T["tdd<br/>RED→GREEN→REFACTOR"]
        V["verify<br/>reject without evidence"]
        R["review<br/>no blind agreement"]
        Q["qa<br/>repeat up to 5 times"]
        D["debug<br/>4-stage root cause"]
    end

    style T fill:#DC2626,color:#fff
    style V fill:#D97706,color:#fff
    style R fill:#059669,color:#fff
    style Q fill:#2563EB,color:#fff
    style D fill:#7C3AED,color:#fff

Skill	Core principle	Details
tdd	“No production code without tests”	RED → GREEN → REFACTOR cycle required
verify	“No should/probably”	Must confirm evidence, test output, build results, before claiming completion
review	“No blind agreement”	Act only after technical validation. Blocks empty praise like “Great point!”
qa	Test → diagnose → fix cycle	Repeats up to 5 times, then stops automatically
debug	“If it fails 3 times, suspect the architecture”	Root cause investigation → pattern analysis → hypothesis validation → implementation, 4 stages

Investigation / analysis

Skill	Description
trace	Investigates competing hypotheses in parallel to find root cause
deep-dive	Deep analysis through a two-stage pipeline: trace → interview
research	External web search and official documentation lookup
science	Data-driven analysis using parallel scientist agents

Git workflows

Skill	Description
worktree	Creates isolated work environments with git worktree. Detects setup automatically
finish	When a branch is done, offers four options: merge / PR / keep / discard

Utilities

Skill	Description
meta	Auto-checks whether a skill should be applied before any action (“run it if there is even a 1% chance it applies”)
deslop	Cleans unnecessary AI-generated code safely, without regressions
ask	Queries Claude / Codex / Gemini simultaneously
ccg	Synthesises results from three models into the best answer
learn	Extracts reusable skills from the current session
cancel	Immediately cancels all active automation modes
skill hud note init	Skill management, HUD settings, notes, project initialisation


⸻

🤖 Agents (15)

Agents are AI workers optimised for specific roles. They are assigned by model tier to balance cost and performance.

graph TB
    subgraph Opus ["🟣 Opus (highest performance, analysis/judgement)"]
        architect
        critic
        code-reviewer
        planner
        analyst
        security-reviewer
    end
    subgraph Sonnet ["🔵 Sonnet (balanced, implementation/testing)"]
        executor
        debugger
        test-engineer
        verifier
        qa-tester
        scientist
        document-specialist
        tracer
    end
    subgraph Haiku ["⚡ Haiku (lightweight, exploration)"]
        explore
    end

Agent	Model	Role	SP enhancement
executor	Sonnet	Code implementation	TDD cycle + atomic commit injection
architect	Opus	Architecture analysis (read-only)	4-stage systematic debugging protocol
critic	Opus	Plan/code critique (read-only)	“No blind agreement” principle
code-reviewer	Opus	Code review (read-only)	2-stage review, spec compliance → code quality
planner	Opus	Execution planning	2 to 5 minute task format + TDD stage format
debugger	Sonnet	Bug root cause tracing	3-failure circuit breaker + escalation
test-engineer	Sonnet	Test strategy/writing	“No production code without tests”
verifier	Sonnet	Completion verification	“No completion claim without evidence”
explore	Haiku	Codebase exploration (read-only)	—
analyst	Opus	Requirements analysis (read-only)	—
qa-tester	Sonnet	Interactive QA testing	—
security-reviewer	Opus	Security vulnerabilities (read-only)	—
scientist	Sonnet	Data analysis/research (read-only)	—
document-specialist	Sonnet	External documentation lookup (read-only)	—
tracer	Sonnet	Causal tracing	—

Read-only: Agents with Write/Edit tools blocked. They analyse only and do not modify code directly.

⸻

⚡ Hooks (11 lifecycle hooks)

They intervene automatically at every stage of a Claude Code session:

graph TD
    A[SessionStart<br/>initialisation + memory load] --> B[UserPromptSubmit<br/>keyword detection + skill injection]
    B --> C[PreToolUse<br/>enforce skill discipline]
    C --> D[PostToolUse<br/>verification gate + memory update]
    D -->|next input| B

    C -.-> E[PermissionRequest]
    D -.-> F[PostToolUseFailure]
    C -.-> G[SubagentStart / SubagentStop]
    D -.-> H[PreCompact]

    I[Stop<br/>context guard + Ralph persistence + code cleanup] --> J[SessionEnd<br/>session cleanup]

    style A fill:#059669,color:#fff
    style I fill:#DC2626,color:#fff
    style J fill:#6B7280,color:#fff


⸻

🔧 MCP tools (15)

All of OMC’s MCP tools are preserved. This is a powerful toolset for code intelligence.

LSP (Language Server Protocol), 12

Tool	Description
lsp_hover	Type info / docs at cursor position
lsp_goto_definition	Jump to symbol definition
lsp_find_references	Find all usages of a symbol
lsp_document_symbols	Symbol list within a file (outline)
lsp_workspace_symbols	Search symbols across the whole workspace
lsp_diagnostics	Errors/warnings for a single file
lsp_diagnostics_directory	Type-check the whole project
lsp_prepare_rename	Check whether renaming is possible
lsp_rename	Multi-file rename preview
lsp_code_actions	Available refactors/fixes
lsp_code_action_resolve	Detailed code action info
lsp_servers	List available language servers

AST (Abstract Syntax Tree), 2

Tool	Description
ast_grep_search	Search structural code patterns ($NAME, $$$ARGS metavariables)
ast_grep_replace	AST-based code transformation (dry-run by default)

Python REPL, 1

Tool	Description
python_repl	Execute Python for data analysis


⸻

🔄 Workflows

Feature development

graph TD
    User["User: add chat feature"] --> Design

    Design["design"] -->|"ambiguity > 20%"| QA["repeat Q&A"]
    QA --> Design
    Design -->|"ambiguity ≤ 20%<br/>generate spec doc"| Plan

    Plan["plan"] -->|"--consensus"| Consensus["Planner → Architect → Critic"]
    Consensus --> Plan
    Plan --> Execute

    Execute["execute"] -->|"task N"| Sub["subagent implementation<br/>→ spec review<br/>→ quality review"]
    Sub --> Execute
    Execute --> Verify

    Verify["verify<br/>confirm evidence"] --> Finish["finish<br/>merge / PR / keep / discard"]

    style Design fill:#7C3AED,color:#fff
    style Plan fill:#6D28D9,color:#fff
    style Execute fill:#5B21B6,color:#fff
    style Verify fill:#D97706,color:#fff
    style Finish fill:#059669,color:#fff

Bug fixes

graph LR
    A["debug<br/>4-stage root cause"] --> B["tdd<br/>write failing test first"] --> C["execute<br/>implement fix"] --> D["verify<br/>confirm evidence"]

    style A fill:#DC2626,color:#fff
    style B fill:#D97706,color:#fff
    style C fill:#5B21B6,color:#fff
    style D fill:#059669,color:#fff

Autonomous execution

graph LR
    User["Handle this yourself"] --> AP

    subgraph AP ["autopilot (full automation, with SP verification gates)"]
        direction LR
        a[design] --> b[plan] --> c[execute] --> d[qa] --> e[verify] --> f[finish]
    end

    style AP fill:#1E1B4B,color:#fff


⸻

🔀 Merge sources

This openly shows what came from which plugin:

Domain	From Superpowers	From OMC
Planning	Socratic Q&A, 2 to 5 minute task format	Consensus validation (Planner → Architect → Critic), ambiguity scoring
Execution	2-stage review (spec compliance → quality), implementer state management	Parallel engine (ultrawork), Ralph persistence loop
Code review	6-point review methodology, “no blind agreement”	Agent roster (code-reviewer, critic)
Debugging	4-stage methodology, 3-failure circuit breaker	trace parallel hypotheses, deep-dive pipeline
Verification	Evidence gate (“no should/probably”)	QA cycling (up to 5 times)
TDD	Mandatory RED → GREEN → REFACTOR rule	test-engineer agent
Infrastructure	—	11 hooks, 15 MCP tools, state management, project memory


⸻

🙏 Credits

This plugin is based on the code and ideas from the following projects:
	•	Superpowers￼ by Jesse Vincent, TDD, systematic debugging, verification gates, code review discipline
	•	Oh-My-ClaudeCode￼ by Yeachan Heo, multi-agent orchestration, MCP tools, state management
	•	Ralph Loop￼, self-referential iterative loop (Ralph Wiggum technique￼)

⸻

📄 License

MIT￼, respects the licences of the original projects.

