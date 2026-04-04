# ASES v3.0 — AI Scrum Engineering System

<!-- ASES-MANAGED: Do not remove this section -->
## ASES Context Integration
Follow all rules in <ases-state> blocks injected by the hook.
These are dynamically injected based on current phase and context window.
They represent live project state and MUST be respected.
<!-- END ASES-MANAGED -->

---

## Model Allocation

| Layer | Model | Role |
|---|---|---|
| Reasoning | Claude Opus 4.6 | Planning, architecture, critique, test design |
| Execution | Claude Sonnet | Code generation, fixes, scaffolding, test impl |
| UI | Gemini 3.1 Pro | UI spec + Next.js/React scaffold |
| Decision | Product Owner (Human) | 6 mandatory approval gates per sprint |

---

## Pipeline

### Project Start (once)
```
/ases-interview → /ases-prd → /ases-hld → /ases-roadmap → /ases-init → /ases-scaffold
```

### Per Sprint

**Phase 1 — Sprint Design**
```
/ases-prd-update (optional) → /ases-lld → /ases-schema → /ases-test-spec → /ases-sprint-gate
```

**Phase 2 — Sprint Execution**
```
/ases-analyze → /ases-sprint-scaffold → /ases-tasks
/ases-ui-design → /ases-ui-review → /ases-ui-scaffold  [if UI tasks]
per task: /ases-validate → /ases-dev → /ases-critique → /ases-fix
/ases-sprint-close
```

**Phase 3 — Sprint Ship**
```
/ases-test-impl → /ases-integration-test → /ases-system-test
/ases-uat → /ases-devops → /ases-final-audit → /ases-release
```

---

## Context Architecture — Three Levels

| Level | File | Loaded By | Access |
|---|---|---|---|
| 3 (lean) | `.ases/context.json` | Hook — always | Every session |
| 2 (sprint) | `.ases/sprint_context.json` | Hook — active sprint only | Every sprint session |
| 1 (global) | `.ases/global_context.json` | Explicit call | `/ases-inject [IDs]` or PO commands |

**Global context entries are identifier-addressed:**
`SP-NNN` sprint digest · `DS-NNN` decision · `TD-NNN` tech debt
`FT-NNN` feature · `RI-NNN` risk · `CF-NNN` carry-forward

Manual injection: `/ases-inject DS-003 SP-001` — injects those specific entries only.

---

## Folder Structure

```
/.ases/              ← system state (hook reads, not project outputs)
  context.json + context.md       ← Level 3: lean always-loaded
  sprint_context.json + sprint_context.md  ← Level 2: sprint-scoped
  global_context.json + global_context.md  ← Level 1: explicit only
  decisions.json + decisions.md    ← ADR log, PO-only

/format/             ← ASES system: schemas + templates (excluded from auto-scan)
  json/
  markdown/

/docs/               ← human-readable PO documents (excluded from agent auto-scan)
  brief.md  prd.md  hld.md  roadmap.md  scaffold.md

/contracts/          ← machine-readable JSON contracts (excluded from auto-scan)
  brief.json  prd.json  hld.json  roadmap.json  scaffold.json

/sprints/SN/
  design/            ← Phase 1: lld, schema, test_cases, sprint_gate, scaffold_spec
  execution/         ← Phase 2: analysis, tasks, ui_*, snapshots/, tasks/T-NNN-*
  ship/              ← Phase 3: test_suite, integration, system, uat, devops, audit, release

/ui/                 ← Gemini scaffold (locked after creation)
/tests/unit/ /tests/integration/ /tests/system/
/src/                ← project code
```

---

## Hard Rules — Never Violate

1. JSON-only outputs between all pipeline stages
2. Every document has two files: `name.json` (agent) + `name.md` (human)
3. File-level isolation for every `/ases-dev` and `/ases-fix` — write only to `output_files[]` from task plan
4. Critique loop mandatory — max 3 iterations then escalate to PO
5. No agent role overlap — Critic detects only, Sonnet implements only
6. Schema validation at every gate transition
7. Sonnet never touches Gemini UI scaffold — only declared `integration_points`
8. Git commit only after UAT approval — `ases-hook.py` enforces this
9. Every architectural tradeoff → ADR entry in `decisions.json` with typed ID
10. Test cases derived from PRD `acceptance_criteria` — never invented

---

## Human Gates — Always Stop and Wait

1. After `/ases-roadmap` — before `/ases-init`
2. After `/ases-sprint-gate` PASS — before `/ases-analyze`
3. Any `ESCALATE` verdict from `/ases-critique`
4. `/ases-uat` — PO reviews running system
5. After `/ases-final-audit` SHIP — before `/ases-release`
6. Any `BLOCK` verdict — directed surgical re-entry

---

*ASES v3.0 · This is not prompting. This is an AI software factory.*
