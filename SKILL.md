---
name: omo-writing-plans
description: Create an executable implementation plan for multi-step, multi-file, risky, or ambiguous coding work after requirements are clear and before editing code. Use for API, migration, security, architectural, or parallel-agent work; do not use for a small local fix with clear acceptance criteria.
---

# OMO Implementation Plans

Create one concise, evidence-based plan that OMO-Slim can execute. This skill
owns plan artifacts; OMO-Slim owns discovery, delegation, implementation,
review, and verification. Do not invoke or require Superpowers skills.

Use the requirements and traceability sections only for API, data, security,
migration, business-flow, or otherwise high-risk changes. Do not create them
for a small local fix with clear acceptance criteria. Do not create a separate
SpecFlow workflow, task engine, status lifecycle, or execution phase. Selectively
adopt SpecFlow concepts only for durable plan content: EARS requirements,
traceability, evidence, and approval for material decisions. Do not create
`.specflow` lifecycle directories or require separate requirements/design/tasks
phases.

## Preconditions

- Read relevant project instructions and source before writing a plan.
- Confirm goal, scope, non-goals, and observable acceptance criteria.
- Ask targeted questions when a decision materially changes behavior, API,
  data, security, or cost. Do not invent missing requirements.
- For work confined to one clear low-risk file, use a task card instead of a
  plan file.

## Plan Location

Save to `docs/plans/YYYY-MM-DD-<feature-name>.md`, unless project convention
or user instruction names another location. Create its parent directory only
when saving an approved plan.

## Plan Format

Start every plan with:

```markdown
# <Feature> Implementation Plan

## Goal
<observable outcome>

## Scope
<included behavior and explicit non-goals>

## Evidence
<relevant paths, symbols, existing patterns, and external constraints>

## Acceptance Criteria
- [ ] <testable behavior>

## Behavioral Requirements
<!-- Include only when the change needs auditable behavior. -->
- REQ-1: WHEN <condition>, system SHALL <observable behavior>.

## Traceability
<!-- Include only when Behavioral Requirements are present. -->
| Requirement | Task | Validation |
| --- | --- | --- |
| REQ-1 | Task N | `<focused command or manual check>` |

## Risks and Decisions
<only material risks, assumptions, or approval-needed decisions>
```

## Approval Gate

Ask for approval before execution only when the plan changes a public API,
data model or migration, authentication or authorization, security posture,
external integration, material cost, or production cutover. Do not add an
approval gate for every task.

## Durable Execution State

Plan file is single source of truth for scope, tasks, dependencies,
acceptance, and progress. OMO-Slim delegates runtime work, but agents must
not create a competing plan or reorder approved tasks without a recorded
decision. Each task records `State` (`pending` | `in_progress` | `blocked` |
`done`), `Result/evidence` (commit-independent summary plus validation
result), and `Blocker/deviation` when applicable. Update state after each
meaningful delegation boundary. After restart, resume from plan file, not
chat or in-memory state. This is durable plan progress, not a second task
engine or phase lifecycle.

Then add ordered tasks. Each task must be independently reviewable and state:

```markdown
### Task N: <outcome>

**Ownership:** <OMO direct | fixer | designer | oracle review>
**Files:** Create/modify exact paths; cite symbols or line ranges when known.
**Dependencies:** <prior task or none>
**State:** <pending | in_progress | blocked | done>
**Implementation:** Concrete behavior, interfaces, data changes, and edge cases.
**Validation:** Exact focused command or manual check with expected result.
**Result/evidence:** <commit-independent summary and validation result>
**Blocker/deviation:** <blocker, approved deviation, or none>
**Done when:** Observable completion condition.
```

Use exact paths and commands when evidence exists. Keep each task bounded to a
coherent change. Split only when work can be tested or reviewed independently.
Do not add placeholder steps, speculative APIs, unrelated refactors, or commit
commands unless user requests them.

## OMO Handoff

For plan-only requests, save or present plan, then stop. For implementation
requests without a risk gate, OMO-Slim may proceed. A material risk gate
requires approval. Scope-invalidating evidence pauses affected lane and
records replan or decision before resuming.

After approval when required, hand execution to OMO-Slim:

1. Execute tasks in dependency order.
2. Delegate bounded implementation only when it reduces total work. Preserve
   explicit file ownership; do not run writers on overlapping files.
3. Use `fixer` for mechanical/headless changes, `designer` for user-visible
   design, and `oracle` only for high-risk decisions or review.
4. Verify each completed task using its validation evidence, then validate all
   acceptance criteria before reporting completion.
5. Record only durable, source-linked decisions in AgentMemory when available.
   Do not store plan transcripts or temporary hypotheses.

## AgentMemory Boundary

AgentMemory is optional advisory context only. It never owns scope, task order,
acceptance, status, or completion evidence; plan and repository evidence remain
authoritative. It must never store secrets, credentials, raw transcripts, full
plan copies, or temporary hypotheses. Save only durable, reviewed,
source-linked decisions or lessons with project scope.

## Plan Review

Before saving, check:

- Every acceptance criterion maps to a task and validation.
- When Behavioral Requirements are used, every `REQ-*` maps to a task and
  validation in Traceability.
- Tasks use repository evidence, not assumed APIs or paths.
- Scope and non-goals prevent feature creep.
- Task ownership and dependencies do not conflict.
- Durable progress, blockers, deviations, and validation evidence are recorded.
- Exactly one plan owns task order and acceptance.
- Restart recovery works from plan file alone.
- Caveman-style brevity did not remove constraints, commands, or risks.
