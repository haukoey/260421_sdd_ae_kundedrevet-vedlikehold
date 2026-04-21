# ARGS Wizard Session Summary — Markedsdrevet Vedlikehold
## Feature 1: Drift Maintenance Stop Registration

**Date**: 2026-04-21
**Session type**: ARGS Wizard — Feature 1 of a multi-feature product
**Product**: Markedsdrevet Vedlikehold (Akershus Energi)
**Output**: `speckit-args-vedlikeholdsstopp-registrering.md`

---

## 1. Session Input Documents

Four documents were provided across the session:

### 1a. Product Vision Document — `product_vision.md`
- **Author**: Jørn Haukøy
- **Content**: Initial product framing for "Markedsdrevet Vedlikehold." Described the product as a single-user React SPA calculator where a driftsleder enters maintenance parameters, the tool calculates three alternative stop windows with estimated revenue loss, and a recommendation engine highlights the optimal window.
- **Critical assessment**: This document turned out to be an early-stage exploratory framing — aspirational and single-actor. It did not reflect the actual workflow the team had subsequently designed. The two-actor handoff structure, the Energihandel role, the request-response model, the status lifecycle, and the Microsoft Fabric data layer were all absent. The field set described (duration, earliest/latest start, price assumptions) was superseded by the richer form design in Disc 1/Disc 3.
- **Useful elements retained**: Business context (AE, hydropower, spot market exposure, timing of downtime = revenue loss), target user (driftsleder), value proposition framing, success criteria (under 2 minutes, immediately understandable recommendation), roadmap items (price API integration, multi-plant dashboard, ISY export).

### 1b. Screen Design Document — `Disc3-speckit.pdf`
- **Content**: Seven annotated screens (Steg 1–7) showing the full end-to-end workflow from Drift's initial form submission through to joint confirmation. Each screen showed the UI alongside a process diagram with three swim lanes: Drift, Energihandel, Datalagring.
- **Key revelation**: The product is a two-actor, structured digital handoff process — not a single-user calculator. The "calculations" are done by Energihandel using an analysis tool (Steg 4), not by the form itself. This fundamentally changed the feature scoping.
- **Specific facts extracted**:
  - **Steg 1 (Drift form)**: Field groups confirmed — Anlegg og Aggregat (stasjon selector + aggregat selector), Stoppinformasjon (ønsket startperiode, estimert varighet, type vedlikehold, kan flyttes?), Begrensninger og Kritikalitet (kritikalitet, avhengighet, kommentar valgfritt). Submit button: "Send til Energihandel".
  - **Steg 2 (Drift confirmation)**: Shows request ID, stasjon/aggregat, ønsket periode, varighet, status "Ny". Also shows "Dine henvendelser" — a personal list of the user's own requests with statuses ("Ny", "Under behandling") and type/duration.
  - **Steg 3 (ENH inbox)**: Energihandel sees all incoming requests with status badges (Ny, Pågår), and a notification banner for new arrivals. Shows request metadata: who sent it, how long ago, flexibility, criticality.
  - **Steg 4 (ENH analysis tool)**: Shows full request details plus prisprognose chart (timeline with "Optimal" marker), vannføringsprognose chart, and nøkkeltall (gjsn. prognose-pris, estimert tap, vannføring status, fyllingsgrad). CTA: "Gå videre — velg og send forslag".
  - **Steg 5 (ENH proposal)**: ENH selects 1–3 stop windows to propose, each showing proposed dates, estimert tap (MNOK), and brief rationale. Plus a free-text kommentar from ENH. CTA: "Send forslag til Drift".
  - **Steg 6 (Drift selection)**: Drift sees the proposals, one marked "Anbefalt", each with estimated loss and rationale. Drift selects one and optionally adds a kommentar (e.g. reason for deviating from recommendation). CTA: "Godkjenn og lagre beslutning".
  - **Steg 7 (Joint confirmation)**: Both parties see a final summary screen — request ID, stasjon/aggregat, valgt periode, estimert tap, status "Besluttet", valgt av (user + date). "Neste steg" note: decision can be fed into ISY. ENH monitors and can update if market changes.
  - **Navigation bar**: 7 tabs across the top: 1. Drift: Ny henvendelse / 2. Drift: Bekreftelse / 3. ENH: Innboks / 4. ENH: Analyseverktøy / 5. ENH: Send forslag / 6. Drift: Velg stopp / 7. Beslutning lagret.

