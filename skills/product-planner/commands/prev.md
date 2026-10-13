# Product Planner — keyword `prev`

Not a Cursor slash command. When the user sends **`prev`** or **`prev <N>`** in chat during a Product Planner session:

Read `skills/product-planner/SKILL.md` and `skills/product-planner/00_pipeline_state.md`.

Go **back** in the pipeline:

- `prev` → target = `current_stage - 1` (minimum 1)
- `prev <N>` → target = N (integer 1…5)

Rules:

1. If no pipeline state, tell the user to run `/product-planner` first.
2. If N is greater than current stage, refuse: forward only via the word `next`.
3. Set `current_stage` to the target.
4. Mark every stage **greater than** the target as `stale` in `00_pipeline_state.md`.
5. Tell the user later artifacts are stale and will be rebuilt on the next forward pass.
6. Briefly restate the target stage purpose; continue work **on that stage only** (do not auto-run the rest of the pipeline).
7. Ask the end-of-stage question when appropriate.

Never write `/prev`. Do not delete stale files immediately. Do not write application code.
