# Implementation workflow

Read this reference only after the user explicitly opts into implementing selected recommendations, whether in the current task or through orchestrated workers. The review criteria, protected guarantees, and phase boundaries in [SKILL.md](../SKILL.md) continue to apply.

Maintain the [recommendation and implementation ledger](review-rubric.md#recommendation-and-implementation-ledger) in the live review.

## Prepare implementation

When the user opts in, freeze the selected recommendation set and create a dependency graph before editing or dispatching work.

Before creating implementation branches or worktrees or dispatching orchestrated work, state any repository-mandated Git constraints and ask the user whether they prefer a specific branch organization and integration strategy. Cover both how units should be branched, such as one branch per unit or stacked branches, and how approved work should land, such as rebase plus fast-forward-only, fast-forward-only, cherry-pick, squash, or merge commits. Recommend a practical default, record the answer as a phase-level constraint, and carry it into monitoring instructions. If the user already stated a preference in the current conversation, confirm and record it instead of asking again.

The coordinating agent owns this skill's phase controls, frozen recommendation set, dependency graph, live-review updates, independent evaluation, correction loop, integration, user communication, and transition to re-review.

Do not instruct a delegated implementation worker to load or use this full skill merely because its unit originated from the review. Give the worker a self-contained prompt that distills the human-maintainability objective, task-local evidence, required after-state, protected guarantees, exclusions, prerequisites, validation, and handoff format. The worker must read applicable repository instructions and may use narrower technical or implementation skills that independently fit its task. Ask a worker to use this skill only when its assigned task is itself an independent cognitive-simplicity review or re-review, not a bounded implementation unit.

For each implementation unit:

- Restate the human-maintainability objective, scope, current evidence, required after-state, essential guarantees, exclusions, prerequisites, validation, and handoff format.
- Select the latest suitable platform model with at least `high` reasoning under the [model-routing rule in SKILL.md](../SKILL.md#route-model-and-reasoning-practically).
- Keep one writer per checkout or worktree. Use isolated tasks, conversations, or worktrees only when the platform supports them and the user authorizes orchestration.
- Start a dependent unit only after its prerequisites are integrated and validated.
- Prefer small, reviewable, bisectable changes. Do not introduce compatibility layers, parallel contracts, or speculative abstractions unless explicitly required.
- Review the complete diff and validation evidence independently before integration. Never approve or integrate solely because the worker reports success.
- When that review finds a substantive problem, send a focused correction request back to the same worker or isolated task. Include the concrete evidence, the guarantee at risk, and the required after-state; require an updated diff and proportional validation, then independently re-review the result. Repeat only while the correction loop is making progress. Do not integrate known defects or silently repair substantive worker mistakes in the coordinator checkout; report a blocker when the unit cannot reach an acceptable state.
- Integrate in dependency order, preserve user-owned work, resolve conflicts against the intended after-state, and rerun proportional checks in the destination.
- Apply the recorded Git strategy consistently. Surface any conflict with a mandatory repository policy before dispatching work. If the user explicitly has no preference and the repository is silent, keep destination history linear: rebase an approved worker branch onto the current destination, then integrate it with fast-forward-only. Use cherry-pick for selected atomic commits, or squash only when intermediate worker commits are not independently useful. Do not create merge commits unless the repository or user explicitly requests them. Never rewrite already-integrated history without explicit approval.
- Notify the user periodically only for meaningful progress, integrations, newly started dependent work, blockers, or decisions requiring input.
- Append dated implementation evidence linked to the relevant finding when it confirms or contradicts the baseline. Keep baseline findings, assessment statuses, and scores unchanged; record progress in the implementation ledger and reserve revised scores for the separately approved re-review.

## Monitor orchestrated work practically

Use bounded task/thread waits during the initial handoff, even when scheduled monitoring is available. Confirm that each worker started from the intended state, read the applicable instructions, understood the assignment and protected guarantees, froze the correct scope, and chose a sound implementation direction. Inspect enough early progress to catch a mistaken plan before leaving the worker unattended, and intervene immediately when its direction would add accidental complexity, weaken quality, or conflict with the dependency graph.

Once the handoff and direction are trustworthy, stop actively waiting when the platform supports scheduled work. Prefer a conversation-attached scheduled heartbeat or recurring follow-up for the remaining long-running orchestration. Choose a practical interval for the expected task duration, give the scheduled run enough state to resume coordination, and have it check task progress, review completed work, integrate approved units, and start newly unblocked dependencies. Scheduled monitoring does not broaden the user's authorization or relax the review and validation requirements above.

When scheduled work is unavailable, continue with bounded task/thread waits. Also use a short bounded wait when completion is genuinely imminent and an immediate result is useful. Carry forward task cursors where supported and back off between unchanged checks; do not busy-poll.

## Complete implementation

After all selected units are integrated, finish the implementation phase with the integrated commits, validation evidence, unresolved risks, deferred work, and any implementation evidence that confirmed or challenged the baseline. Do not automatically begin the re-review.

Explain the optional third phase in plain language: it independently tests whether the integrated code actually became easier to understand without losing quality, rather than merely checking that implementation tasks were completed. Describe the evidence it will revisit and ask whether the user wants to opt in.
