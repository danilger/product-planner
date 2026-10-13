# Product Planner — keyword `next`

Not a Cursor slash command. When the user sends the word **`next`** in chat during a Product Planner session:

Read `skills/product-planner/SKILL.md` and `skills/product-planner/00_pipeline_state.md`.

Advance the pipeline **one stage forward**:

1. If there is no `00_pipeline_state.md`, tell the user to run `/product-planner` first.
2. Verify the current stage is finished (`done` or `skipped`). If not, refuse and say what is missing.
3. Increment `current_stage`. If the next stage was `stale`, rebuild its artifact; do not reuse stale content blindly.
4. Update `00_pipeline_state.md` (`updated_at`, statuses).
5. Briefly explain the next stage purpose and expected output.
6. Execute that stage per SKILL.md.
7. End with: ask whether they want to add anything for this stage; if not, they should send the word `next` again.

Never write `/next`. Never skip stages. Never create OpenSpec folders or application code.
