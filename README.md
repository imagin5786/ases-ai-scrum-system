# ASES — AI Scrum Engineering System

> *This is not prompting. This is an AI software factory.*

ASES is a structured, Scrum-based engineering system that runs inside Claude Code. It turns an open-ended AI session into a disciplined delivery pipeline — with defined roles, schema-validated outputs, human approval gates, and hook-based context injection — from first idea to production release.

---

## Table of Contents

- [What ASES Actually Is](#what-ases-actually-is)
- [Why It Exists](#why-it-exists)
- [How It Works](#how-it-works)
- [The Full Pipeline](#the-full-pipeline)
- [Context Architecture](#context-architecture)
- [Model Allocation](#model-allocation)
- [Command Reference](#command-reference)
- [Folder Structure](#folder-structure)
- [Hard Rules](#hard-rules)
- [Human Gates](#human-gates)
- [Quick Start](#quick-start)
- [Requirements](#requirements)
- [Who This Is For](#who-this-is-for)

---

## What ASES Actually Is

Most AI coding workflows are one of two things: a single big prompt in `CLAUDE.md`, or a loose collection of slash commands with no enforcement between them. Both break down the moment a project gets complex.

ASES is neither. It is a three-phase sprint engine with:

- **Hook-based context injection** at three levels (lean, sprint, global) — loaded automatically, not manually pasted
- **Dual-file outputs** at every stage — `name.json` for agents, `name.md` for humans
- **Schema validation at every gate** — nothing advances until it passes
- **A mandatory critique loop** — every dev task goes through validate → build → critique → fix before it's done
- **6 human approval gates per sprint** — the Product Owner is part of the system, not an afterthought
- **A typed architectural decision log** (`decisions.json`) — every tradeoff gets a permanent `DS-NNN` ID
- **Multi-model routing** — Opus for planning and critique, Sonnet for execution, Gemini for UI scaffolding

This is what Scrum actually looks like when it's built for AI.

---

## Why It Exists

When you use an LLM to build something real, a few things happen:

- Context grows and gets messy across sessions
- Outputs drift from the original spec
- You repeat the same decisions because nothing was recorded
- Token usage climbs because everything is always in context
- The agent "completes" tasks that aren't actually done

ASES fixes each of these with structure. Context is layered and injected by hook. Decisions are logged with typed IDs. Outputs are validated by schema. Completion requires critique, not just generation. The human stays in control at every meaningful boundary.

---

## How It Works

The core execution loop is:

```
/ases-validate → /ases-dev → /ases-critique → /ases-fix
```

Every task goes through all four stages. The critique loop runs up to 3 iterations. If it can't resolve, it escalates to the Product Owner — it does not silently pass.

Above that loop, a full sprint flows through three phases:

**Phase 1 — Design:** Define the sprint, produce LLD + schema + test spec, get gate approval before any code runs.

**Phase 2 — Execution:** Analyze, scaffold, build task by task through the validate/dev/critique/fix loop. UI tasks go through a separate Gemini-driven path.

**Phase 3 — Ship:** Unit → integration → system → UAT → DevOps → final audit → release. Git commits only happen after UAT approval, enforced by `ases-hook.py`.

---

## The Full Pipeline

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
/ases-ui-design → /ases-ui-review → /ases-ui-scaffold   [UI tasks only]
per task: /ases-validate → /ases-dev → /ases-critique → /ases-fix
/ases-sprint-close
```

**Phase 3 — Sprint Ship**
```
/ases-test-impl → /ases-integration-test → /ases-system-test
/ases-uat → /ases-devops → /ases-final-audit → /ases-release
```

---

## Context Architecture

ASES separates context into three levels, each with its own file and injection rule:

| Level | File | When Loaded | Access Pattern |
|-------|------|-------------|----------------|
| 3 — Lean | `.ases/context.json` | Hook — every session | Always available |
| 2 — Sprint | `.ases/sprint_context.json` | Hook — active sprint only | Loaded during sprint sessions |
| 1 — Global | `.ases/global_context.json` | Explicit call only | `/ases-inject [IDs]` or PO commands |

Global context uses identifier-addressed entries:

| Prefix | Type |
|--------|------|
| `SP-NNN` | Sprint digest |
| `DS-NNN` | Architectural decision |
| `TD-NNN` | Tech debt item |
| `FT-NNN` | Feature record |
| `RI-NNN` | Risk item |
| `CF-NNN` | Carry-forward |

Surgical injection of specific entries: `/ases-inject DS-003 SP-001`

This means lean context is always available without token waste, sprint context exists only while relevant, and global context is never loaded speculatively.

---

## Model Allocation

| Layer | Model | Role |
|-------|-------|------|
| Reasoning | Claude Opus 4.6 | Planning, architecture, critique, test design |
| Execution | Claude Sonnet | Code generation, fixes, scaffolding, test implementation |
| UI | Gemini 3.1 Pro | UI spec + Next.js/React scaffold |
| Decision | Product Owner (Human) | 6 mandatory approval gates per sprint |

Sonnet never touches the Gemini UI scaffold — only declared `integration_points`. Role separation is enforced, not suggested.

---

## Command Reference

### Project Setup
| Command | What It Does |
|---------|--------------|
| `/ases-interview` | Structured project discovery — extracts goals, constraints, and scope |
| `/ases-prd` | Generates Product Requirements Document (JSON + Markdown) |
| `/ases-hld` | High-Level Design — architecture, tech stack, system boundaries |
| `/ases-roadmap` | Sprint roadmap across milestones |
| `/ases-init` | Initialises `.ases/` state and folder structure |
| `/ases-scaffold` | Generates project scaffold from HLD contracts |

### Sprint Design (Phase 1)
| Command | What It Does |
|---------|--------------|
| `/ases-prd-update` | Updates PRD when scope changes mid-project |
| `/ases-lld` | Low-Level Design — component specs, interfaces, data flows |
| `/ases-schema` | Database / data model schema generation |
| `/ases-test-spec` | Test cases derived from PRD acceptance criteria |
| `/ases-sprint-gate` | Gate check — validates Phase 1 outputs before execution begins |

### Sprint Execution (Phase 2)
| Command | What It Does |
|---------|--------------|
| `/ases-analyze` | Codebase analysis before execution begins |
| `/ases-sprint-scaffold` | Sprint-level scaffolding from LLD |
| `/ases-tasks` | Task decomposition into `tasks.json` |
| `/ases-ui-design` | UI spec generation (Gemini-routed) |
| `/ases-ui-review` | UI design review before scaffold |
| `/ases-ui-scaffold` | UI component scaffold (Gemini-routed) |
| `/ases-validate` | Pre-execution task validation |
| `/ases-dev` | Code generation for a specific task |
| `/ases-critique` | Post-dev critique — up to 3 iterations before PO escalation |
| `/ases-fix` | Targeted fix from critique output |
| `/ases-sprint-close` | Closes sprint execution phase, updates state |

### Sprint Ship (Phase 3)
| Command | What It Does |
|---------|--------------|
| `/ases-test-impl` | Unit test implementation from test spec |
| `/ases-integration-test` | Integration test execution |
| `/ases-system-test` | Full system test |
| `/ases-uat` | User Acceptance Testing — PO reviews running system |
| `/ases-devops` | Deployment pipeline and environment validation |
| `/ases-final-audit` | Pre-release audit — SHIP or BLOCK verdict |
| `/ases-release` | Production release — only runs after SHIP verdict + PO gate |

### Context Management
| Command | What It Does |
|---------|--------------|
| `/ases-inject [IDs]` | Injects specific global context entries by ID |

---

## Folder Structure

```
/.ases/                          ← System state (hook reads, never project outputs)
  context.json + context.md          Level 3: lean, always loaded
  sprint_context.json + sprint_context.md  Level 2: sprint-scoped
  global_context.json + global_context.md  Level 1: explicit only
  decisions.json + decisions.md      ADR log, PO-managed

/format/                         ← ASES schemas + templates (excluded from agent auto-scan)
  json/
  markdown/

/docs/                           ← Human-readable PO documents
  brief.md  prd.md  hld.md  roadmap.md  scaffold.md

/contracts/                      ← Machine-readable JSON contracts
  brief.json  prd.json  hld.json  roadmap.json  scaffold.json

/sprints/SN/
  design/                        ← Phase 1: lld, schema, test_cases, sprint_gate, scaffold_spec
  execution/                     ← Phase 2: analysis, tasks, ui_*, snapshots/, tasks/T-NNN-*
  ship/                          ← Phase 3: test_suite, integration, system, uat, devops, audit, release

/ui/                             ← Gemini scaffold (locked after creation)
/tests/
  unit/
  integration/
  system/
/src/                            ← Project source code
```

---

## Hard Rules

These are never negotiated. The system enforces them.

1. JSON-only outputs between all pipeline stages
2. Every document has two files: `name.json` (agent) + `name.md` (human)
3. File-level isolation for every `/ases-dev` and `/ases-fix` — write only to `output_files[]` declared in the task plan
4. Critique loop is mandatory — max 3 iterations, then escalate to PO
5. No agent role overlap — Critic detects only, Sonnet implements only
6. Schema validation at every gate transition
7. Sonnet never touches Gemini UI scaffold — only declared `integration_points`
8. Git commit only after UAT approval — `ases-hook.py` enforces this
9. Every architectural tradeoff → ADR entry in `decisions.json` with typed `DS-NNN` ID
10. Test cases derived from PRD `acceptance_criteria` — never invented by the agent

---

## Human Gates

ASES stops and waits at 6 mandatory points per sprint. These cannot be bypassed.

| Gate | Trigger |
|------|---------|
| 1 | After `/ases-roadmap` — before `/ases-init` |
| 2 | After `/ases-sprint-gate` PASS — before `/ases-analyze` |
| 3 | Any `ESCALATE` verdict from `/ases-critique` |
| 4 | `/ases-uat` — PO reviews the running system |
| 5 | After `/ases-final-audit` SHIP — before `/ases-release` |
| 6 | Any `BLOCK` verdict — directed surgical re-entry |

---

## Quick Start

```bash
# 1. Copy ASES into your project
cp -r ases-ai-scrum-system/. your-project/

# 2. Open your project in Claude Code
cd your-project && claude

# 3. Start the intake process
/ases-interview
```

ASES will guide you from there. The interview captures your project scope, constraints, and goals. Everything that follows is driven by what gets captured here.

---

## Requirements

- **Claude Code** — ASES lives inside it
- **Git** — enforced at release gate
- **Gemini access** (optional) — required for UI scaffold path only; everything else runs on Claude

---

## Who This Is For

**Developers building real, multi-sprint projects with Claude Code** who want the structure of a software delivery team without the overhead of actually managing one.

Specifically:
- You've hit the wall where unstructured AI sessions start producing inconsistent outputs
- You want decisions recorded, not just made
- You want the agent to critique its own work, not just submit it
- You want a human checkpoint before things go to production
- You want to know exactly what context the agent has access to at any point

**Not for:** one-off scripts, quick experiments, or single-session tasks. ASES has setup overhead — it pays off on projects that span multiple sprints.

---

## Command Centre

ASES ships with a visual reference interface at `command-centre/ases.html`. It shows all available commands, skills, hooks, and workflow stages in one place. It's a reference tool — not a runtime controller. All execution happens through `.claude/commands`, `.claude/skills`, and `.claude/hooks`.

---

## Acknowledgements

Inspired by ideas from [Claude-Mem](https://github.com/thedotmack/claude-mem) (memory structuring) and [CARL](https://github.com/ChristopherKahler/carl) (runtime context control).

---

*ASES v3.0 · 
Unstructured → Unpredictable → Expensive. 
Structured → Repeatable → Controlled.*
