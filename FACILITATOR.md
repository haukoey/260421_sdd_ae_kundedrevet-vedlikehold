# Facilitator Guide

## Workshop Overview

This workshop takes participants from a raw business context to a working product prototype in approximately 3.5 hours. It has two major phases:

**Phase A — Product Discovery** (~60 min, in Claude)
A Claude-based Product Discovery Agent guides the group through structured questioning to produce a `product_vision.md` — the strategic foundation for the build.

**Phase B — Spec-Driven Development** (~2 hours, in Cursor)
Participants use GitHub Spec Kit in Cursor to turn the product vision into working software through the specify → plan → tasks → implement cycle.

---

## Before the Workshop

### 1. Create a new repo from this template
- Go to this template repo → "Use this template" → "Create a new repository"
- Name it `sdd-workshop-[client]` or `sdd-workshop-[date]`
- Set to Private
- Clone this new workshop repo to your machine and open it in Cursor

### 2. Set up the Product Discovery Agent
- Create a new Claude Project (claude.ai → Projects → New Project)
- Paste the Product Discovery Agent instruction file into the Project Instructions
- Test it with a sample business to verify the flow works

### 3. Invite participants as collaborators
- Repo → Settings → Collaborators → Add people

### 4. Send prep message

```
Hei!

Vi kjører en hands-on workshop der vi bygger en fullstack webapp
fra scratch med AI. Du trenger laptopen din med dette installert:

1. Git — sjekk med: git --version
2. Node.js 18+ — sjekk med: node --version (nodejs.org)
3. Python 3.10+ — sjekk med: python3 --version (python.org)
4. Cursor IDE — last ned fra cursor.com (du trenger en konto)
5. uv (Python package installer) — curl -LsSf https://astral.sh/uv/install.sh | sh

Du har fått en invite til GitHub-repoet — godkjenn den.
Ta med lader!
```

### 5. Pre-workshop checklist
- [ ] Template repo cloned and tested on your machine
- [ ] Claude Project with Product Discovery Agent instructions ready
- [ ] Screen sharing / projector set up for the Claude discovery phase
- [ ] Participants have accepted GitHub invite
- [ ] You've run through Prompts 01–03 yourself at least once

---

## During the Workshop

### Timing

| Phase | Duration | Tool | Notes |
|-------|----------|------|-------|
| **Phase A: Product Discovery** | | | |
| Welcome + intro | 5 min | Projector | Explain the two-phase structure |
| Product Discovery — business context | 10–15 min | Claude (projected) | Agent researches the business, group answers on screen |
| Product Discovery — opportunities | 15–20 min | Claude (projected) | Agent surfaces challenges, group votes and ranks |
| Product Discovery — problem validation | 10–15 min | Claude (projected) | Narrow to one problem, test feasibility |
| Product Discovery — product definition | 15–20 min | Claude (projected) | Name, features, positioning, scope for workshop |
| Product Vision review | 5 min | Claude (projected) | Agent generates product_vision.md, group reviews |
| **Break** | **10 min** | | Facilitator saves product_vision.md during break |
| **Phase B: Spec-Driven Development** | | | |
| Prompt 01 — Project setup | 10 min | Cursor | Everyone asks Cursor Agent to run `prompts/01_PROJECT_SETUP.md` |
| Prompt 02 — Spec Kit install | 5 min | Terminal | Terminal commands, not Cursor agent |
| Prompt 03 — Constitution | 15–20 min | Cursor | Add product_vision.md to memory, then run /speckit.constitution |
| Feature 1 — full SDD cycle | 45 min | Cursor | specify → plan → tasks → implement |
| Break | 10 min | | |
| Feature 2 — second cycle | 30 min | Cursor | Faster this time |
| Debrief | 15 min | Open discussion | What surprised you? What would you build? |
| **Total** | **~3.5 hours** | | |

### Phase A: Running the Product Discovery session

1. Open the Claude Project with the Product Discovery Agent on the projector
2. Start a new conversation — the agent will begin by asking about the business
3. The group reads questions on the wall and decides together how to answer
4. For multiple-choice questions: the group votes, facilitator clicks the answer
5. For open-ended questions: the group discusses, facilitator types a summary
6. The agent will guide through: business context → opportunities → problem validation → product definition
7. At the end, the agent generates `product_vision.md` — review it as a group and save the file

**Facilitator tips for Phase A:**
- Don't rush the opportunity discovery phase — this is where the real value is
- If the group is split on a direction, let the agent propose a decision framework
- If the product idea is too ambitious, the agent will help find a "minimum lovable slice"
- Save `product_vision.md` to your machine during the break so it's ready for Phase B

### Phase B: Running the build session

