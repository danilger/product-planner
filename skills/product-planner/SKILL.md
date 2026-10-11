---
name: product-planner
description: >-
  Interactive product planning before implementation: clarify intent; discover
  product, technical, and UX/UI preferences against an optional planning/inbox;
  define MVP; decompose into capabilities and ordered OpenSpec-sized changes;
  then hand off the first change. Use when the user describes a product idea,
  asks for MVP scope, roadmap, domain discovery, tech/UX direction, or
  pre-coding product planning. Do not use for OpenSpec propose/apply, writing
  specs/tasks, or implementing code.
license: MIT
metadata:
  version: "1.2.0"
---

# Product Planner

Facilitate product discovery from a rough idea to an approved MVP and an **ordered queue of OpenSpec-sized changes**—enough context that a coding agent need not invent the next unit of work **or** invent stack, UI kit, or visual direction. Do not replace OpenSpec or write application code.

Speak and write planning artifacts in the user's language. Keep these instructions in English.

## Primacy rule: minimal depth

Choose the **shallowest** investigation that still supports decisions. Skip phases that add no decision value. Do not keep researching for documentary completeness. Prefer a short correct plan over a full checklist walkthrough.

This rule overrides habit: if the product is small, collapse phases, skip Domain and Capabilities, and avoid inventing changes or gates. **Technical and UX/UI discovery also scale**—a CLI may skip UX/UI entirely; a branded site may treat visual direction as mandatory before MVP.

## Completeness rule (not only features)

> Do not assume product discovery is complete merely because functional requirements are clear. Identify technical constraints, implementation preferences, UX/UI direction, and explicitly forbidden approaches when they materially affect the product.

Functional clarity alone is not enough for handoff when coding agents would otherwise choose framework, component library, or look-and-feel independently.

## Three levels of work

| Level | Who owns it | Purpose | Example |
| --- | --- | --- | --- |
| **Capability** | Product Planner (optional grouping) | Large product ability | Owner content |
| **Change** | Product Planner → OpenSpec change | Small shippable user-facing increment | Publish an article |
| **Tasks** | OpenSpec only | Tech steps inside one change | API, schema, UI, tests |

Product Planner defines **what**, **in what order**, and **cross-cutting constraints** (product + tech preferences + UX/UI direction). OpenSpec defines **how** and the task checklist for one change.

**Never stop at epic-sized “vertical slices”.** Four product areas named as slices without OpenSpec-sized changes is a failed decomposition.

## Boundary with OpenSpec

| Product Planner (constraints & intent) | OpenSpec (implementation of one change) |
| --- | --- |
| MVP, business rules, ordered changes | Components, API shapes, schema, tests for that change |
| Preferred/forbidden stack (e.g. Next.js, no SPA) | Folder layout, patterns, concrete component choices within constraints |
| Visual direction, theme, references | Pixel-level implementation of one feature |
| Integrations named (RouterAI, Resend) | How that change wires the integration |

Product Planner must **not** become an architecture committee: no folder trees, no pattern mandates (DDD/CQRS), no per-change component inventories. It **must** lock decisions that affect the whole product so agents do not diverge.

Example of planner-level constraints (intent, not implementation design):

```yaml
frontend:
  framework: Next.js
  ui: shadcn/ui
  styling: Tailwind CSS
design:
  direction: Minimalist editorial
  theme: Dark-first
  responsive: Required
constraints:
  - No standalone React SPA
  - Reuse existing authentication
```

## Three discovery directions (one skill)

| Direction | What we clarify | Default persist location |
| --- | --- | --- |
| **Product Discovery** | Who, problem, MVP capabilities, business rules | `planning/product.md` |
| **Technical Discovery** | Stack, hosting, DB, auth, integrations, **forbidden** choices | sections in `product.md` (or `planning/technical.md` if large) |
| **UX/UI Discovery** | Visual direction, references, nav, responsive, motion (if UI matters) | sections in `product.md` (or `planning/design.md` if large) |

These are **directions of questioning inside one skill**, not separate agents. Depth of each depends on the project.

### When to deepen each direction

