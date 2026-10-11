---
name: product-planner
description: >-
  Interactive product planning before implementation: clarify intent, discover
  requirements against an optional planning/inbox, define MVP, decompose into
  vertical slices, order delivery, and hand off context for OpenSpec changes.
  Use when the user describes a product idea, asks for MVP scope, roadmap,
  domain discovery, or pre-coding product planning. Do not use for OpenSpec
  propose/apply, writing specs/tasks, or implementing code.
license: MIT
metadata:
  version: "1.0.0"
---

# Product Planner

Facilitate product discovery from a rough idea to an approved MVP, vertical slices, and implementation order—enough context to start OpenSpec changes. Do not replace OpenSpec or write application code.

Speak and write planning artifacts in the user's language. Keep these instructions in English.

## Primacy rule: minimal depth

Choose the **shallowest** investigation that still supports decisions. Skip phases that add no decision value. Do not keep researching for documentary completeness. Prefer a short correct plan over a full checklist walkthrough.

This rule overrides habit: if the product is small, collapse phases, skip Domain, and avoid inventing slices or gates.

## Hard boundaries

**Do**

- Ask before inventing requirements; label `Confirmed` / `Assumption` / `Open`
- Prefer vertical slices with user value over horizontal tech layers
- Reuse existing project docs; avoid parallel sources of truth
- Stop after OpenSpec handoff unless the user explicitly asks to continue elsewhere

**Do not**

- Create `openspec/changes/*`, delta specs, `tasks.md`, or run `/opsx:*`
- Implement features, scaffolds, or refactors without explicit permission
- Mandate DDD, CQRS, microservices, Event Storming, or heavy architecture
- Fill template sections with guessed content
- Duplicate BMAD agents, PRFAQ, market research packs, or sprint machinery

## Progress checklist

Track lightly (chat is enough). Skip items that do not apply:

```
- [ ] 0 Bootstrap
- [ ] 1 Intent
- [ ] 2 Discovery
- [ ] 3 Domain lite (skip if not needed)
- [ ] 4 Scope / MVP          ← mandatory approval
- [ ] 5 Decomposition
- [ ] 6 Roadmap
- [ ] 7 Review + persist     ← mandatory approval
- [ ] 8 OpenSpec handoff
```

### Mandatory gates (only two)

1. **MVP boundary** (end of phase 4) — user agrees what is in / out / later.
2. **Final plan** (phase 7) — user explicitly approves; then persist artifacts.

Complexity level, domain notes, and slice lists are discussed as needed—not separate formal confirmation rounds.

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

**Inbox source tags** (`Supported by inbox` / `Conflicts with inbox` / `Not in inbox`) — use **only when inbox materials are in play** (folder non-empty or `context.md` exists from inbox). If there is no inbox content, do not tag every answer `Not in inbox`; that adds noise.

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
| Simple | One actor, CRUD/util, few rules | Skip Domain; one `product.md` may embed slices |
| Standard | Several scenarios, light integrations | Full path; domain notes in `product.md` or short `domain.md` |
| Complex | Multiple roles, stateful processes, money/approvals, contested rules | Domain lite before Scope |

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

Split MVP into **vertical slices** that match the product—not a fixed count.

| Scale | Slices |
| --- | --- |
| Small | One slice is fine if the MVP is a single shippable capability; or a few natural cuts |
| Medium | Several independent user-facing capabilities |
| Complex | More slices as needed, ordered by dependencies and risk |

Each slice: value, "done enough", rough dependencies. Avoid horizontal-only slices (e.g. "DB only"). Do not invent extra slices to hit a quota.

Discuss the list; no separate formal approval—final buy-in is at Review.

### 6 Roadmap

Order slices; call out dependencies and top risky assumptions. Single-slice MVPs can fold this into `product.md`.

### 7 Review — gate 2

Present the full plan. Apply feedback (loop back as needed). Require explicit approval ("утверждаю" / "approve" / equivalent).

**On approval — persist** planning files. Earlier disk writes only if the user asks.

### 8 OpenSpec handoff

Handoff block in chat + section in `product.md` / `roadmap.md`. Recommend first OpenSpec change slugs (one-line intent each)—typically one for a tiny product, a few for larger ones. Do **not** create OpenSpec artifacts or code.

Then stop.

## Readiness bar

Before handoff, all must be yes:

1. Who and what pain?
2. MVP in / out clear?
3. Slices and order clear? (one slice is valid)
4. Dangerous assumptions named?
5. Enough that the next agent need not invent business rules for slice 1?

If no → return to Discovery or Domain.

## Artifacts (target product project)

Write under `planning/` after Review approval (default):

| File | When |
| --- | --- |
| `planning/context.md` | Inbox was used |
| `planning/product.md` | Always on persist |
| `planning/roadmap.md` | More than one slice or ordering/dependencies matter |
| `planning/domain.md` | Complex / non-trivial domain only |

Tiny products: single `planning/product.md` with embedded slices and order.

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

## Requirements & constraints
- Confirmed:
- Assumptions:
- Open:

## Sources
- Key inbox / docs (paths):

## OpenSpec handoff
- Recommended first changes:
  - `kebab-slug` — one-line intent
```

### `roadmap.md` template

```markdown
# Roadmap

## Slices
1. **Name** — value; done enough; depends on …
2. …

## Order & dependencies
…

## Risks & assumptions
…
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

Suggested first changes (create with OpenSpec yourself):
1. `slice-slug` — …
2. `…` — …

Product Planner stops here. Next: open an OpenSpec change for slice 1.
```

## Gotchas

- Minimal depth first—skip empty ritual
- Questions before documents; persist after final approval (`context.md` index only when inbox is used)
- Do not auto-create empty `planning/inbox/`
- Inbox tags only when materials exist
- Only two mandatory gates: MVP and final plan
- Do not mandate Event Storming—only when Domain lite triggers fire
- Never silent-merge inbox vs user conflicts
- Do not propose microservices/CQRS/DDD without a concrete risk and user interest
- Do not start coding or OpenSpec apply after handoff
- Prefer fewer sharper artifacts over three empty files
