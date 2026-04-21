# 02 — Spec-Kit Install

In Cursor Agent chat, ask:
`Run prompts/02_SPECKIT_INSTALL.md in this repository.`
Approve terminal commands when prompted.
If this is a fresh chat, first ask the agent to read and summarize all repository files for context.

AGENT OPERATING MODE (IMPORTANT):
- Start by showing a short overall plan for the install.
- Then run through the install to completion without step-by-step stopping.
- Batch commands where it is safe to do so (to reduce approval prompts).
- If Cursor asks for approval, approve quickly and continue (do not pause after each individual command).
- Do final verification at the end, then summarize results.
- If anything fails, fix it and continue without adding extra checkpoints.

---

## Step 1: Install uv (if not already installed)

```bash
# macOS / Linux:
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows (PowerShell):
# powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

# Restart your terminal after install
```

## Step 2: Install spec-kit CLI

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

Verify: `specify --help` should show usage info.

## Step 3: Initialize spec-kit in the project

From the project root:

```bash
specify init --here --ai cursor-agent --ignore-agent-tools --force
```

## Step 4: Restart Cursor

Close and reopen Cursor, or: Cmd+Shift+P → "Reload Window"

Verify: type `/` in Agent chat — you should see `speckit.constitution`, `speckit.specify`, etc.

## Step 5: Checkpoint commit (recommended)

```bash
git add -A
git commit -m "speckit: SDD scaffolding installed"
```

If short on time, continue to Prompt 03 and commit there.

## What you now have

```
.cursor/commands/          ← Slash commands for Cursor agent
├── speckit.constitution.md
├── speckit.specify.md
├── speckit.plan.md
├── speckit.clarify.md
├── speckit.tasks.md
├── speckit.implement.md
├── speckit.analyze.md
└── speckit.checklist.md

.specify/memory/
└── constitution.md        ← Template — fill in next step

.specify/templates/
├── spec-template.md
├── plan-template.md
└── tasks-template.md
```

**Next:** Open `prompts/03_CONSTITUTION.md`
