# Markedsdrevet Vedlikehold Constitution

## Core Principles

### I. Product-Vision Grounded Delivery
All implementation decisions MUST align with the product vision in `.specify/memory/product_vision.md`.
Every feature should help driftsleder choose a maintenance window that minimizes estimated revenue loss.
If trade-offs appear, prioritize user decision value and clarity over architectural perfection.

### II. Prototype-First Execution
This is a workshop prototype. Working software beats polished software.
Prefer simple, readable implementation choices that can be built and demonstrated within workshop constraints.
Avoid adding non-essential complexity (advanced infrastructure, premature abstractions, speculative features).

### III. User and UX Commitments
UI language MUST be Norwegian (Bokmal).
Core flows MUST remain understandable without training: input maintenance constraints, compare scenarios, see recommendation.
Each delivered feature should function on mobile layouts and desktop layouts, even if desktop is primary usage.

### IV. Data and Architecture Constraints
Primary stack is React + TypeScript + Tailwind CSS + Vite in frontend and FastAPI + SQLite in backend.
API endpoints MUST live under `/api/` and frontend `/api/*` calls MUST go through Vite proxy to backend.
For workshop v0.1, business logic may run client-side and use mock or user-entered data where appropriate.
No external infrastructure dependencies are required for core workshop functionality.

### V. Scope and Quality Boundaries
No authentication is required in v0.1.
No tests are required in v0.1, but code must remain clear, modular, and easy to verify manually.
Calculation logic for estimated revenue loss is business-critical and MUST remain explicit and deterministic.

## Technical Guardrails

- Frontend: React 18+ with TypeScript, Tailwind utility classes, Recharts for visualizations when charting is needed.
- Backend: Python 3.10+ with FastAPI.
- Database: SQLite (file-based).
- Data model for workshop: maintenance task inputs, flexibility window, scenario comparison values, recommendation result.
- Internationalization scope: Norwegian only for this workshop version.

## Delivery Workflow

1. Start each feature by describing user outcome and acceptance conditions in plain language.
2. Keep changes small and demonstrable: input flow -> calculation flow -> recommendation display.
3. Validate manually after each meaningful change (UI rendering, API route behavior, scenario math).
4. Prefer fixing issues in specs/requirements and assumptions before adding workaround code.

## Governance

This constitution overrides conflicting local conventions for this repository.
Changes to this constitution must be intentional, documented, and approved by workshop owners.
All future specs, plans, and implementation tasks must remain traceable to this constitution and the product vision.

**Version**: 1.0.0 | **Ratified**: 2026-04-21 | **Last Amended**: 2026-04-21
