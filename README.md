# SDD Workshop Template

A template for running Spec-Driven Development workshops where participants go from business idea to working prototype in ~3.5 hours.

## How it works

1. **Product Discovery** (~60 min) — A Claude-based AI agent guides the group through structured product discovery: understanding the business, identifying opportunities, validating a problem, and defining a buildable product. Output: `product_vision.md`

2. **Project Setup** (~15 min) — Participants scaffold a full-stack project (React + FastAPI + SQLite) using an AI coding agent in Cursor.

3. **Spec-Driven Build** (~2 hours) — Using GitHub Spec Kit, participants describe features in plain language and the AI builds them: specify → plan → tasks → implement.

## Quick start

1. Use this template to create a new repo
2. Read `FACILITATOR.md` for the full workshop guide
3. Follow prompts in `prompts/` folder in order (01 → 02 → 03)

## Workshop start flow (facilitator)

This is the recommended startup sequence for every workshop:

1. In GitHub, create a new workshop repository from this template.
2. Clone that new repository to your computer (this means downloading a local copy).
3. Open the cloned repository in Cursor.
4. In Agent chat, first ask:
   `Read and summarize all files in this repository so you understand the project before we run prompts.`
5. Then run prompts 01 → 02 → 03 in order.

## Operator flow (agent-first)

In Cursor Agent chat, run the prompts in order:

1. `Run prompts/01_PROJECT_SETUP.md in this repository.`
2. `Run prompts/02_SPECKIT_INSTALL.md in this repository.`
3. `Run prompts/03_CONSTITUTION.md in this repository.`

Tip: Ask the agent to show a short plan first, then run through setup to completion. Approve when prompted.

## Prerequisites for participants

- Git, Node.js 18+, Python 3.10+, Cursor IDE, uv
- See `FACILITATOR.md` for the full prep checklist

---
**Spec-Driven Development with Cursor & Spec-Kit — by Code11**

This repository is a template for running hands-on SDD workshops. Create a new repo from this template for each workshop, invite participants, and follow the steps below.

---

## Prerequisites

Each participant needs:

- **Git** — `git --version`
- **Node.js 18+** — `node --version` ([nodejs.org](https://nodejs.org))
- **Python 3.10+** — `python3 --version` ([python.org](https://python.org))
- **Cursor IDE** — [cursor.com](https://cursor.com) (needs an account with Agent mode)

---

## Workshop Flow

| Step | File | Time | What happens |
|------|------|------|--------------|
| 1 | `prompts/01_PROJECT_SETUP.md` | 10 min | Ask Cursor Agent to run the prompt file → full-stack skeleton appears |
| 2 | `prompts/02_SPECKIT_INSTALL.md` | 5 min | Ask Cursor Agent to run the prompt file → spec-kit scaffolding ready |
| 3 | `prompts/03_CONSTITUTION.md` | 20 min | Ask Cursor Agent to run the prompt file → interactive constitution Q&A |
| 4 | Start building | — | `/speckit.specify` → `/speckit.plan` → `/speckit.tasks` → `/speckit.implement` |

---

## Quick Start

```bash
# 1. Clone your workshop repo
git clone https://github.com/<org>/<workshop-repo>.git
cd <workshop-repo>

# 2. Open in Cursor
cursor .

# 3. In Cursor Agent chat, run:
#    "Run prompts/01_PROJECT_SETUP.md in this repository."
#    Then approve commands/edits when prompted.

# 4. Then run prompts 02 and 03 the same way:
#    "Run prompts/02_SPECKIT_INSTALL.md in this repository."
#    "Run prompts/03_CONSTITUTION.md in this repository."
#    Ask for a short plan first, then run through setup to completion. Approve when prompted.

# 5. Start building features with /speckit.specify
```

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18 + TypeScript + Vite |
| Styling | Tailwind CSS |
| Backend | Python 3.10+ + FastAPI |
| Database | SQLite |
| API | REST under `/api/` prefix |

---

## For Workshop Facilitators

See `FACILITATOR.md` for setup instructions, timing guidance, and troubleshooting.
