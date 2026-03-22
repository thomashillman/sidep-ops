---
name: design
description: "You MUST use this before any creative work - creating features, building components, adding functionality, or modifying behavior. Explores user intent, requirements and design before implementation."
argument-hint: "[--quick|--standard|--deep] <idea or description>"
---

# Design: Brainstorming + Deep Interview

Turn ideas into fully formed designs through collaborative Socratic dialogue with mathematical ambiguity scoring.

<HARD-GATE>
Do NOT invoke any implementation skill, write any code, scaffold any project, or take any implementation action until you have presented a design and the user has approved it.
</HARD-GATE>

## Anti-Pattern: "This Is Too Simple To Need A Design"

Every project goes through this process. "Simple" projects are where unexamined assumptions cause the most wasted work. The design can be short, but you MUST present it and get approval.

## Modes

- `--quick`: Skip ambiguity scoring, lightweight Q&A (3-5 questions max)
- `--standard` (default): Full Socratic Q&A with ambiguity scoring
- `--deep`: Extended interview with challenge agents (contrarian, simplifier, ontologist)

## Checklist

1. **Explore project context** — check files, docs, recent commits
2. **Ask clarifying questions** — one at a time, understand purpose/constraints/success criteria
3. **Score ambiguity** — after each answer, score clarity across dimensions (Goal, Constraints, Success Criteria, Context)
4. **Propose 2-3 approaches** — with trade-offs and recommendation
5. **Present design** — in sections, get user approval after each section
6. **Write design doc** — save to `docs/specs/YYYY-MM-DD-<topic>-design.md`
7. **Spec review loop** — dispatch spec-reviewer subagent; fix and re-dispatch until approved (max 3 iterations)
8. **User reviews written spec** — ask user to review before proceeding
9. **Transition to plan** — invoke `plan` skill

## Ambiguity Scoring

After each user answer, score clarity:

| Dimension | Weight (Greenfield) | Weight (Brownfield) |
|-----------|-------------------|-------------------|
| Goal Clarity | 40% | 35% |
| Constraint Clarity | 30% | 25% |
| Success Criteria | 30% | 25% |
| Context Clarity | N/A | 15% |

**Formula:** `ambiguity = 1 - weighted_sum_of_scores`

**Threshold:** Proceed when ambiguity ≤ 20%.

Display after each round:
```
Round {n} | Targeting: {weakest_dimension} | Ambiguity: {score}%
```

## Question Strategy

- **One question at a time** — never batch
- **Target weakest dimension** — improve lowest-scoring area
- **Expose assumptions** — not just gather feature lists
- **Gather codebase facts via explore agent BEFORE asking user**
- **Multiple choice preferred** when possible

## Challenge Modes (--deep only)

| Mode | Round | Purpose |
|------|-------|---------|
| Contrarian | 4+ | "What if the opposite were true?" |
| Simplifier | 6+ | "What's the simplest version?" |
| Ontologist | 8+ (if ambiguity > 0.3) | "What IS this, really?" |

## Design Principles

- **YAGNI ruthlessly** — remove unnecessary features
- **Design for isolation** — clear boundaries, well-defined interfaces
- **Scale sections to complexity** — few sentences if straightforward, detailed if nuanced
- **Follow existing patterns** in existing codebases

## After Design Approval

1. Write spec to `docs/specs/YYYY-MM-DD-<topic>-design.md`
2. Dispatch spec-reviewer (see `spec-reviewer-prompt.md`)
3. If issues: fix, re-dispatch (max 3 iterations)
4. User review gate
5. Invoke `plan` skill — the ONLY next step

## Spec Review Dispatch

See `skills/design/spec-reviewer-prompt.md` for the reviewer subagent template.
