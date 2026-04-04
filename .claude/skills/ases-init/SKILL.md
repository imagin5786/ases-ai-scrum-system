---
name: ases-init
description: >
  ASES Phase 1 — Initialize the project container after roadmap approval.
  Invoke with /ases-init after PO approves /ases-roadmap. Creates the full folder structure,
  initializes context.json and decisions.json, and prepares the project for /ases-scaffold.
  Only runs once per project.
allowed-tools: Read, Write, Bash(mkdir:*)
---

# ASES `/ases-init`
**Agent:** System · **Scope:** Project · **Runs once**

## Input
Read `contracts/roadmap.json` (must be PO approved), `contracts/brief.json`, `contracts/hld.json`

## Creates

```
docs/
  context.json + context.md
  decisions.json + decisions.md   ← copies ADRs already written in /ases-hld
  brief.json + brief.md           ← already exists, confirmed
  prd.json + prd.md               ← already exists, confirmed
  hld.json + hld.md               ← already exists, confirmed
  roadmap.json + roadmap.md       ← already exists, approved
  sprints/
    S1/   S2/   ...               ← one folder per sprint in roadmap
format/json/
format/markdown/
format/markdown/
ui/                               ← Next.js scaffold target
tests/
  unit/
  integration/
  system/
.env.example
CHANGELOG.md
```

## Initializes
- `.ases/context.json` — full state, `prd_version: 1`, `current_sprint: S1`
- `.ases/decisions.json` — copies ADR entries from `/ases-hld`
- `CHANGELOG.md` — empty with header
- `.env.example` — placeholder

## Output
```
.ases/context.json   ← schema: format/json/context.schema.json
docs/context.md
```

## Next Step
→ `/ases-scaffold`