| Direction | Deepen when… | Skip / shallow when… |
| --- | --- | --- |
| Product | Always (core) | — |
| Technical | Greenfield, stack choice open, migration, or conflicts with existing code | Stack already fixed in repo + `AGENTS.md` / architecture docs—**cite, don’t re-ask** except gaps and forbidden overrides |
| UX/UI | User-facing UI, brand/marketing site, design is a success criterion | CLI, API-only, internal script, or user says design is irrelevant |

If the user drops **references** (sites, screenshots), analyze them and ask fewer questions—do not run a 40-question design interrogation.

### Technical Discovery (preferences, not full architecture)

Ask for owner preferences and constraints, including **what is categorically unwanted**:

- Frontend framework / rendering model
- UI kit / component approach
- Styling approach
- Backend shape (same app vs separate API)
- Database (if relevant)
- Auth (reuse existing vs new)
- Hosting
- Integrations
- **Forbidden solutions** (e.g. “no standalone React SPA”)

Do not design schemas, route trees, or module layouts here.

### UX/UI Discovery (direction, not mockups)

When UI matters, clarify enough that agents share one visual intent:

- Desired impression / visual direction (minimal, editorial, tech, …)
- Light / dark / system
- Liked and disliked references (prefer analyzing provided ones)
- What the first viewport / home should emphasize
- Relative priority (e.g. portfolio vs articles vs lead capture)
- Motion importance
- Mobile-first vs desktop-first
- Whether a shared design system / component kit is required

Stop when direction is clear enough for OpenSpec not to invent a conflicting aesthetic.

## Hard boundaries

**Do**

- Ask before inventing requirements; label `Confirmed` / `Assumption` / `Open`
- Prefer user-valuable changes over horizontal tech layers
- Size each change for one autonomous agent cycle (see Decomposition)
- Reuse existing project docs and **existing code capabilities**; avoid parallel sources of truth
- **Cite** existing canons (`AGENTS.md`, `ARCHITECTURE.md`, `openspec/config.yaml`, design tokens) instead of copying them into planning docs
- Capture **forbidden** tech/UX choices, not only preferred ones
- Stop after OpenSpec handoff unless the user explicitly asks to continue elsewhere

**Do not**

- Create `openspec/changes/*`, delta specs, `tasks.md`, or run `/opsx:*` (except suggesting the user run `/opsx:propose`)
- Implement features, scaffolds, or refactors without explicit permission
- Break a change into OpenSpec-style task lists (API / DB / UI / tests as separate plan items)
- Mandate DDD, CQRS, microservices, Event Storming, or heavy architecture
- Fill template sections with guessed content
- Auto-create `planning/technical.md` / `planning/design.md` for every project
- Duplicate BMAD agents, PRFAQ, market research packs, or sprint/Jira machinery

## Progress checklist

Track lightly (chat is enough). Skip items that do not apply:

```
- [ ] 0 Bootstrap
- [ ] 1 Intent
- [ ] 2 Product Discovery
- [ ] 3 Technical Discovery   ← adaptive; cite existing stack when present
- [ ] 4 UX/UI Discovery       ← skip if no meaningful UI
- [ ] 5 Domain lite           ← skip if not needed
- [ ] 6 Discovery Coverage    ← internal check before MVP
- [ ] 7 Scope / MVP           ← mandatory approval
- [ ] 8 Decomposition         ← capabilities (optional) + OpenSpec-sized changes
- [ ] 9 Roadmap               ← ordered change queue
- [ ] 10 Review + persist     ← mandatory approval
- [ ] 11 OpenSpec handoff     ← first pending change only
```

### Mandatory gates (only two)

1. **MVP boundary** (end of Scope / MVP) — user agrees what is in / out / later.
2. **Final plan** (Review) — user explicitly approves; then persist artifacts.

Complexity, capabilities, tech/UX lists, and change lists are discussed as needed—not separate formal confirmation rounds for every direction.

## Discovery Coverage (internal check)

Before moving to **Scope / MVP**, verify coverage. Do **not** paste this table every turn—use it as an internal completeness control.

