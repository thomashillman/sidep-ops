---
name: plan
description: Use when you have a spec or requirements for a multi-step task, before touching code
argument-hint: "[--direct|--consensus] <spec or requirements>"
---

# Plan: Implementation Planning with Consensus Validation

Write comprehensive implementation plans with bite-sized tasks. Optionally validate through Planner→Architect→Critic consensus loop.

**Save plans to:** `docs/plans/YYYY-MM-DD-<feature-name>.md`

## Modes

- `--direct` (default): Write plan directly from spec
- `--consensus`: Planner→Architect→Critic validation loop (RALPLAN-DR)

## Scope Check

If the spec covers multiple independent subsystems, suggest breaking into separate plans — one per subsystem.

## File Structure

Before defining tasks, map out which files will be created or modified. Each file should have one clear responsibility.

## Bite-Sized Task Granularity

**Each step is one action (2-5 minutes):**
- "Write the failing test" — step
- "Run it to make sure it fails" — step
- "Implement the minimal code to make the test pass" — step
- "Run the tests and make sure they pass" — step
- "Commit" — step

## Plan Document Header

```markdown
# [Feature Name] Implementation Plan

> **For agentic workers:** Use `execute` skill to implement this plan task-by-task.

**Goal:** [One sentence]
**Architecture:** [2-3 sentences]
**Tech Stack:** [Key technologies]

---
```

## Task Structure

````markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

- [ ] **Step 1: Write the failing test**
```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

- [ ] **Step 2: Run test to verify it fails**
Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL

- [ ] **Step 3: Write minimal implementation**
```python
def function(input):
    return expected
```

- [ ] **Step 4: Run test to verify it passes**
Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

- [ ] **Step 5: Commit**
```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```
````

## Remember
- Exact file paths always
- Complete code in plan (not "add validation")
- Exact commands with expected output
- DRY, YAGNI, TDD, frequent commits

## Consensus Mode (--consensus)

When `--consensus` is used, validate through RALPLAN-DR loop:

1. **Planner** creates implementation plan from spec
2. **Architect** reviews for architectural soundness, steelmans antithesis
3. **Critic** validates quality, testability, and gaps
4. Loop until consensus (max 5 iterations)

**RALPLAN-DR output includes:**
- Principles (3-5)
- Decision Drivers (top 3)
- Viable Options (≥2) with pros/cons
- ADR (Decision, Drivers, Alternatives, Why chosen, Consequences)

## Plan Review Loop

After writing the complete plan:

1. Dispatch plan-reviewer subagent (see `plan-reviewer-prompt.md`)
2. If ❌ Issues Found: fix, re-dispatch
3. If ✅ Approved: proceed to execution handoff
4. Max 3 iterations, then surface to human

## Execution Handoff

After saving the plan, offer execution choice:

**"Plan complete. Two execution options:**

**1. Subagent-Driven (recommended)** — fresh subagent per task, review between tasks
**2. Parallel Execution** — fire all independent tasks simultaneously

**Which approach?"**

Then invoke `execute` skill with the chosen mode.