1. Everyone clones the repo and opens it in Cursor
2. Before Prompt 01, ask the agent to read and summarize all repository files so it has full context
3. Walk through Prompts 01 → 02 → 03 in order by asking Cursor Agent to run each prompt file
4. During Prompt 03: add `product_vision.md` into `.specify/memory/` before running `/speckit.constitution`
5. For Feature 1: the facilitator drives `/speckit.specify` on the projector while the group describes the feature
6. For Feature 2: participants can work independently or in pairs

**Suggested operator script (say this out loud):**
- "First ask: `Read and summarize all files in this repository so you understand the project before we run prompts.`"
- "Open Agent chat and ask: `Run prompts/01_PROJECT_SETUP.md in this repository.`"
- "Approve prompts for terminal commands and file edits so the agent can proceed."
- "When done, repeat with Prompt 02, then Prompt 03."
- "Ask the agent to show a plan first, then run through setup to completion. Approve when prompted."

**Key teaching moments:**
- **During Prompt 01:** "The AI just set up an entire full-stack project from a description. No manual configuration."
- **During Prompt 03:** "This constitution is the DNA of the project. Every line of code the AI writes will respect these principles. And it's grounded in the product vision we just created together."
- **During Feature 1 specify:** "Write it like you're explaining to a smart colleague. Be specific about WHAT and WHO, not HOW."
- **When something goes wrong:** "The fix is in the spec, not in the code. Let's look at what we told the AI."
- **After Feature 1 runs:** "You just built working software by describing a problem."
- **Connection to Phase A:** "The reason this works is because we spent an hour getting clear on WHAT to build and WHY. That clarity is what makes the AI effective."

### The debrief takeaway

> "The bottleneck is no longer coding. It's knowing what to build and describing it clearly. That's a business skill, not a technical one. That's why we started with product discovery, not with code."

---

## Troubleshooting

### Phase A (Product Discovery in Claude)

| Problem | Fix |
|---|---|
| Agent asks too many questions | Tell it: "Let's move to the next phase" |
| Agent proposes an unbuildable product | Remind it of the 2–3 hour workshop constraint |
| Group can't agree on direction | Ask the agent to propose a decision framework |
| Agent doesn't research the business | Prompt: "Search the web for [company name] and tell us what you find" |

### Phase B (Setup & Build in Cursor)

| Problem | Fix |
|---|---|
| `pip install` fails | Use venv: `python3 -m venv .venv && source .venv/bin/activate && python3 -m pip install -r requirements.txt` |
| `pip` not found (Ubuntu/WSL) | Run: `sudo apt install python3-pip python3-venv` |
| `uvicorn` not found after install | Activate the venv first: `source backend/.venv/bin/activate` |
| Backend was working, now /api fails | Backend server stopped — restart it in a terminal |
| Frontend works but /api returns errors | Backend isn't running — check Terminal 1 |
| Agent is working in wrong folder | Tell it: "Use the project root folder, then rerun this prompt file." |
| A command was denied accidentally | Re-run the same prompt file and approve commands/edits when asked |
| Cursor doesn't show slash commands | Restart Cursor after Spec Kit install (Cmd+Shift+P → Reload Window) |
| `specify` command not found | `uv tool install specify-cli --from git+https://github.com/github/spec-kit.git` |
| `--ai cursor-agent` fails | Add `--ignore-agent-tools` flag |
| `npm create vite` hangs | Try `npx create-vite@latest frontend --template react-ts` |
| FastAPI won't start | Check port 8000 isn't in use: `lsof -i :8000` |
| SQLite errors | Delete `workshop.db` and restart backend |
| AI generates wrong stack | Check `.cursor/rules/project.mdc` and `.specify/memory/constitution.md` |
| AI ignores constitution | Ensure constitution.md has content; start a fresh Agent chat |
| /docs shows blank page | Swagger UI loads JS from CDN — try `/openapi.json` or `/redoc` instead |

---

## File Reference

After full setup, the project structure should look like:

```
project-root/
├── .cursor/
│   ├── commands/           ← Spec Kit slash commands
│   └── rules/
│       └── project.mdc     ← Cursor project rules
├── .specify/
│   ├── memory/
│   │   ├── constitution.md  ← Project principles & constraints
│   │   └── product_vision.md ← Product vision from discovery session
│   └── templates/           ← Spec Kit templates
├── backend/
│   ├── .venv/               ← Python virtual environment
│   ├── main.py              ← FastAPI application
│   └── requirements.txt
├── frontend/
│   ├── src/
│   │   ├── App.tsx
│   │   └── index.css
│   ├── package.json
│   └── vite.config.ts
├── prompts/
│   ├── 01_PROJECT_SETUP.md
│   ├── 02_SPECKIT_INSTALL.md
│   └── 03_CONSTITUTION.md
├── .gitignore
├── FACILITATOR.md
└── README.md
```