### 1c. Workshop Documentation — `Disc3_-_streamlit_pbi__1_.pdf`
- **Content**: Multi-section discovery document covering: Disc 1 (information needs mapping between Drift and Energihandel), Disc 2 (data sources and desired dashboard presentation), Disc 3 (technical tool evaluation: React vs Power Platform vs Streamlit), Disc 4 (full process diagram and repeated screen walkthroughs).
- **Key facts extracted**:

  **Disc 1 — Drift → ENH information needs (the form fields, confirmed):**
  | Field | Description | ENH usage |
  |-------|-------------|-----------|
  | Navn på innmelder | Who initiates the request | Contact / accountability |
  | Stasjon og aggregat | Plant and unit (incl. MW capacity) | Calculate lost production, spot market bid |
  | Forventet varighet | Days/hours/weeks | Price and production forecast basis |
  | Stoppvindu (start/slutt) | Desired or planned window | ENH proposes final timing |
  | Hva stoppet gjelder | Short description (frost, wear, inspection, etc.) | Assess flexibility |
  | Fleksibilitet på stopp | Ikke flyttbar / Kan flyttes / Høy fleksibilitet | Market-optimal stop planning |
  | Kommentarer og endringer | Free text, changes in situation or duration | Practical coordination and follow-up |

  **Disc 1 — ENH → Drift information needs (what ENH sends back):**
  | Data | Description | Drift usage |
  |------|-------------|-------------|
  | 3 foreslåtte stopp-tidspunkt | Based on price forecast, water flow data, system load | Plan and confirm actual timing |
  | Vannføringsprognose | Expected water flow during planned period | Assessment basis |
  | Prisvurdering / produksjonsverdi | Estimate of how "costly" the stop is in the market | Helps Drift choose best timing |
  | Prioritering av stopp | Ranked from best to next-best alternative | Makes planning more holistic |

  **Disc 2 — Data sources (for ENH analysis tool — future features):**
  - Vannføringsprognoser: GLB (m³/s; hourly and daily forecasts; basis for MW/MWh)
  - Prisprognoser: Volue (NO1; time resolution: hour/week/month/year)
  - Variables: MW (capacity) and MWh (energy); aggregated per plant/group
  - Slukeevne per aggregat: fixed variable (max, min m³/s)
  - Dashboard needs: status per plant, production basis for spot, alerts, time filter, calendar visualization, status tracking of open requests

  **Disc 3 — Technology evaluation:**
  - Three alternatives assessed: React/Next.js (Alt B), Power Platform + Power BI (Alt A), Hybrid Power Platform + Streamlit (Alt C)
  - Alt B (React) rejected: no internal developer capacity to build or maintain
  - Alt C (Streamlit) rejected: no Python competence in ENH team, requires external hosting, slower time-to-MVP
  - **Recommended: Alt A — Power Platform + Power BI**
    - Power Apps canvas app handles Drift form and ENH proposal screens
    - Power Automate handles persistence (Fabric Lakehouse) and notifications (Teams/email via Graph API)
    - Power BI (embedded in Power Apps) handles ENH analysis dashboard
    - ENH can own and iterate the BI reports without developer involvement
    - Existing licenses (pending Power BI Premium verification for embedding)
    - Fastest time-to-MVP: 4–6 weeks

  **Disc 3 — Full functional requirements (F-01 through F-17):**
  - F-01/F-02: Standardized form with validation (Drift)
  - F-03/F-04: Auto-save to Fabric with ID + timestamp + status "Ny"; confirmation to Drift
  - F-05/F-06: Auto-notify ENH with summary + link (Teams and/or email)
  - F-07: Shared overview of all open requests for both parties (with status)
  - F-08/F-09/F-10: ENH analysis tool showing price forecast, water flow forecast, production forecast, capacity; scenario comparison; ENH can comment/justify choices
  - F-11/F-12/F-13: ENH selects 2–4 alternative windows with rationale and risk; saved to Fabric with status "Alternativer sendt"
  - F-14: Auto-notify Drift when ENH has sent proposals
  - F-15/F-16/F-17: Drift sees proposals, selects one, optionally adds comment; saved with timestamp + user; status → "Godkjent/Planlagt"; both parties notified

  **Disc 3 — Data model (three core entities in Fabric):**
  1. **Vedlikeholdsforespørsel**: Fields from Disc 1 + forespørsel-ID, timestamp, status, submitting user
  2. **Tidspunktsalternativer**: Linked to forespørsel via ID; fields: alternativ-ID, rangering, foreslått start/slutt, begrunnelse, risikovurdering, behandler, registreringstidspunkt
  3. **Endelig valg**: Linked to forespørsel and selected alternative; fields: valgt alternativ-ID, beslutningstaker, valgtidspunkt, kommentar, status

  **Disc 4 — Full process diagram (canonical reference):**
  9-step process across three swimlanes (Drift, Energihandel, Datalagring):
  1. Drift: Identifiserer behov (anlegg/komponent)
  2. Drift: Registrerer forespørsel (standardisert skjema) → Forespørsel lagret (ID, tidsstempel, status)
  3. Drift: Mottar bekreftelse (Status: Ny)
  4. ENH: Varsles om forespørsel (sammendrag + detaljer)
  5. ENH: Vurderer tidspunkt (priser, kapasitet, historikk, regulering)
  6. ENH: Velger 3 alternativer (begrunnelse, risiko) → Alternativer lagret (3 × tidspunkt, begrunnelse)
  7. Drift: Mottar alternativer (varsel med 3 forslag)
  8. Drift: Velger tidspunkt (ett av tre alternativer) → Valg lagret (valg, bruker, status)
  9. Drift + ENH: Bekreftet — planlagt (begge parter varsles)