| Area | Status |
| --- | --- |
| Product intent | Confirmed / Assumption / Open |
| MVP requirements (enough to draft boundary) | … |
| Technical direction | … |
| UX/UI direction | … (N/A if skipped) |
| Existing implementation | Inspected |
| Constraints / forbidden choices | … |

Rules:

- If an area **materially affects** the product and is still Open → ask, or explicitly agree to defer that decision to OpenSpec (and record the deferral).
- Example: unset database may be OK to defer; unset visual direction for a portfolio site is **not** OK to leave to random agent taste.
- Existing repo: mark Technical from inspection + cite paths; only question gaps and conflicts.

**Do not enter MVP / Decomposition until** Product is solid enough to draft in/out, and every *material* Technical / UX/UI gap is Confirmed, explicitly Assumed, deferred with owner OK, or N/A.

## Phase 0: Bootstrap

On activation:

1. Scan the target project for existing product docs (`planning/`, README, specs, ADRs), **canonical tech/agent rules** (`AGENTS.md`, `ARCHITECTURE.md`, `openspec/config.yaml`), **and reusable implementation** (auth, CRUD patterns, shared UI, design system, integrations already wired). Summarize what already exists; extend it instead of forking a second canon.
2. Mention that materials are optional:

> If you have briefs, notes, screenshots, or old requirements, drop them into `planning/inbox/` (say "inbox ready" when done). Otherwise we start discovery from your idea.

3. **Do not** create `planning/inbox/` or other planning files up front. Create `planning/inbox/` only when the user provides materials, asks to use inbox, or says they will drop files there.
4. If `planning/inbox/` already exists and is non-empty: read it, draft/update `planning/context.md` (index only), short finding summary.
5. Proceed to Intent immediately; materials can arrive later.

### `planning/context.md` (index, not a PRD)

Create or refresh only when inbox is in use (non-empty or user said inbox is ready):

```markdown
# Context index

## Sources
- `inbox/<file>` — one-line gist

## Useful for MVP
- …

## Conflicts / stale
- …

## Gaps
- …
```

Do not rewrite inbox originals. Cite paths and conclusions—no large verbatim dumps.

## Context grounding

Always distinguish `Confirmed` / `Assumption` / `Open`.

**Inbox source tags** (`Supported by inbox` / `Conflicts with inbox` / `Not in inbox`) — use **only when inbox materials are in play**. If there is no inbox content, do not tag every answer `Not in inbox`.

When inbox is active (through MVP and on revisit):

1. Before questions, align open topics with inbox / `context.md`.
2. After answers, use source tags where they clarify evidence; always surface `Conflicts with inbox: <path>` and ask which source wins—never silent-merge.
3. Prefer recommendations that cite concrete files.
4. Unclear fragments → `Assumption` or a question; do not invent business rules.
5. New files mid-session → re-read, update index, note impact on agreed points.

### Inbox budget

- Many files: index by name/title first; deep-read only what matters now
- Images: capture relevant UI/flows/tables in the index; cite paths later
- Ask once if inbox is authoritative truth or archive
- Extra material must not inflate MVP—park as Out of scope / Later

## Questioning protocol

- Usually **3–5 questions per turn**, then a short summary (Confirmed / Assumption / Open; add inbox tags only if inbox is active)
- Mix directions as needed in one turn (product + tech + UX), but do not overwhelm—prioritize what blocks MVP
- Hunt contradictions, risks, and unknown unknowns
- One follow-up on a vague answer, then park as `Assumption` if still unclear
- User may say "enough" / "write the plan" to skip remaining grill—still run Discovery Coverage and surface any material Opens before MVP

## Complexity ladder

After Intent + early Product Discovery, **state** the working level (no separate approval gate). Adjust if the user corrects you.

| Level | Signals | Path |
| --- | --- | --- |
| Simple | One actor, CRUD/util, few rules | Skip Domain and Capabilities; few changes, maybe in `product.md` only; light Technical; UX/UI only if UI exists |
| Standard | Several scenarios, light integrations | Capabilities if helpful; ordered changes in `roadmap.md`; Technical + UX/UI as material |
| Complex | Multiple roles, stateful processes, money/approvals, contested rules | Domain lite; full coverage check; capabilities + change queue |

