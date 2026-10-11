# product-planner

Agent Skill for **pre-implementation product planning**: clarify the idea, discover requirements, define MVP, split into vertical slices, order delivery, and hand off enough context to start [OpenSpec](https://openspec.dev) changes.

It does **not** replace OpenSpec (proposals, specs, tasks) and does **not** write application code.

Portable Agent Skills format ([agentskills.io](https://agentskills.io)). Works with Cursor and other compatible agents.

## What you get

After an interactive session you should have:

- A clear product intent and audience
- An agreed MVP (in / out / later)
- Vertical slices with dependencies and order (one slice is fine for a tiny MVP)
- Distinctions between confirmed facts, assumptions, and open questions
- Optional grounding against materials in `planning/inbox/`
- Suggested first OpenSpec change slugs (names + intent only)

## Process (short)

0. **Bootstrap** — read existing docs; offer optional inbox  
1. **Intent** — problem, users, outcome  
2. **Discovery** — questions, risks (grounded in inbox when present)  
3. **Domain lite** — only if the domain is non-trivial  
4. **Scope** — MVP boundary (**approval**)  
5. **Decomposition** — vertical slices sized to the product  
6. **Roadmap** — order and risks  
7. **Review** — approve (**approval**), then write `planning/*.md`  
8. **Handoff** — OpenSpec next steps; stop  

Depth scales with the product. Only MVP and final plan are hard approval gates.

## Inbox (optional)

If you have materials, put them in the **target product project**:

```text
planning/inbox/     # notes, briefs, screenshots, exports — any mess
planning/context.md # short index written by the agent when inbox is used
planning/product.md # after you approve the plan
planning/roadmap.md # if multiple slices / ordering matters
planning/domain.md  # only for non-trivial domains
```

The agent creates `planning/inbox/` when you start using it—not on every activation. You can add files mid-session. Conflicts between inbox and your answers are surfaced, not silently merged.

## Example session

1. `/product-planner` (or describe a product idea so the skill activates)
2. Optionally drop files into `planning/inbox/` and say `inbox ready`
3. Answer clarifying questions
4. Approve MVP, then the final plan
5. Get `planning/product.md` (+ roadmap/domain as needed) and suggested OpenSpec change names
6. Create the first OpenSpec change yourself (e.g. `/opsx:propose`)

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
| Discovery, MVP, slices, order | Concrete changes |
| `planning/*.md` product context | `proposal.md`, delta specs, design, tasks |
| Suggests change slugs | Creates and applies changes |

## License

MIT — see [LICENSE](LICENSE).