---

## 2. Product Context (Confirmed)

**Product**: Markedsdrevet Vedlikehold
**Organisation**: Akershus Energi (AE) — ~99 employees, publicly owned by Akershus county, portfolio of hydropower, wind, district heating, solar, hydrogen, battery
**Market context**: Norwegian/Nordic spot market. Revenue directly exposed to price volatility. Timing of production downtime has direct P&L consequences. Hydropower can store water and shift production, but maintenance requires planned downtime — ideally during lowest-price periods.
**Core problem**: Drift and Energihandel operate in separate silos coordinated by phone and Excel. Maintenance decisions are made on routine/experience rather than systematic analysis of price outlook — causing unnecessary revenue loss.
**Target users**:
- **Primary**: Driftsleder at AE's hydropower plants — plans and coordinates maintenance tasks, balances technical requirements against production targets. JTBD: "Find the maintenance timing that is technically sound and minimizes revenue loss — without stitching data from five different places."
- **Secondary**: Energihandel/traders — currently involved ad-hoc, need predictability about planned downtime for portfolio strategy optimization.
**Current alternatives**: Excel spreadsheets with own price estimates + manual coordination with Energihandel. Maintenance plans and market data live in separate silos with no systematic link.
**Product status**: New product, not yet built. This is the first feature specification session.
**Feature category**: First-in-category for the entire product. All patterns, entities, and UI conventions established here are the foundation for subsequent features.

---

## 3. Feature Scoping Decision

