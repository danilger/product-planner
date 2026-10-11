# product-planner

Agent Skill for **pre-implementation product planning**: clarify the idea; discover **product, technical, and UX/UI** preferences; define MVP; decompose into optional capabilities and an **ordered queue of OpenSpec-sized changes**; then hand off the first pending change.

It does **not** replace OpenSpec (proposals, specs, tasks) and does **not** write application code.

Portable Agent Skills format ([agentskills.io](https://agentskills.io)). Works with Cursor and other compatible agents.

## What you get

After an interactive session you should have:

- A clear product intent and audience
- An agreed MVP (in / out / later)
- **Technical direction** (required / forbidden / existing / open)—or citations to `AGENTS.md` / architecture docs; sticky choices (framework, UI kit, backend shape) are confirmed or explicitly deferred, not guessed
- **UX/UI direction** when the product has a meaningful UI (or an explicit N/A), including critical journeys that affect MVP / change boundaries
- Optional capability groupings for orientation
- An ordered change queue (`## Ordered Changes`, `CH-001`…) with slug, optional capability label, goal, scope, acceptance, dependencies
- Distinctions between confirmed facts, assumptions, and open questions
- Optional grounding against materials in `planning/inbox/`
- Handoff pointing at the **first pending** change for `/opsx:propose`

## Why technical and UX/UI discovery belong here

OpenSpec designs **one change**. By then, cross-cutting choices (Next.js vs SPA, shadcn vs ad-hoc CSS, dark editorial vs generic dashboard) must already be fixed—or every coding agent invents its own product.

Product Planner locks **preferences and constraints**. OpenSpec implements **within** them.

Depth is adaptive: a CLI may skip UX/UI; a personal site may treat visual direction as mandatory before MVP.

## Process (short)

0. **Bootstrap** — read existing docs and implementation; offer optional inbox  
1. **Intent** — problem, users, outcome  
2. **Product Discovery** — scenarios, risks (grounded in inbox when present)  
3. **Technical Discovery** — stack preferences, integrations, **forbidden** choices (cite existing canon when present; no silent Assumed on sticky stack choices)  
4. **UX/UI Discovery** — visual direction, references, critical journeys (skip if no UI)  
5. **Domain lite** — only if the domain is non-trivial  
6. **Discovery Coverage** — internal check that material product / tech / UX gaps are closed or deferred; sticky decisions not Assumed  
7. **Scope** — MVP boundary (**approval**)  
8. **Decomposition** — capabilities (optional) → OpenSpec-sized changes  
9. **Roadmap** — ordered change queue  
10. **Review** — approve (**approval**), then write `planning/*.md`  
11. **Handoff** — first pending change → suggest `/opsx:propose`; stop  

Flow:

`Idea → Product + Tech + UX Discovery → Coverage → MVP → Ordered OpenSpec Changes → OpenSpec`

Only MVP and final plan are hard approval gates. Epic-sized “vertical slices” without further split are not enough.

## Inbox (optional)

If you have materials, put them in the **target product project**:

```text
planning/inbox/     # notes, briefs, screenshots, exports — any mess
planning/context.md # short index written by the agent when inbox is used
planning/product.md # after you approve the plan (includes Technical + UX/UI sections)
planning/roadmap.md # global Ordered Changes queue (capability is a field)
planning/domain.md  # only for non-trivial domains
planning/technical.md  # optional — only if Technical Direction outgrows product.md
planning/design.md     # optional — only if UX/UI Direction outgrows product.md
```

The agent creates `planning/inbox/` when you start using it—not on every activation. It does **not** auto-create `technical.md` / `design.md`. You can add files mid-session. Conflicts between inbox and your answers are surfaced, not silently merged.

If the repo already has `AGENTS.md` or architecture docs for the stack, planning should **link** to them instead of duplicating rules.

## Example session

1. `/product-planner` (or describe a product idea so the skill activates)
2. Optionally drop files into `planning/inbox/` and say `inbox ready`
3. Answer clarifying questions (product, tech preferences, UX/UI as relevant)
4. Approve MVP, then the final plan (including the change queue)
5. Get `planning/product.md` + `planning/roadmap.md` (when needed)
6. Run `/opsx:propose` for the first pending change slug from the roadmap

## Install

Requires Node for the [skills](https://github.com/vercel-labs/skills) CLI.

```bash
# Project-local (Cursor)
npx skills add danilger/product-planner -a cursor -y

# Global (all projects on this machine)
npx skills add danilger/product-planner -g -a cursor -y
```

### Manual install

```bash
cp -r skills/product-planner .agents/skills/product-planner
# or
cp -r skills/product-planner ~/.cursor/skills/product-planner
```

## Update

If you installed via the skills CLI, refresh from the recorded source:

```bash
npx skills update product-planner -y
```

Use `-g` or `-p` to limit scope to global or project installs. Re-run `npx skills add danilger/product-planner …` if the skill was installed manually or the lockfile has no source entry.

## Repository layout

```text
├── README.md
├── LICENSE
└── skills/
    └── product-planner/
        └── SKILL.md
```

## Boundary with OpenSpec

| Product Planner | OpenSpec |
| --- | --- |
| Product + tech preferences + UX/UI direction, MVP, **ordered changes** | One concrete change at a time |
| `planning/*.md` context + change queue | `proposal.md`, delta specs, design, **tasks** |
| Forbidden stack / aesthetic drift | Implements within those constraints |
| Hands off first pending change | Creates and applies that change |

## License

MIT — see [LICENSE](LICENSE).
