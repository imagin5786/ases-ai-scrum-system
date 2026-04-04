---
name: ases-ui-design
description: >
  ASES Sprint Execution UI Track — Gemini designs the UI component specification for all
  UI-tagged tasks. Invoke with /ases-ui-design [sprint-id] after /ases-tasks when UI tasks
  exist. Produces ui_spec.json + ui_spec.md. Spec only — no code yet. Feeds /ases-ui-review.
allowed-tools: Read, Write
argument-hint: "[sprint-id e.g. S1]"
---

# ASES `/ases-ui-design [sprint-id]`
**Agent:** UI Designer (Gemini) · **Scope:** Sprint · **Condition:** has_ui_tasks

## Input
Read `sprints/$ARGUMENTS/execution/tasks.json` (ui_tasks[]),
`contracts/prd.json`, `contracts/hld.json`, `sprints/$ARGUMENTS/design/lld.json`

## Process
1. For each UI task: design component structure
2. Define props, state, interactions per component
3. Map API dependencies — endpoints each component calls
4. Specify `mock_data_ref` — mock file location for standalone scaffold
5. Define responsive behaviour + accessibility notes
6. Assign routes for page-level components

## Rules
- Spec only — no implementation code
- Every component links to `task_ref` + `feature_ref`
- API dependencies reference actual lld interface endpoints
- Mock data isolated in `/ui/mocks/`
- Components implementable in Next.js + Tailwind

## Output
```
sprints/$ARGUMENTS/execution/ui_spec.json   ← schema: format/json/ui_spec.schema.json
sprints/$ARGUMENTS/execution/ui_spec.md
```

## Next Step
→ `/ases-ui-review $ARGUMENTS`
