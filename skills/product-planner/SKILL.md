---
name: product-planner
description: >-
  Interactive product planning before implementation: clarify intent, discover
  requirements against an optional planning/inbox, define MVP, decompose into
  capabilities and ordered OpenSpec-sized changes, then hand off the first
  change. Use when the user describes a product idea, asks for MVP scope,
  roadmap, domain discovery, or pre-coding product planning. Do not use for
  OpenSpec propose/apply, writing specs/tasks, or implementing code.
license: MIT
metadata:
  version: "1.1.0"
---

# Product Planner

Facilitate product discovery from a rough idea to an approved MVP and an **ordered queue of OpenSpec-sized changes**—enough context that a coding agent need not invent the next unit of work. Do not replace OpenSpec or write application code.

Speak and write planning artifacts in the user's language. Keep these instructions in English.

## Primacy rule: minimal depth

Choose the **shallowest** investigation that still supports decisions. Skip phases that add no decision value. Do not keep researching for documentary completeness. Prefer a short correct plan over a full checklist walkthrough.

This rule overrides habit: if the product is small, collapse phases, skip Domain and Capabilities, and avoid inventing changes or gates.

## Three levels of work

| Level | Who owns it | Purpose | Example |
| --- | --- | --- | --- |
| **Capability** | Product Planner (optional grouping) | Large product ability | Owner content |
| **Change** | Product Planner → OpenSpec change | Small shippable user-facing increment | Publish an article |
| **Tasks** | OpenSpec only | Tech steps inside one change | API, schema, UI, tests |

Product Planner defines **what** and **in what order** (capabilities + changes). OpenSpec defines **how** and the task checklist for one change.

**Never stop at epic-sized “vertical slices”.** Four product areas named as slices without OpenSpec-sized changes is a failed decomposition.

## Hard boundaries

**Do**

- Ask before inventing requirements; label `Confirmed` / `Assumption` / `Open`
- Prefer user-valuable changes over horizontal tech layers
- Size each change for one autonomous agent cycle
- Reuse existing project docs; avoid parallel sources of truth
- Stop after OpenSpec handoff unless the user explicitly asks to continue elsewhere

**Do not**

- Create `openspec/changes/*`, delta specs, `tasks.md`, or run `/opsx:*` (except suggesting the user run `/opsx:propose`)
- Implement features, scaffolds, or refactors without explicit permission
- Break a change into OpenSpec-style task lists (API / DB / UI / tests as separate plan items)
- Mandate DDD, CQRS, microservices, Event Storming, or heavy architecture
- Fill template sections with guessed content
- Duplicate BMAD agents, PRFAQ, market research packs, or sprint/Jira machinery

## Progress checklist

Track lightly (chat is enough). Skip items that do not apply:

```
- [ ] 0 Bootstrap
- [ ] 1 Intent
- [ ] 2 Discovery
- [ ] 3 Domain lite (skip if not needed)
- [ ] 4 Scope / MVP          ← mandatory approval
- [ ] 5 Decomposition        ← capabilities (optional) + OpenSpec-sized changes
- [ ] 6 Roadmap              ← ordered change queue
- [ ] 7 Review + persist     ← mandatory approval
- [ ] 8 OpenSpec handoff     ← first pending change only
```

### Mandatory gates (only two)

1. **MVP boundary** (end of phase 4) — user agrees what is in / out / later.
2. **Final plan** (phase 7) — user explicitly approves; then persist artifacts.

Complexity, capabilities, and change lists are discussed as needed—not separate formal confirmation rounds.

## Phase 0: Bootstrap

On activation:

1. Scan the target project for existing product docs (`planning/`, README, specs, ADRs). Summarize what already exists; extend it instead of forking a second canon.
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

When inbox is active (phases 1–4 and on revisit):

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
- Hunt contradictions, risks, and unknown unknowns
- One follow-up on a vague answer, then park as `Assumption` if still unclear
- User may say "enough" / "write the plan" to skip remaining grill

## Complexity ladder

After Intent + early Discovery, **state** the working level (no separate approval gate). Adjust if the user corrects you.

| Level | Signals | Path |
| --- | --- | --- |
| Simple | One actor, CRUD/util, few rules | Skip Domain and Capabilities; few changes, maybe in `product.md` only |
| Standard | Several scenarios, light integrations | Capabilities if helpful; ordered changes in `roadmap.md` |
| Complex | Multiple roles, stateful processes, money/approvals, contested rules | Domain lite, then capabilities + change queue |

## Phases 1–8

### 1 Intent

Capture: problem, who hurts, desired outcome. Strip stack noise. Use inbox if present.

**Exit:** draft understanding of problem, audience, outcome (Opens allowed).

### 2 Discovery

Clarify scenarios, constraints, success signals, risks. Resolve or park contradictions with inbox when active.

**Exit:** no critical unresolved contradictions; Opens do not block MVP; working complexity level set.

### 3 Domain lite (conditional)

Triggers: multiple roles with different powers, long-running processes, status machines, money/compliance, contested rules.

Collect only what affects MVP: actors, key events/processes, business rules, important entities. Lightweight Event Storming ideas are allowed; do not force a full storm or `.dio` ritual.

**Exit:** MVP-relevant domain facts agreed or assumed. Skip for Simple.