The session started with the product vision document describing a single-user calculator. After reviewing the screen designs (Disc 3 Steg 1–7), the scope was fundamentally reframed.

The product has **seven screens** across a two-actor workflow. These were decomposed into at least three separate ARGS/spec cycles:

| Feature | Screens | Actor(s) | Status |
|---------|---------|----------|--------|
| **Feature 1: Drift registration form + confirmation** | Steg 1–2 | Drift only | ✅ ARGS complete |
| **Feature 2: ENH inbox + analysis tool + proposal creation** | Steg 3–5 | ENH only | Pending |
| **Feature 3: Drift proposal selection + joint confirmation** | Steg 6–7 | Drift + ENH | Pending |

Additional candidate features identified (not yet scoped):
- F-07: Shared request overview (both parties — may belong to Feature 1 or be standalone)
- Notification system (Teams/email triggers — may be infrastructure, not a standalone spec)
- ISY integration (post-MVP, referenced in Steg 7 "Neste steg")
- Historical calibration, multi-plant dashboard, resource planning (post-MVP roadmap)

---

## 4. Key Domain Rules and Constraints Established

These are the facts the downstream AI could not guess and must have in the spec:

### Form structure
- Aggregat selection is **dependent on stasjon** — only aggregats belonging to the selected station are shown
- MW capacity is a **property of the aggregat** — not a manually entered field; the system knows it
- All fields except `kommentar` are required; form validates and blocks submission on missing required fields

### Fleksibilitet enum (three values, semantics matter for ENH analysis)
- `Ikke flyttbar`: Stop must happen now — technical or safety reason; ENH has no latitude
- `Kan flyttes`: Stop can be moved within the desired period
- `Høy fleksibilitet`: ENH has broad latitude to suggest the best market timing

### Type vedlikehold (predefined list, not free-text only)
Minimum values: standard revisjon, lekkasje, periodisk tilsyn, turbinbytte, + "annet" (free-text fallback)

### Status lifecycle (full — spans all features)
- `Ny`: Set automatically on submission. Cannot be set manually. Only Feature 1 creates this status.
- `Under behandling`: Set by ENH when they open/start analyzing a request (Feature 2)
- `Alternativer sendt`: Set by ENH when proposals are sent to Drift (Feature 2)
- `Besluttet` / `Godkjent / Planlagt`: Set when Drift confirms a window (Feature 3)

### Request history visibility rule
- The "Dine henvendelser" list on the confirmation screen shows **only the current driftsleder's own requests** — not requests from other driftsledere at other plants

### No editing after submission (v1 constraint, intentional)
- Once submitted, a request cannot be edited by Drift through the tool
- This is explicit v1 scope — do not work around it; if changes are needed Drift contacts ENH directly

### ENH proposal count
- ENH selects **2–4** alternatives (F-11), not always exactly 3 (though 3 is the illustrated default)

---

## 5. Key Entities (Data Model)

### Vedlikeholdsforespørsel (maintenance stop request)
Core entity created by Feature 1. Fields:
- `forespørsel_id` (system-generated, unique)
- `tidsstempel` (submission timestamp)
- `innmelder` (submitting user identity)
- `stasjon` (power station)
- `aggregat` (specific generating unit)
- `aggregat_mw` (capacity in MW — property of aggregat, not entered by user)
- `ønsket_startperiode` (desired start period — rough date, not precise timestamp)
- `estimert_varighet_dager` (estimated duration in days)
- `type_vedlikehold` (enum + optional free text)
- `fleksibilitet` (enum: ikke_flyttbar / kan_flyttes / høy_fleksibilitet)
- `kritikalitet` (enum: lav / middels / høy)
- `avhengighet` (e.g. "Ekstern leverandør", "Internt mannskap")
- `kommentar` (nullable free text)
- `status` (enum — see lifecycle above)

