---
name: ases-critique
description: >
  ASES Sprint Execution — Opus audits GLM's code output. Invoke with /ases-critique
  [task-id] [sprint-id] after every /ases-dev or /ases-fix. Four lenses: spec, contract,
  test, security. Reads decisions.json first — distinguishes tradeoff from bug. Max 3
  iterations then auto-escalates. Detection only — never rewrites code.
allowed-tools: Read, Write
argument-hint: "[task-id e.g. T-001] [sprint-id e.g. S1]"
---

# ASES `/ases-critique [task-id] [sprint-id]`
**Agent:** Critic (Opus) · **Scope:** Task · **Max iterations:** 3

Parse: `TASK_ID` = first arg, `SPRINT_ID` = second arg.

## Input
Read task files (code written by GLM),
`sprints/$SPRINT_ID/T-$TASK_ID-plan.json`,
`sprints/$SPRINT_ID/lld.json`,
`.ases/decisions.json` ← **READ FIRST before flagging anything**,
`sprints/$SPRINT_ID/test_cases.json` (refs for this task)

## Four Lenses

### 1 — Spec
Does implementation match `plan.json`? Match lld function signatures + interfaces?

### 2 — Contract
Do exports match what other files expect (lld interfaces)?
Are all imports from `depends_on[]` used correctly?

### 3 — Test
Does implementation satisfy `test_case.expected_output`?
Are edge cases from `test_cases.json` handled?

### 4 — Security
Input validation, injection vectors, exposed secrets?

## Rules
- Read `decisions.json` FIRST — set `is_adr_tradeoff: true` for known decisions
- Detection only — no rewrites
- `fix_instruction` must be specific + actionable
- `iteration ≥ 3` AND `FIX_REQUIRED` → force verdict to `ESCALATE`

## Output
```
sprints/$SPRINT_ID/critique_$TASK_ID.json   ← schema: format/json/critique.schema.json
sprints/$SPRINT_ID/critique_$TASK_ID.md
```

CLEAN → update `tasks.json` status=complete → update `context.json` → next task
FIX_REQUIRED → `/ases-fix $TASK_ID $SPRINT_ID`
ESCALATE → present to PO → resume or `/ases-prd-update`
