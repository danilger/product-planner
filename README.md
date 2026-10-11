# product-planner

Agent Skill for **pre-implementation product planning**: clarify the idea, discover requirements, define MVP, decompose into optional capabilities and an **ordered queue of OpenSpec-sized changes**, then hand off the first pending change.

It does **not** replace OpenSpec (proposals, specs, tasks) and does **not** write application code.

Portable Agent Skills format ([agentskills.io](https://agentskills.io)). Works with Cursor and other compatible agents.

## What you get

After an interactive session you should have:

- A clear product intent and audience
- An agreed MVP (in / out / later)
- Optional capability groupings for orientation
- An ordered change queue (`## Ordered Changes`, `CH-001`…) with slug, optional capability label, goal, scope, acceptance, dependencies
- Distinctions between confirmed facts, assumptions, and open questions
- Optional grounding against materials in `planning/inbox/`
- Handoff pointing at the **first pending** change for `/opsx:propose`

## Process (short)

0. **Bootstrap** — read existing docs; offer optional inbox  
1. **Intent** — problem, users, outcome  
2. **Discovery** — questions, risks (grounded in inbox when present)  
3. **Domain lite** — only if the domain is non-trivial  
4. **Scope** — MVP boundary (**approval**)  
5. **Decomposition** — capabilities (optional) → OpenSpec-sized changes  
6. **Roadmap** — ordered change queue  
7. **Review** — approve (**approval**), then write `planning/*.md`  
8. **Handoff** — first pending change → suggest `/opsx:propose`; stop  

Flow:

`Idea → Discovery → MVP → Capabilities? → Ordered OpenSpec Changes → OpenSpec`

Depth scales with the product. Only MVP and final plan are hard approval gates. Epic-sized “vertical slices” without further split are not enough.

## Inbox (optional)

If you have materials, put them in the **target product project**:

```text
planning/inbox/     # notes, briefs, screenshots, exports — any mess
planning/context.md # short index written by the agent when inbox is used
planning/product.md # after you approve the plan
planning/roadmap.md # global Ordered Changes queue (capability is a field)
planning/domain.md  # only for non-trivial domains
```

The agent creates `planning/inbox/` when you start using it—not on every activation. You can add files mid-session. Conflicts between inbox and your answers are surfaced, not silently merged.

## Example session

1. `/product-planner` (or describe a product idea so the skill activates)
2. Optionally drop files into `planning/inbox/` and say `inbox ready`
3. Answer clarifying questions
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
| Discovery, MVP, capabilities, **ordered changes** | One concrete change at a time |
| `planning/*.md` product context + change queue | `proposal.md`, delta specs, design, **tasks** |
| Hands off first pending change | Creates and applies that change |

## License

MIT — see [LICENSE](LICENSE).
