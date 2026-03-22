---
name: execute
description: Use when executing implementation plans with independent tasks in the current session
argument-hint: "[--parallel|--persist] <plan file path>"
---

# Execute: Subagent-Driven Development with Parallel Engine

Execute plans by dispatching fresh subagent per task, with two-stage review after each: spec compliance first, then code quality.

**Core principle:** Fresh subagent per task + two-stage review (spec then quality) = high quality, fast iteration

## Modes

- **Default**: Sequential subagent-per-task with two-stage review
- `--parallel`: Fire all independent tasks simultaneously (ultrawork pattern)
- `--persist`: Ralph persistence loop with PRD tracking — keeps working until all acceptance criteria pass

## The Process

```
Read plan → Extract all tasks → Create task list
  ↓
Per Task:
  Dispatch implementer subagent
    ↓
  Implementer asks questions? → Answer, re-dispatch
    ↓
  Implementer implements, tests, commits, self-reviews
    ↓
  Dispatch spec reviewer → Spec compliant?
    No → Implementer fixes → Re-review
    Yes ↓
  Dispatch code quality reviewer → Approved?
    No → Implementer fixes → Re-review
    Yes ↓
  Mark task complete
    ↓
More tasks? → Next task
    ↓
Dispatch final reviewer for entire implementation
    ↓
Invoke `finish` skill
```

## Model Selection

- **Mechanical tasks** (1-2 files, clear spec): fast model (haiku/sonnet)
- **Integration tasks** (multi-file, pattern matching): standard model (sonnet)
- **Architecture/review tasks**: most capable model (opus)

## Handling Implementer Status

| Status | Action |
|--------|--------|
| **DONE** | Proceed to spec compliance review |
| **DONE_WITH_CONCERNS** | Read concerns, address if about correctness/scope, then review |
| **NEEDS_CONTEXT** | Provide missing context, re-dispatch |
| **BLOCKED** | Assess: context problem → provide context; reasoning → upgrade model; too large → split task; plan wrong → escalate to human |

## Two-Stage Review

**Stage 1 — Spec Compliance** (MUST pass before Stage 2):
- Does code match spec? Nothing more, nothing less?
- Missing requirements? Extra/unneeded work?
- Template: `skills/execute/spec-reviewer-prompt.md`

**Stage 2 — Code Quality** (only after Stage 1 passes):
- Code quality, architecture, testing, DRY, edge cases
- Template: `skills/execute/code-quality-reviewer-prompt.md`

## Parallel Mode (--parallel)

Fire all independent tasks simultaneously:
1. Identify task dependencies from plan
2. Group independent tasks
3. Dispatch all independent tasks as parallel subagents
4. Wait for all to complete
5. Run spec + quality review on each
6. Fix issues sequentially
7. Proceed to next dependency group

## Persist Mode (--persist)

Ralph-style persistence loop:
1. Load PRD/acceptance criteria
2. Execute tasks
3. Run verification against acceptance criteria
4. If any criteria fail → iterate with fixes
5. Continue until all criteria pass or max iterations reached

## Red Flags

**Never:**
- Skip reviews (spec compliance OR code quality)
- Proceed with unfixed issues
- Dispatch multiple implementation subagents in parallel on same files
- Make subagent read plan file (provide full text instead)
- Start code quality review before spec compliance is ✅
- Move to next task while either review has open issues

## Integration

- `plan` creates the plan this skill executes
- `tdd` — subagents follow RED-GREEN-REFACTOR
- `review` — code review template for reviewer subagents
- `finish` — complete development after all tasks

## Prompt Templates

- `./implementer-prompt.md` — Dispatch implementer subagent
- `./spec-reviewer-prompt.md` — Spec compliance reviewer
- `./code-quality-reviewer-prompt.md` — Code quality reviewer
