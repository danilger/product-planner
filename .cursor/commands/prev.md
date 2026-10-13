# Product Planner — /prev

Read `skills/product-planner/SKILL.md` and `skills/product-planner/00_pipeline_state.md`.

Go **back** in the Product Planner pipeline:

- `/prev` → target = `current_stage - 1` (minimum 1)
- `/prev <N>` → target = N (integer 1…5)

Rules:

1. If no pipeline state, tell the user to run `/product-planner` first.
2. If N is greater than current stage, refuse: forward only via `/next`.
3. Set `current_stage` to the target.
4. Mark every stage **greater than** the target as `stale` in `00_pipeline_state.md`.
5. Tell the user later artifacts are stale and will be rebuilt on the next forward pass.
6. Briefly restate the target stage purpose; continue work **on that stage only** (do not auto-run the rest of the pipeline).
7. Ask the end-of-stage question when appropriate.

Do not delete stale files immediately. Do not write application code.
