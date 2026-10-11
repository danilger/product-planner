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

Copy and update mentally (or in chat) as you go:

```
- [ ] 0 Bootstrap
- [ ] 1 Intent
- [ ] 2 Discovery
- [ ] 3 Domain lite (skip if not needed)
- [ ] 4 Scope / MVP
- [ ] 5 Decomposition
- [ ] 6 Roadmap
- [ ] 7 Review + persist
- [ ] 8 OpenSpec handoff
```

## Phase 0: Bootstrap

On activation:

1. Scan the target project for existing product docs (`planning/`, README, specs, ADRs). Summarize what already exists; extend it instead of forking a second canon.
2. Ensure `planning/inbox/` exists (create empty dir if missing and the environment allows writes).
3. Invite materials (do not block on them):

> Drop any materials into `planning/inbox/` — briefs, notes, screenshots, exports. Chaos is fine. Say "inbox ready" or just continue; I will use whatever is already there.

4. If inbox is non-empty: read it, draft/update `planning/context.md` (index only), and give a short finding summary.
5. Proceed to Intent immediately; inbox can grow in parallel.

### `planning/context.md` (index, not a PRD)

Create or refresh when inbox is non-empty or the user says inbox is ready:

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

Do not rewrite inbox originals. Do not dump large verbatim quotes into later artifacts—cite paths and conclusions.

## Context grounding (required)

On every turn in phases 1–4 (and whenever revisiting them):

1. Before asking questions, align open topics with inbox / `context.md`.
2. After user answers, tag evidence:
   - `Supported by inbox: <path>`
   - `Conflicts with inbox: <path>` — surface the conflict; ask which source wins; never silent-merge
   - `Not in inbox` — user statement or assumption only
3. Prefer recommendations that cite concrete files.
4. Fragments that look like rules but are unclear → `Assumption` or a clarifying question. Do not invent business rules.
5. If new files appear mid-session: re-read, update the index, note impact on already agreed points.

### Inbox budget

- Many files: index by name/title first; deep-read only what matters for the current phase
- Images: capture relevant UI/flows/tables in the index; cite paths in `product.md`
- Ask once if inbox is authoritative truth or archive
- Extra material must not inflate MVP—park as Out of scope / Later

## Questioning protocol

- Usually **3–5 questions per turn**, then a short summary: Confirmed / Assumption / Open (+ grounding tags)
- Hunt contradictions, risks, and unknown unknowns
- One follow-up on a vague answer, then park as `Assumption` and move on if still unclear
- User may say "enough" / "write the plan" to skip remaining grill slots

## Complexity ladder

After Intent + early Discovery, propose a level and confirm with the user:

| Level | Signals | Path |
| --- | --- | --- |
| Simple | One actor, CRUD/util, few rules | Skip Domain; short product doc may embed slices |
| Standard | Several scenarios, light integrations | Full path; domain notes inside `product.md` or short `domain.md` |
| Complex | Multiple roles, stateful processes, money/approvals, contested rules | Run Domain lite before Scope |

## Phases 1–8

### 1 Intent

Capture: problem, who hurts, desired outcome. Strip stack noise. Use inbox if present.

**Exit:** draft understanding of problem, audience, outcome (may still have Opens).

### 2 Discovery

Clarify scenarios, constraints, success signals, risks. Resolve or park contradictions with inbox.

**Exit:** no critical unresolved contradictions; Opens do not block MVP; complexity level chosen.

### 3 Domain lite (conditional)

Triggers: multiple roles with different powers, long-running processes, status machines, money/compliance, contested rules.

Collect only what affects MVP: actors, key events/processes, business rules, important entities. Lightweight Event Storming ideas are allowed; do not force a full storm or `.dio` ritual.

**Exit:** MVP-relevant domain facts agreed or explicitly assumed. Skip entirely for Simple.

### 4 Scope / MVP

Define MVP in, explicit out, and Later. Mark assumptions. Confirm verbally before treating MVP as fixed.

**Exit:** user agrees on MVP boundary.

### 5 Decomposition

Split MVP into **3–8 vertical slices** (user-valuable, independently shippable/testable). Each slice: value, "done enough", rough dependencies. Avoid horizontal-only slices (e.g. "DB only").

**Exit:** slice list agreed.

### 6 Roadmap

Order slices; call out dependencies and top risky assumptions.

**Exit:** sequence + risks clear.

### 7 Review

Present the full plan. Apply feedback (loop back to Discovery/Domain/Scope as needed). Require explicit approval ("утверждаю" / "approve" / equivalent).

**On approval — persist** planning files (see Artifacts). Earlier disk writes only if the user asks.

### 8 OpenSpec handoff

Output handoff block (chat + section in `product.md` or `roadmap.md`). Recommend 1–3 first OpenSpec change slugs with one-line intent each. Do **not** create OpenSpec artifacts or code.

Then stop.

## Readiness bar

Before handoff, all must be yes:

1. Who and what pain?
2. MVP in / out clear?
3. Slices and order clear?
4. Dangerous assumptions named?
5. Enough that the next agent need not invent business rules for slice 1?

If no → return to Discovery or Domain.

## Artifacts (target product project)

Write under `planning/` after Review approval (default):

| File | When |
| --- | --- |
| `planning/context.md` | Inbox non-empty (may exist earlier) |
| `planning/product.md` | Always on persist |
| `planning/roadmap.md` | More than one slice or ordering/dependencies matter |
| `planning/domain.md` | Complex / non-trivial domain only |

Tiny products may use a single `planning/product.md` that embeds slices and order.

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

- Questions first; documents after Scope/Review (except early `context.md` index)
- Do not mandate Event Storming—only when Domain lite triggers fire
- Inbox conflicts are first-class; never overwrite user answers with stale TZ silently
- Do not propose microservices/CQRS/DDD without a concrete risk and user interest
- Do not start coding or OpenSpec apply after handoff
- Prefer fewer sharper artifacts over three empty files
- Keep turns short enough for cheaper models: checklist + gates + templates, not essays
