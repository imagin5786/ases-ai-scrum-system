---
name: ases-ui-scaffold
description: >
  ASES Sprint Execution UI Track — Gemini builds the complete standalone Next.js/React
  scaffold after UI review approval. Invoke with /ases-ui-scaffold [sprint-id] after
  /ases-ui-review APPROVED. Produces complete runnable frontend with mock data. Zero backend
  calls, zero auth logic. Locked after creation — GLM may only touch integration_points.
allowed-tools: Read, Write, Bash(npm:*), Bash(mkdir:*)
argument-hint: "[sprint-id e.g. S1]"
---

# ASES `/ases-ui-scaffold [sprint-id]`
**Agent:** UI Designer (Gemini) · **Scope:** Sprint · **Runs after:** ui_review.APPROVED

## Input
Read `sprints/$ARGUMENTS/execution/ui_spec.json` (approved), `sprints/$ARGUMENTS/execution/ui_review.json`

## Process
1. Build complete Next.js/React project under `/ui/`
2. Implement all components from ui_spec exactly
3. Create mock data files in `/ui/mocks/`
4. Declare all `integration_points` in manifest — every place GLM will connect
5. Verify project runs standalone: `npm run dev`

## Rules
- ZERO backend API calls — mock data only
- ZERO auth/session logic — placeholder comments only
- ZERO env var reads — hardcoded mocks only
- Every future GLM touch point declared as `integration_point`

## 🔒 Post-Scaffold Lock
UI scaffold is **LOCKED** after this step.
GLM may ONLY touch declared `integration_points`.
Any structural change requires a new ui-design → ui-review → ui-scaffold cycle.

## Output
```
/ui/                                                    ← complete Next.js project
sprints/$ARGUMENTS/execution/ui_scaffold_manifest.json       ← schema: format/json/ui_scaffold_manifest.schema.json
sprints/$ARGUMENTS/execution/ui_scaffold_manifest.md
```

## Next Step
→ Begin execution loop with `/ases-validate [first-task-id] $ARGUMENTS`
