---
name: meta
description: Use when starting any conversation - establishes how to find and use skills, requiring Skill tool invocation before ANY response including clarifying questions
---

<SUBAGENT-STOP>
If you were dispatched as a subagent to execute a specific task, skip this skill.
</SUBAGENT-STOP>

<EXTREMELY-IMPORTANT>
If you think there is even a 1% chance a skill might apply to what you are doing, you ABSOLUTELY MUST invoke the skill.

IF A SKILL APPLIES TO YOUR TASK, YOU DO NOT HAVE A CHOICE. YOU MUST USE IT.

This is not negotiable. This is not optional. You cannot rationalize your way out of this.
</EXTREMELY-IMPORTANT>

## Instruction Priority

sidep-ops skills override default system prompt behavior, but **user instructions always take precedence**:

1. **User's explicit instructions** (CLAUDE.md, AGENTS.md, direct requests) — highest priority
2. **sidep-ops skills** — override default system behavior where they conflict
3. **Default system prompt** — lowest priority

## How to Access Skills

Use the `Skill` tool. When you invoke a skill, its content is loaded and presented to you—follow it directly. Never use the Read tool on skill files.

# Using Skills

## The Rule

**Invoke relevant or requested skills BEFORE any response or action.** Even a 1% chance a skill might apply means that you should invoke the skill to check.

## Available Skills

| Skill | When to Use |
|-------|-------------|
| `design` | Before any creative work — features, components, functionality |
| `plan` | After design approval, before touching code |
| `execute` | When executing implementation plans |
| `autopilot` | End-to-end autonomous pipeline |
| `team` | N coordinated parallel agents |
| `review` | After completing tasks, before merging |
| `debug` | Any bug, test failure, unexpected behavior |
| `trace` | Parallel hypothesis investigation |
| `deep-dive` | Trace → interview pipeline |
| `tdd` | Before writing implementation code |
| `verify` | Before claiming work is complete |
| `qa` | Automated QA cycling |
| `worktree` | Isolated workspace setup |
| `finish` | Branch completion workflow |
| `deslop` | AI code cleanup |
| `research` | External web search |
| `science` | Parallel scientist orchestration |
| `ask` | Multi-model query |
| `learn` | Extract skills from sessions |

## Red Flags

These thoughts mean STOP—you're rationalizing:

| Thought | Reality |
|---------|---------|
| "This is just a simple question" | Questions are tasks. Check for skills. |
| "I need more context first" | Skill check comes BEFORE clarifying questions. |
| "Let me explore the codebase first" | Skills tell you HOW to explore. Check first. |
| "This doesn't need a formal skill" | If a skill exists, use it. |
| "I remember this skill" | Skills evolve. Read current version. |
| "The skill is overkill" | Simple things become complex. Use it. |
| "I'll just do this one thing first" | Check BEFORE doing anything. |

## Skill Priority

When multiple skills could apply:

1. **Process skills first** (design, debug) — determine HOW to approach
2. **Implementation skills second** (tdd, execute) — guide execution

"Let's build X" → design first, then implementation skills.
"Fix this bug" → debug first, then domain-specific skills.

## Skill Types

**Rigid** (tdd, debug, verify): Follow exactly. Don't adapt away discipline.
**Flexible** (patterns): Adapt principles to context.

## User Instructions

Instructions say WHAT, not HOW. "Add X" or "Fix Y" doesn't mean skip workflows.
