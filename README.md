# ASES — AI Scrum Engineering System

Stop prompting. Start running an AI software system.

---

## 📑 Table of Contents

- [⚡ Quick Start](#-quick-start)
- [🧪 Demo](#-demo--from-idea-to-structured-project)
- [🧠 What this does](#-what-this-does)
- [🚨 Why this exists](#-why-this-exists)
- [⚡ What ASES is](#-what-ases-is)
- [🔁 Workflow](#-workflow)
- [🎛️ Command Center](#-command-center)
- [⚡ Context System](#-context-system)
- [🤖 Model Usage](#-model-usage-optional)
- [📂 What ASES manages](#-what-ases-manages)
- [🧰 Requirements](#-requirements)
- [🎯 Who this is for](#-who-this-is-for)
- [🚫 Not for](#-not-for)
- [🙏 Acknowledgements](#-acknowledgements)
- [💬 Feedback](#-feedback)
- [⭐ Support](#-support)
- [🧠 Final Thought](#-final-thought)

---

## ⚡ Quick Start

1. Copy this repo into your project folder  
2. Open your project in Claude Code  
3. Run:

    /ases-interview

That’s it. ASES will guide you through the workflow.

---

## 🧪 Demo — From Idea to Structured Project

Example: Build a simple habit tracker app

### Step 1 — Start

    /ases-interview

ASES asks a few structured questions.

---

### Step 2 — PRD generated

**PRD.md**

# Habit Tracker

## Features
- Create and manage habits  
- Track daily completion  
- View streaks  

---

### Step 3 — Architecture

    /ases-hld

**HLD.md**

- Frontend: React  
- Backend: API  
- Database: PostgreSQL  

---

### Step 4 — Tasks

    /ases-tasks

**tasks.json**

- Setup project  
- Build API  
- Build UI  

---

### Step 5 — Execute

    /ases-dev TASK-002

ASES generates code using only relevant context.

---

## 🧠 What this does

Instead of:

Prompt → Response → Prompt → Confusion  

You get:

Idea → PRD → Architecture → Tasks → Execution  

---

## 🚨 Why this exists

When using LLMs for real projects:

- Context becomes messy  
- Outputs drift  
- You repeat yourself  
- Token usage increases  

ASES introduces structure to solve this.

---

## ⚡ What ASES is

- A structured Scrum-style workflow for AI  
- A way to organize AI-driven development  
- A system for managing context and outputs  

---

## 🔁 Workflow

### Setup

    /ases-interview → /ases-prd → /ases-hld → /ases-roadmap

### Sprint

    Design → Execute → Ship

### Execution Loop

    Analyze → Build → Critique → Fix

---

## 🎛️ Command Center

ASES includes a Command Center that shows how the system works.

Location:

    command-centre/ases.html

---

### What it is

The Command Center is a **visual and descriptive interface**.

It helps you understand:

- Available commands (`/ases-*`)
- Skills and responsibilities  
- Hooks and system behavior  
- Workflow stages  

---

### What it is not

- Not a runtime controller  
- Not part of execution  

All execution happens through:

- `.claude/commands`
- `.claude/skills`
- `.claude/hooks`

---

### Why it exists

- Makes the system easier to understand  
- Shows how everything connects  
- Acts as a reference while using ASES  

---

## ⚡ Context System

ASES separates context into layers:

- Lean → always available  
- Sprint → current work  
- Global → injected when needed  

Example:

    /ases-inject DS-003 SP-001

---

## 🤖 Model Usage (optional)

- Claude Opus → planning  
- Claude Sonnet → execution  
- Gemini → UI  

You can use one model or multiple.

---

## 📂 What ASES manages

- PRD (requirements)  
- HLD / LLD (architecture)  
- Tasks  
- Tests  
- `.ases/` project state  

---

## 🧰 Requirements

- Claude Code  
- Git  

---

## 🎯 Who this is for

- Developers building real AI projects  
- People struggling with context management  
- Anyone who wants structured workflows  

---

## 🚫 Not for

- One-off prompts  
- Quick experiments  

---

## 🙏 Acknowledgements

Inspired by ideas from:

- Claude-Mem (memory structuring)  
- CARL (runtime context control)  

---

## 💬 Feedback

If you try this, share:

- What worked  
- What didn’t  
- What felt unnecessary  

---

## ⭐ Support

If this is useful, give it a star.

---

## 🧠 Final Thought

Unstructured → Unpredictable → Expensive  

Structured → Repeatable → Controlled  