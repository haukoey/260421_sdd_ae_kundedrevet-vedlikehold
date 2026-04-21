# 03 — Constitution

This is a two-part step.
If this is a fresh chat, first ask the agent to read and summarize all repository files for context.

AGENT OPERATING MODE (IMPORTANT):
- Start by showing a short overall plan for the constitution step.
- Then run through the step to completion without step-by-step stopping.
- If Cursor asks for approval, approve quickly and continue (do not pause after each individual command).
- Do final verification at the end, then summarize what was generated/edited.
- If anything fails, fix it and continue without adding extra checkpoints.

---

## Part A: Add the product vision to project memory

1. The facilitator will share the `product_vision.md` file that the group created during the Product Discovery session.

2. Copy the file into the Spec Kit memory folder:
   ```bash
   cp /path/to/product_vision.md .specify/memory/product_vision.md
   ```
   Windows (PowerShell):
   ```powershell
   Copy-Item C:\path\to\product_vision.md .specify\memory\product_vision.md
   ```
   (Or: create the file `.specify/memory/product_vision.md` and paste the content into it.)
   If unsure, ask the agent to create the file and insert the content for you.

3. In Cursor Agent chat, ask the agent to:
   - Read `.specify/memory/product_vision.md` carefully
   - Confirm understanding of:
     1. What the product is
     2. Who the target user is
     3. What the 2-3 core features are for this workshop
     4. What the technical constraints are
   - Do not generate any code yet

Review the agent's summary. If it matches what the group intended, proceed. If anything looks off, correct it now — this understanding shapes everything that follows.

---

## Part B: Build the constitution

Type in Agent chat:

```
/speckit.constitution
```

The agent will walk you through an interactive Q&A. Guide the group through these decisions:

### Project identity
One sentence — use the value proposition from product_vision.md as a starting point.
The agent has already read the vision document, so it should propose this automatically.

### Core principles
- Workshop prototype — working beats polished
- Every feature must work on mobile
- UI language: Norwegian (Bokmål)
- No authentication for v1
- SQLite database — no external infrastructure

### Tech stack (confirm what's already set up)
- Frontend: React 18 + TypeScript + Tailwind CSS + Vite
- Backend: Python 3.10+ with FastAPI
- Database: SQLite
- API: REST under /api/ prefix
- Frontend proxies /api/* to backend via Vite config

### Quality
- No tests required
- Clear, readable code
- Prototype quality

---

## After the constitution is generated

1. Open `.specify/memory/constitution.md` and review it as a group
   - Verify it references the product vision — the constitution should reflect the product's target user, core features, and technical constraints from product_vision.md
2. Edit anything that doesn't fit
3. Checkpoint commit (recommended):

```bash
git add -A
git commit -m "speckit: constitution established"
```

If short on time, this commit can be done together with your first feature commit.

---

## You're ready

Start building features:

```
/speckit.specify
```

Describe your first feature in plain language. Then follow with `/speckit.plan`, `/speckit.tasks`, `/speckit.implement`.
