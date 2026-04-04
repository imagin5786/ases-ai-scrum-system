---
name: ases-sprint-scaffold
description: >
  ASES Sprint Execution — Create new structural files for the current sprint. Invoke with
  /ases-sprint-scaffold [sprint-id] after /ases-analyze READY. TWO-STEP: Opus identifies
  new structure needed (Step A), then Claude Sonnet creates it (Step B). Sonnet only creates
  files explicitly listed by Opus. Whitelisted types only. Updates scaffold manifest.
allowed-tools: Read, Write, Bash(mkdir:*), Bash(find:*)
argument-hint: "[sprint-id e.g. S1]"
---

# ASES `/ases-sprint-scaffold [sprint-id]`
**Agent:** Architect (Opus) identifies → Developer (Claude Sonnet) creates

---

## Step A — Opus Identifies New Structure

Read `sprints/$ARGUMENTS/design/lld.json` and `contracts/scaffold.json`

Diff: which files in lld.json do NOT exist in scaffold.json?
List only:
- New module directories
- `__init__.py` stubs (empty body)
- Migration stub files
- New config entries

Write `sprints/$ARGUMENTS/design/scaffold_spec.json` with exact file list.
This file gates Step B — Sonnet does not start without it.

---

## Step B — Sonnet Creates Structure

**Pre-check:** Read `sprints/$ARGUMENTS/design/scaffold_spec.json`.
If it does not exist → STOP. Output: "Sprint scaffold_spec.json missing — Opus must complete Step A first."

**Permitted file types (whitelist):**
- `__init__.py` — empty body: `# {module name} module\n` only
- `*.py` migration stubs — class definition + `pass` only
- New directories via `mkdir -p`
- Config entries appended to existing config files only

**Forbidden:** Any file with business logic, imports of project modules, or function bodies.

Create only files listed in `scaffold_spec.files_to_create[]`.

---

## Step C — Update Scaffold Manifest

Add new files to `contracts/scaffold.json` with timestamps and empty hashes.
Update `docs/scaffold.md`.

## Next Step
→ `/ases-tasks $ARGUMENTS`
