# product-planner

Agent Skill for **pre-implementation product planning**: clarify the idea, discover requirements, define MVP, split into vertical slices, order delivery, and hand off enough context to start [OpenSpec](https://openspec.dev) changes.

It does **not** replace OpenSpec (proposals, specs, tasks) and does **not** write application code.

Portable Agent Skills format ([agentskills.io](https://agentskills.io)). Works with Cursor and other compatible agents.

## What you get

After an interactive session you should have:

- A clear product intent and audience
- An agreed MVP (in / out / later)
- Vertical slices with dependencies and order
- Distinctions between confirmed facts, assumptions, and open questions
- Optional grounding against materials you dropped into `planning/inbox/`
- Suggested first OpenSpec change slugs (names + intent only)

## Process (short)

0. **Bootstrap** — read existing docs; invite `planning/inbox/` materials  
1. **Intent** — problem, users, outcome  
2. **Discovery** — questions, contradictions, risks (grounded in inbox when present)  
3. **Domain lite** — only if the domain is non-trivial  
4. **Scope** — MVP boundary  
5. **Decomposition** — 3–8 vertical slices  
6. **Roadmap** — order and risks  
7. **Review** — approve, then write `planning/*.md`  
8. **Handoff** — OpenSpec next steps; stop  

Depth scales: a small utility may need only a short plan; a complex product can use domain modeling and a fuller roadmap.

## Inbox (optional, from the start)

Drop chaotic materials into the **target product project**:

```text
planning/inbox/     # notes, briefs, screenshots, exports — any mess
planning/context.md # short index written by the agent
planning/product.md # after you approve the plan
planning/roadmap.md # if multiple slices / ordering matters
planning/domain.md  # only for non-trivial domains
```

You can add files at any time. The agent should cite inbox sources, flag conflicts with your answers, and keep MVP from ballooning off old specs.

## Example session

In an agent chat (with the skill installed), roughly:

1. `/product-planner` (or describe a product idea so the skill activates)
2. Optionally drop files into `planning/inbox/` and say `inbox ready`
3. Answer a few rounds of clarifying questions
4. Approve MVP and slice order
5. Get `planning/product.md` (+ roadmap/domain as needed) and suggested OpenSpec change names
6. Create the first OpenSpec change yourself (e.g. `/opsx:propose`)

## Install

Requires Node for the [skills](https://github.com/vercel-labs/skills) CLI.

```bash
# Project-local (Cursor)
npx skills add <owner>/<repo> -a cursor -y

# Global (all projects on this machine)
npx skills add <owner>/<repo> -g -a cursor -y
```

Replace `<owner>/<repo>` with the GitHub path of this repository after you publish it.

### Manual install

Copy the skill folder into an agent skills directory:

```bash
# Cursor / Agent Skills common paths
cp -r skills/product-planner .agents/skills/product-planner
# or
cp -r skills/product-planner ~/.cursor/skills/product-planner
```

## Update

```bash
npx skills update product-planner
```

Or re-copy the folder if you installed manually.

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