## Phases

### 1 Intent

Capture: problem, who hurts, desired outcome. Strip stack noise from the *problem statement*, but note stack hints for Technical Discovery. Use inbox if present.

**Exit:** draft understanding of problem, audience, outcome (Opens allowed).

### 2 Product Discovery

Clarify scenarios, success signals, risks, business constraints. Resolve or park contradictions with inbox when active.

**Exit:** no critical unresolved product contradictions; Opens do not block drafting an MVP boundary; working complexity level set.

### 3 Technical Discovery

Discover preferences, constraints, and **forbidden** approaches. Prefer inspection of the existing project over re-asking.

- If stack is already canon → summarize + cite; ask only about gaps, migrations, or owner overrides.
- If greenfield or ambiguous → short preference questions (framework, UI kit, styling, backend shape, DB, hosting, integrations, forbidden).
- Record integrations that affect many changes (email, LLM, payments) at planner level.

**Exit:** material technical direction Confirmed, Assumed, explicitly deferred, or cited from existing docs. No folder/API design.

### 4 UX/UI Discovery (conditional)

Skip when there is no meaningful end-user UI, or owner marks design out of scope.

Otherwise clarify visual direction, theme, references (analyze if provided), information priority on key surfaces, responsive expectations, motion—only as deep as needed so agents share one look-and-feel.

**Exit:** material UX/UI direction Confirmed / Assumed / N/A. Not wireframes or component inventories.

### 5 Domain lite (conditional)

Triggers: multiple roles with different powers, long-running processes, status machines, money/compliance, contested rules.

Collect only what affects MVP: actors, key events/processes, business rules, important entities. Lightweight Event Storming ideas are allowed; do not force a full storm or `.dio` ritual.

**Exit:** MVP-relevant domain facts agreed or assumed. Skip for Simple.

### 6 Discovery Coverage → then Scope / MVP — gate 1

Run the Discovery Coverage check. Close or explicitly defer material Opens.

Then define MVP in, explicit out, and Later. MVP scope is primarily product capabilities; Technical / UX/UI directions constrain *how* the MVP will be built but are not a substitute for in/out features. Mark assumptions.

**Exit:** user agrees on MVP boundary (mandatory). Material tech/UX constraints recorded (in chat until persist).

### 7 Decomposition

Two steps. Skip step A for Simple products when the MVP is already one or two changes.

**A. Capabilities (optional)** — name large product abilities for orientation (e.g. Content, Leads). Labels only—**not** OpenSpec units and **not** a substitute for changes. Do **not** use capability as the primary ordering structure later.

**B. OpenSpec-sized changes** — split the MVP into **minimal meaningful changes**. Each change must:

- Deliver a **verifiable user-facing result** (or a necessary end-to-end path to one)
- Have clear in/out boundaries
- Be small enough for **one autonomous agent cycle**: prefer changes that can be implemented and verified independently, with limited context, without requiring simultaneous edits across unrelated subsystems. **Split** when scope or acceptance becomes hard to reason about. No artificial quotas (LOC, hours).
- Be usable without implementing the entire capability first
- **Not** list technical tasks (those belong in OpenSpec)
- **Reuse what exists** — before adding a change, check the codebase/docs for existing capabilities (auth, models, admin shells, email, i18n, etc.). Do not plan re-implementation unless modification is required; if reuse is assumed, state it under Scope (e.g. “uses existing auth”)
- **Respect Technical / UX/UI direction** — do not invent a change that implies a forbidden stack or a second design system

| Scale | Changes |
| --- | --- |
| Small | One change can be the whole MVP |
| Medium | Several independent shippable changes |
| Complex | As many as needed—no quota; merge if two changes are tiny and always ship together |

**Anti-patterns (reject these):**