### Tidspunktsalternativer (proposed windows from ENH)
Created by Feature 2. Fields:
- `alternativ_id`
- `forespørsel_id` (FK)
- `rangering` (rank/priority among proposals)
- `foreslått_start`
- `foreslått_slutt`
- `estimert_tap_mnok`
- `begrunnelse` (free text rationale)
- `risikovurdering`
- `behandler` (ENH user who created the proposal)
- `registreringstidspunkt`

### Endelig valg (final decision)
Created by Feature 3. Fields:
- `valg_id`
- `forespørsel_id` (FK)
- `valgt_alternativ_id` (FK to Tidspunktsalternativer)
- `beslutningstaker` (Drift user who confirmed)
- `valgtidspunkt`
- `kommentar` (optional — reason for deviation from recommendation)
- `status`

---

## 6. Output

**File**: `speckit-args-vedlikeholdsstopp-registrering.md`
**Length**: ~680 words
**Covers**: Intent and business context, actor and motivation, complete form field set with groupings and semantics, aggregat→stasjon dependency, MW as aggregat property, confirmation screen behavior, personal request history list, status lifecycle (full, spanning all features), entity definition for Vedlikeholdsforespørsel, explicit scope exclusions, success criteria.

---

## 7. Technology Context (for Cursor harness — informational only, not in ARGS)

The recommended implementation platform is **Power Platform + Power BI (Alt A)**:
- **Drift form (Feature 1)**: Power Apps canvas app
- **Persistence and notifications**: Power Automate (writes to Fabric Lakehouse, notifies via Teams/Graph API)
- **ENH analysis tool (Feature 2)**: Power Apps with embedded Power BI report
- **Data store**: Microsoft Fabric Lakehouse (tables: maintenance_requests, time_alternatives, final_decisions, audit_log)
- **Audit logging**: Dedicated audit_log table; Automate writes on each status transition

A React/Next.js prototype is also under consideration for workshop/validation purposes. The ARGS is written tech-agnostically and is valid for either implementation path.

**Data sources for ENH analysis tool (Feature 2 context):**
- Vannføringsprognoser: GLB (m³/s; hourly/daily; delivered as Excel via email, digitized)
- Prisprognoser: Volue (NO1 price area; resolution: hour/day/week/month/year)
- Computed: MW capacity from water flow + slukeevne per aggregat (fixed min/max m³/s)

---

## 8. What Comes Next

**Next ARGS session — Feature 2: ENH Inbox + Analysis Tool + Proposal Creation**

This is the most complex feature in the product. Key unknowns to resolve in the next session:
- How does ENH navigate from inbox notification to analysis tool to proposal screen?
- What exactly does the prisprognose visualization show — and how does ENH use it to select windows?
- Can ENH save a partial analysis and return to it, or is the flow single-session?
- Is the "Optimal" marker on the price chart calculated by the system, or placed manually by ENH?
- How many alternatives must ENH provide — is 2 acceptable, or is 3 the minimum?
- Can ENH request more information from Drift before sending proposals? (Not visible in screens — is this out of scope?)
- What happens if ENH determines the stop should not go ahead? (Rejection path — is it in scope?)

**Harness prompt for Feature 2** (to run in Cursor before the next ARGS session):

> I'm about to specify Feature 2 of Markedsdrevet Vedlikehold — the ENH (Energihandel) inbox, analysis tool, and proposal creation screens. Before I start the interview, I need a few facts from the codebase:
>
> 1. Does a data model for `Vedlikeholdsforespørsel` already exist? If so, list the fields and their types.
> 2. Does a data model for `Tidspunktsalternativer` already exist? If so, list the fields and their types.
> 3. Are there any existing components, pages, or routes related to the ENH/Energihandel view? List them with a brief description.
> 4. Is there an existing pattern for how price forecast data (from Volue) or water flow data (from GLB) is fetched or displayed? Describe the pattern if it exists.
> 5. Are there any existing specs in `/specs/` related to this product? Summarize the most recent one.
