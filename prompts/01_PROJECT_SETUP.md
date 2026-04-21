# 01 — Project Setup

In Cursor Agent chat, ask:
`Read and summarize all files in this repository so you understand the project before we run prompts.`
Then ask:
`Run prompts/01_PROJECT_SETUP.md in this repository. Execute all steps.`
Approve any terminal commands or file edits the agent asks to run.
**Note:** If Cursor asks you to approve commands or file edits, approve them so the agent can complete the setup.

AGENT OPERATING MODE (IMPORTANT):
- Start by showing a short overall plan for the full setup.
- Then run through the setup to completion without step-by-step stopping.
- When running terminal commands, batch them where it is safe to do so (to reduce approval prompts).
- If Cursor asks for approval, approve quickly and continue (do not pause after each individual command).
- Do final verification at the end (URLs + health checks), then summarize results.
- If anything fails, fix it and continue without adding extra checkpoints.

---

Set up a complete full-stack project from scratch in this folder. Execute every step — run every command, create every file, verify everything works. Do not ask me questions — just do it.

PROJECT OVERVIEW:
- This is a workshop project. We will build a product prototype using Spec-Driven Development.
- Frontend: React 18 with TypeScript, built with Vite, styled with Tailwind CSS
- Backend: Python with FastAPI
- Database: SQLite (file-based, no server needed)
- UI text: Norwegian (Bokmål). Code and comments: English

STEP-BY-STEP:

1. INITIALIZE GIT (skip if .git already exists)
   Run: git init

2. CREATE .gitignore in the project root:
   node_modules/
   dist/
   .env
   .DS_Store
   __pycache__/
   *.pyc
   .venv/
   *.db

3. SCAFFOLD THE REACT FRONTEND
   - Run: npm create vite@latest frontend -- --template react-ts
   - cd frontend && npm install
   - Install Tailwind: npm install -D tailwindcss @tailwindcss/vite
   - Update vite.config.ts to include the tailwindcss plugin AND a proxy so /api/* requests go to http://localhost:8000:

     import { defineConfig } from 'vite'
     import react from '@vitejs/plugin-react'
     import tailwindcss from '@tailwindcss/vite'
     export default defineConfig({
       plugins: [react(), tailwindcss()],
       server: { proxy: { '/api': 'http://localhost:8000' } }
     })

   - Replace the contents of src/index.css with ONLY this line:
     @import "tailwindcss";

   - Replace src/App.tsx with:

     function App() {
       return (
         <div className="min-h-screen bg-gray-50 flex items-center justify-center">
           <div className="text-center">
             <h1 className="text-4xl font-bold text-gray-900 mb-4">SDD Workshop</h1>
             <p className="text-lg text-gray-600">Prosjektet er klart. La oss bygge noe.</p>
           </div>
         </div>
       )
     }
     export default App

   - Delete any unused default Vite files (svg logos, default css files not needed, counter components, etc.)
   - cd back to project root

4. SCAFFOLD THE FASTAPI BACKEND
   - Create backend/ directory
   - Create backend/requirements.txt:
     fastapi>=0.110.0
     uvicorn>=0.27.0

   - Create backend/main.py:

     from fastapi import FastAPI
     from fastapi.middleware.cors import CORSMiddleware
     import sqlite3
     import os

     app = FastAPI(title="SDD Workshop API")

     app.add_middleware(
         CORSMiddleware,
         allow_origins=["http://localhost:5173"],
         allow_credentials=True,
         allow_methods=["*"],
         allow_headers=["*"],
     )

     DB_PATH = os.path.join(os.path.dirname(__file__), "workshop.db")

     def get_db():
         conn = sqlite3.connect(DB_PATH)
         conn.row_factory = sqlite3.Row
         return conn

     @app.get("/api/health")
     def health_check():
         return {"status": "ok", "message": "Backend kjører"}

   - Set up a Python virtual environment and install dependencies:

     # Unix / macOS / WSL:
     cd backend
     python3 -m venv .venv
     source .venv/bin/activate
     python3 -m pip install -r requirements.txt
     cd ..

     # Windows (PowerShell):
     # cd backend
     # py -3 -m venv .venv
     # .venv\Scripts\activate
     # pip install -r requirements.txt
     # cd ..

     # If python3 or pip is missing on Ubuntu/WSL:
     # sudo apt update && sudo apt install python3 python3-pip python3-venv

   - Confirm: with the venv activated, run `python -c "import fastapi; print('OK')"` — it should print OK.

5. CREATE CURSOR PROJECT RULES
   - Create directory: .cursor/rules/
   - Create .cursor/rules/project.mdc with this exact content:

     ---
     description: Project-wide rules for SDD Workshop
     alwaysApply: true
     ---
     # SDD Workshop
     Full-stack workshop project built using Spec-Driven Development.
     ## Tech Stack
     - Frontend: React 18 + TypeScript + Vite
     - Styling: Tailwind CSS (utility-first, use Tailwind classes only)
     - Backend: Python 3.10+ with FastAPI
     - Database: SQLite (file-based, use sqlite3 standard library)
     - API: REST endpoints under /api/ prefix
     ## Conventions
     - UI text: Norwegian (Bokmål)
     - Code and comments: English
     - File naming: kebab-case for files, PascalCase for React components
     - One React component per file
     - All API endpoints start with /api/
     - Frontend proxies /api/* to backend via Vite config
     ## Quality
     - Workshop prototype — working beats polished
     - No authentication required
     - No tests required
     - Clear, readable code

6. VERIFY EVERYTHING WORKS
   - Start the backend (keep this terminal open):
     cd backend && source .venv/bin/activate && uvicorn main:app --reload --host 127.0.0.1 --port 8000
     # Windows: cd backend && .venv\Scripts\activate && uvicorn main:app --reload --host 127.0.0.1 --port 8000
   - Start the frontend in a SECOND terminal (keep both terminals open):
     cd frontend && npm run dev
   - Confirm all of the following work:
     a) http://localhost:5173 shows the React app with "SDD Workshop"
     b) http://localhost:8000/docs shows the FastAPI Swagger docs
     c) http://localhost:8000/api/health returns {"status": "ok"}
     d) http://localhost:5173/api/health returns {"status": "ok"} — confirms the Vite proxy is forwarding to the backend
   - Report what you see for each URL

IMPORTANT — RUNNING THE PROJECT AFTER SETUP:

The dev servers (frontend + backend) are NOT background services. They stop when you close the terminal.
After setup is complete, this is how you run the project day-to-day:

   Terminal 1 (backend):
   cd backend && source .venv/bin/activate && uvicorn main:app --reload

   Terminal 2 (frontend):
   cd frontend && npm run dev

   Rule: If /api/ requests fail or /docs won't load, check Terminal 1 first — the backend has probably stopped.

7. CHECKPOINT COMMIT (RECOMMENDED)
   - Stop the servers
   - Run: git add -A && git commit -m "initial: project skeleton — React + FastAPI + SQLite"
   - If short on time, continue and commit after Prompt 03 instead.

When you are done, tell me the result of each verification step and confirm the git commit was successful.