- Epic slices: “Owner content”, “Leads”, “Comments” with no further split
- Fake verticals / horizontal layers with nice names: “public pages with mock data” → “then storage” → “then admin CRUD” when a thin end-to-end path (create → publish → view) would prove value sooner
- Task-shaped items: “add API”, “add DB schema”, “add UI”
- Re-planning existing features (e.g. auth already works) as greenfield changes
- Stack-migration epics disguised as product changes unless the owner explicitly wants a platform change in MVP

Prefer an early thin vertical (e.g. owner publishes one article and a visitor opens it) over layering UI, then DB, then admin.

Discuss the list; final buy-in is at Review.

### 8 Roadmap

Build a single **`## Ordered Changes`** queue. Cross-capability interleaving is normal (e.g. thin content publish → leads form → richer content admin). **Order is defined by this list and Dependencies, not by Capability sections.**

For each change record:

| Field | Meaning |
| --- | --- |
| ID | Stable id, e.g. `CH-001` |
| Slug | Future OpenSpec change name (`kebab-case`) |
| Capability | Optional label (Content, Leads, …)—orientation only |
| Goal | User-facing outcome |
| Scope | In / out for this change only; note reused existing features |
| Acceptance | How to verify done |
| Dependencies | Prior change IDs, or `none` |
| Status | `pending` at plan time |

Call out top risks/assumptions for the queue (include tech/UX risks if material). Single-change MVPs may live entirely in `product.md`; otherwise use `roadmap.md`.

**Status field:** keep it as a light queue marker (`pending` / later `done` when the user asks to refresh the plan). Do **not** turn `roadmap.md` into a second issue tracker if the project already tracks work elsewhere (e.g. Multica)—statuses stay thin; detailed tracking stays in one place.

### 9 Review — gate 2

Present: MVP, **Technical / UX/UI direction** (or citations to existing canon), capabilities (if any), **full ordered change queue**, risks. Apply feedback. Require explicit approval ("утверждаю" / "approve" / equivalent).

**On approval — persist** `product.md` (including Technical / UX/UI sections when applicable), and `roadmap.md` / `domain.md` / optional `technical.md` / `design.md` when needed. `planning/context.md` may be created or updated before approval when inbox is in use. All other planning artifacts are written after final approval unless the user explicitly requests an earlier draft.

### 10 OpenSpec handoff

Do **not** invent a new shortlist of 1–3 slugs. The queue already exists in the roadmap.

1. Point to the **first pending** change (lowest order with `Status: pending`).
2. Restate its Goal, Scope, Acceptance, Dependencies.
3. Remind that implementation must follow `planning/product.md` Technical / UX/UI direction (and cited project canons).
4. Suggest the user run `/opsx:propose` (or their OpenSpec flow) for that slug.
5. Stop. Do not create OpenSpec artifacts or code.

## Readiness bar

Before handoff, all must be yes:

1. Who and what pain?
2. MVP in / out clear?
3. Material **Technical** direction known, cited, or explicitly deferred?
4. Material **UX/UI** direction known, N/A, or explicitly deferred?
5. Forbidden / constraint list clear enough to prevent stack or aesthetic drift?
6. Changes are OpenSpec-sized (not epics), ordered, with acceptance?
7. Dangerous assumptions named?
8. Enough that the next agent need not invent scope **or** foundational stack/UI direction for change 1?

If no → return to the relevant Discovery direction, Domain, or Decomposition.

## Artifacts (target product project)

| File | When |
| --- | --- |
| `planning/context.md` | When inbox is in use — **may write/update before approval** |
| `planning/product.md` | After final approval (always on persist); includes Technical / UX/UI sections by default |
| `planning/roadmap.md` | After final approval when more than one change, or when the queue needs a dedicated file |
| `planning/domain.md` | After final approval, if Complex / non-trivial domain |
| `planning/technical.md` | Optional — only when Technical Direction outgrows `product.md` |
| `planning/design.md` | Optional — only when UX/UI Direction outgrows `product.md` |

**Do not** create `technical.md` / `design.md` automatically. Prefer sections inside `product.md` for small/medium products. When a separate file exists, `product.md` keeps a short summary + link.

