# product-planner

Agent Skill for **stage-gated product planning before implementation**: pick a communication language; optionally collect context; interview for intent/MVP; lock existing stack or choose a greenfield architecture; emit a **finely granulated** OpenSpec-ready change plan (sized for weak LLMs and Multica); hand off the first change and offer `AGENTS.md` links.

It does **not** replace OpenSpec and does **not** write application code.

Portable Agent Skills format ([agentskills.io](https://agentskills.io)). Works with Cursor and other compatible agents.

## Commands

| Command | Action |
| --- | --- |
| `/product-planner` | Start / restart at stage 0 |
| `/next` | Advance one stage (only way forward) |
| `/prev` | Go back one stage |
| `/prev <N>` | Jump back to stage N (1…5) |

Saying “nothing to add” does **not** advance — only `/next` does.

Commands live in `skills/product-planner/commands/` and are mirrored to `.cursor/commands/` in this repo.

## Pipeline

`0 → 1 → 2 → 3 → 4 → 5`

| Stage | What | Artifact (in skill folder) |
| --- | --- | --- |
| 0 | Language + rules | `00_pipeline_state.md` |
| 1 | Context (skippable) | `02_context/`, `03_context_summary.md` (always) |
| 2 | Interview | `04_interview.md` |
| 3 | Project state / architecture | `01_project_state.md` |
| 4 | Granulated change plan | `05_change_plan.md` |
| 5 | Handoff + offer `AGENTS.md` links | chat |

Project-state review comes **after** the product interview.

Each stage starts with a short purpose brief and ends with: *anything to add? if not → `/next`*.

## What you get

- Agreed communication language for the session
- Optional context index (`03`) — empty file if skipped
- Interview log with Confirmed / Assumption / Open
- Project state: existing canon, overrides, or greenfield target (`[проверенные]` / `[в тренде]`, web search when available)
- Ordered, **split-checked** changes (`SPLIT_FILES` / `SPLIT_BE_FE` / `SPLIT_DOMAIN` / `SPLIT_FUNC`) with links and Multica parallel/sequential hints
- Handoff to first pending change → suggest `/opsx:propose`
- Optional `AGENTS.md` artifact links (only after you agree)

## Example session

1. `/product-planner` → choose language
2. Drop files into `skills/product-planner/02_context/` or skip
3. `/next` → interview
4. `/next` → confirm stack canon or pick greenfield stack
5. `/next` → review granulated plan
6. `/next` → handoff; optionally accept `AGENTS.md` links
7. Run `/opsx:propose` for the first change slug

Use `/prev` or `/prev 2` to rewind; later stages become `stale` and are rebuilt on the next forward pass.

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

# Slash commands (Cursor)
mkdir -p .cursor/commands
cp skills/product-planner/commands/*.md .cursor/commands/
```

## Update

```bash
npx skills update product-planner -y
```

## Repository layout

```text
├── README.md
├── LICENSE
├── scratch_02.md          # design source for v2
├── .cursor/commands/      # /product-planner /next /prev
└── skills/
    └── product-planner/
        ├── SKILL.md
        ├── commands/
        ├── templates/
        └── 02_context/
```

## Boundary with OpenSpec

| Product Planner | OpenSpec |
| --- | --- |
| Intent, stack constraints, granulated ordered changes | One concrete change at a time |
| Artifacts in the skill folder | `proposal.md`, specs, **tasks** |
| Split flags + Multica hints | Implements within those constraints |
| Hands off first pending change | Creates and applies that change |

## License

MIT — see [LICENSE](LICENSE).
