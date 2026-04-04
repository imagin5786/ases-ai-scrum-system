---
name: ases-scaffold
description: >
  ASES Phase 1 — Build the runnable project skeleton. Invoke with /ases-scaffold after
  /ases-init. TWO-STEP: Opus writes scaffold_spec.json first (Step A), then Claude Sonnet
  reads it and executes exactly (Step B). Sonnet does NOT start until scaffold_spec.json
  exists. Produces only whitelisted file types. Runs verify_cmd and reports output.
allowed-tools: Read, Write, Bash(mkdir:*), Bash(npm:*), Bash(python3:*), Bash(find:*)
---

# ASES `/ases-scaffold`
**Agent:** Architect (Opus) writes spec → Developer (Claude Sonnet) executes

---

## Step A — Opus Writes Scaffold Spec

Read `contracts/hld.json`, `contracts/brief.json`, `contracts/roadmap.json`

Produce `contracts/scaffold_spec.json` — exact instructions for Sonnet.
Every file must have explicit `path`, `type`, and `content`.
No ambiguity — Sonnet receives complete machine-readable instructions.

Schema: `format/json/scaffold_spec.schema.json`

---

## Step B — Sonnet Executes (only after scaffold_spec.json exists)

**Pre-check:** Read `contracts/scaffold_spec.json`.
If it does not exist → STOP. Output: "scaffold_spec.json missing — Opus must complete Step A first."

Read `contracts/scaffold_spec.json` completely before creating any file.

**Permitted file types (whitelist — nothing else):**
- `*.toml` `*.json` `*.ini` `*.cfg` `*.yaml` `*.yml` — config files
- `.gitignore` `.env.example` `.claudeignore` — project root files
- `__init__.py` — empty body only: `# module init\n`
- `index.ts` — type exports only: `export * from './types'`
- `README.md` — headings and placeholder text only
- `requirements.txt` `package.json` `pyproject.toml` — dependency manifests

**Any file not matching this whitelist → do not create → flag in output.**

Create each file exactly as specified in `scaffold_spec.files_to_create[]`.
Install each package in `scaffold_spec.packages_to_install[]`.

---

## Step C — Verify and Report

Run the `verify_cmd` from `scaffold_spec.json`:
```bash
!`<scaffold_spec.verify_cmd>`
```

Report the actual output. Compare against `expected_verify_output`.
If output does not match → list the discrepancy. Do NOT attempt to fix — report only.

---

## Step D — Write Scaffold Manifest

Write `contracts/scaffold.json` — record of every file created:
```json
{
  "created_at": "<ISO-8601>",
  "tech_stack": {},
  "files": [
    { "path": "", "type": "", "hash": "" }
  ],
  "verify_cmd": "",
  "verify_output": "<actual output>",
  "verify_passed": true
}
```

Write `docs/scaffold.md` — human-readable summary for PO.

## Next Step
→ Sprint cycle begins: `/ases-prd-update [sprint-id]` (optional) then `/ases-lld S1`