Tiny products: single `planning/product.md` embedding the one (or few) changes.

If `AGENTS.md` / architecture docs already define the stack, Technical Direction should **reference** those paths and list only planner-specific additions (integrations, forbidden overrides, open decisions)—do not duplicate the canon.

### `product.md` template

```markdown
# Product

## Intent
- Goal:
- Problem:
- For whom:
- Success looks like:

## MVP
- In:
- Out:
- Later:

## Capabilities
- (optional; omit for tiny products)

## Technical Direction
### Required
- …

### Forbidden
- …

### Existing (cite, do not duplicate)
- e.g. AGENTS.md, existing auth module

### Integrations
- …

### Open decisions
- … (explicitly deferred to OpenSpec if owner agreed)

## UX/UI Direction
### Visual identity
- …

### Theme / responsive
- …

### References
- … (paths or URLs)

### UX constraints
- …

### N/A
- (if no meaningful UI—state why)

## Requirements & constraints
- Confirmed:
- Assumptions:
- Open:

## Sources
- Key inbox / docs (paths):

## OpenSpec handoff
- First pending change: `CH-001` / `kebab-slug` — see planning/roadmap.md (or Changes below)
```

Omit Technical or UX/UI subsections only when truly N/A; prefer an explicit **N/A** note over silence when UI was skipped.

### `roadmap.md` template

```markdown
# Roadmap

## Risks & assumptions
- …

## Ordered Changes

### CH-001: <Short title>
Status: pending
OpenSpec: kebab-slug
Capability: Content   <!-- optional -->

Goal:
…

Scope:
- In: …
- Out: …
- Reuses: existing auth (example)

Acceptance:
- …

Dependencies: none

### CH-002: …
Status: pending
OpenSpec: …
Capability: Leads

Goal:
…

Scope:
- In: …
- Out: …

Acceptance:
- …

Dependencies: CH-001
```

### `domain.md` template

```markdown
# Domain

## Actors
## Key events / processes
## Business rules
## Entities
```

## OpenSpec handoff format

```markdown
## Handoff to OpenSpec

Planning source of truth:
- planning/product.md   # includes Technical + UX/UI Direction
- planning/roadmap.md (if any)
- planning/domain.md (if any)
- planning/technical.md / planning/design.md (if any)
- planning/context.md (if any)
- plus cited project canons (AGENTS.md, etc.)

First pending change:
- ID: CH-00N
- OpenSpec slug: kebab-slug
- Goal: …
- Scope: …
- Acceptance: …
- Dependencies: …

Constraints reminder: follow Technical / UX/UI Direction in planning/product.md (and cited canons).

Next step for you: `/opsx:propose` (or equivalent) for `kebab-slug`.
Product Planner stops here.
```

## Gotchas

- Minimal depth first—skip empty ritual; still run Discovery Coverage for material gaps
- Functional MVP clarity ≠ discovery complete—tech and UX/UI may still be open
- Decomposition fails if you only name capabilities/epics—always produce OpenSpec-sized changes
- Prefer thin end-to-end value over UI-then-DB-then-admin layering
- Order changes globally; Capability is a field, not a section that reorders work
- Do not re-plan features that already exist unless they need modification
- Cite `AGENTS.md` / architecture docs; do not paste them into `product.md`
- Do not auto-create `technical.md` / `design.md`
- Ask what is **forbidden**, not only what is preferred
- Analyze design references when provided instead of endless questionnaires
- Questions before plan docs; persist product/roadmap/domain after final approval; `context.md` may land earlier when inbox is active
- Do not auto-create empty `planning/inbox/`
- Inbox tags only when materials exist
- Only two mandatory gates: MVP and final plan
- Do not mandate Event Storming—only when Domain lite triggers fire
- Never silent-merge inbox vs user conflicts
- Do not propose microservices/CQRS/DDD without a concrete risk and user interest
- Do not start coding or create OpenSpec folders after handoff
- Do not grow roadmap into a second Jira/Multica
- Prefer fewer sharper changes over a long fake-granular queue
- Do not design folder structure or per-change component lists in Product Planner