### 4 Scope / MVP — gate 1

Define MVP in, explicit out, and Later. Mark assumptions.

**Exit:** user agrees on MVP boundary (mandatory).

### 5 Decomposition

Two steps. Skip step A for Simple products when the MVP is already one or two changes.

**A. Capabilities (optional)** — group MVP into large product abilities (e.g. Content, Leads). These are labels for orientation, **not** OpenSpec units and **not** a substitute for changes.

**B. OpenSpec-sized changes** — split each capability (or the whole MVP) into **minimal meaningful changes**. Each change must:

- Deliver a **verifiable user-facing result** (or a necessary end-to-end path to one)
- Have clear in/out boundaries
- Be small enough for **one autonomous agent cycle**
- Be usable without implementing the entire capability first
- **Not** list technical tasks (those belong in OpenSpec)

| Scale | Changes |
| --- | --- |
| Small | One change can be the whole MVP |
| Medium | Several independent shippable changes |
| Complex | As many as needed—no quota; merge if two changes are tiny and always ship together |

**Anti-patterns (reject these):**

- Epic slices: “Owner content”, “Leads”, “Comments” with no further split
- Fake verticals / horizontal layers with nice names: “public pages with mock data” → “then storage” → “then admin CRUD” when a thin end-to-end path (create → publish → view) would prove value sooner
- Task-shaped items: “add API”, “add DB schema”, “add UI”

Prefer an early thin vertical (e.g. owner publishes one article and a visitor opens it) over layering UI, then DB, then admin.

Discuss the list; final buy-in is at Review.

### 6 Roadmap

Build an **ordered queue of changes**, not a list of capabilities.

For each change record:

| Field | Meaning |
| --- | --- |
| ID | Stable id, e.g. `CH-001` |
| Slug | Future OpenSpec change name (`kebab-case`) |
| Goal | User-facing outcome |
| Scope | In / out for this change only |
| Acceptance | How to verify done |
| Dependencies | Prior change IDs, or `none` |
| Status | `pending` at plan time |

Call out top risks/assumptions for the queue. Single-change MVPs may live entirely in `product.md`; otherwise use `roadmap.md`.

**Status field:** keep it as a light queue marker (`pending` / later `done` when the user asks to refresh the plan). Do **not** turn `roadmap.md` into a second issue tracker if the project already tracks work elsewhere (e.g. Multica)—statuses stay thin; detailed tracking stays in one place.

### 7 Review — gate 2

Present: MVP, capabilities (if any), **full ordered change queue**, risks. Apply feedback. Require explicit approval ("утверждаю" / "approve" / equivalent).

**On approval — persist** `product.md`, and `roadmap.md` / `domain.md` when needed. `planning/context.md` may be created or updated before approval when inbox is in use. All other planning artifacts are written after final approval unless the user explicitly requests an earlier draft.

### 8 OpenSpec handoff

Do **not** invent a new shortlist of 1–3 slugs. The queue already exists in the roadmap.

1. Point to the **first pending** change (lowest order with `Status: pending`).
2. Restate its Goal, Scope, Acceptance, Dependencies.
3. Suggest the user run `/opsx:propose` (or their OpenSpec flow) for that slug.
4. Stop. Do not create OpenSpec artifacts or code.

## Readiness bar

Before handoff, all must be yes:

1. Who and what pain?
2. MVP in / out clear?
3. Changes are OpenSpec-sized (not epics), ordered, with acceptance?
4. Dangerous assumptions named?
5. Enough that the next agent need not invent scope for change 1?

If no → return to Discovery, Domain, or Decomposition.

## Artifacts (target product project)

| File | When |
| --- | --- |
| `planning/context.md` | When inbox is in use — **may write/update before approval** |
| `planning/product.md` | After final approval (always on persist) |
| `planning/roadmap.md` | After final approval when more than one change, or when the queue needs a dedicated file |
| `planning/domain.md` | After final approval, if Complex / non-trivial domain |

Tiny products: single `planning/product.md` embedding the one (or few) changes.

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

## Requirements & constraints
- Confirmed:
- Assumptions:
- Open:

## Sources
- Key inbox / docs (paths):

## OpenSpec handoff
- First pending change: `CH-001` / `kebab-slug` — see planning/roadmap.md (or Changes below)
```

### `roadmap.md` template

```markdown
# Roadmap

## Risks & assumptions
- …

## Capability: <Name>   <!-- omit Capability headings for tiny products -->

### CH-001: <Short title>
Status: pending
OpenSpec: kebab-slug

Goal:
…

Scope:
- In: …
- Out: …

Acceptance:
- …

Dependencies: none

### CH-002: …
Status: pending
OpenSpec: …

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
- planning/product.md
- planning/roadmap.md (if any)
- planning/domain.md (if any)
- planning/context.md (if any)

First pending change:
- ID: CH-00N
- OpenSpec slug: kebab-slug
- Goal: …
- Scope: …
- Acceptance: …
- Dependencies: …

Next step for you: `/opsx:propose` (or equivalent) for `kebab-slug`.
Product Planner stops here.
```

## Gotchas

- Minimal depth first—skip empty ritual
- Decomposition fails if you only name capabilities/epics—always produce OpenSpec-sized changes
- Prefer thin end-to-end value over UI-then-DB-then-admin layering
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
