---
name: cognitive-simplicity-review
description: "Review a repository, application, subsystem, or feature for cognitive complexity and human maintainability. Use when asked to assess readability, navigability, onboarding difficulty, ownership clarity, execution-path traceability, overengineering, unnecessary abstractions or indirection, duplicated representations, tangled state, spaghetti code, dead or speculative machinery, or similar code smells; maintain an evidence-led live review with 1–10 scores and prioritized recommendations; then explain optional planning, implementation, and re-review capabilities and ask whether the user wants to continue. Implementation and re-review are separate opt-in phases and must preserve correctness and engineering quality."
---

# Cognitive Simplicity Review

Cognitive simplicity means that the code presents the smallest accurate mental model of the system. A developer should be able to find the owner of a behavior, follow its normal execution path, understand its state and boundaries, and make a local change without first reconstructing unrelated parts of the application.

Simplicity is one dimension of engineering quality, not a substitute for it. Never reduce line count, file size, layer count, model count, abstraction count, or apparent complexity at the expense of correctness, security, privacy, data integrity, transaction boundaries, concurrency semantics, performance, operability, testability, external-contract fidelity, or cohesive ownership. A longer or more structured design is better when it makes essential complexity explicit and keeps those guarantees understandable.

## Objective

Reduce accidental cognitive complexity without reducing engineering quality. Seek the smallest accurate mental model of the system, not the fewest files, lines, types, layers, or abstractions.

Treat correctness, security, privacy, data integrity, transaction and concurrency semantics, performance, operability, testability, and external-contract fidelity as constraints. Prefer a longer or more structured design when it makes essential complexity and guarantees easier to understand.

Default to review-only. Do not modify product code, begin a refactor, create implementation tasks, or merge changes until the review is complete and the user explicitly opts into a next phase.

## Route model and reasoning practically

Use the latest suitable model available on the active Codex or Claude platform. Do not hard-code dated model names or assume one model family is always best.

Never request reasoning effort below `high`. Use `high` for focused, well-bounded reviews and implementation units; use a stronger practical setting for broad architecture, ambiguous ownership, concurrency, distributed state, persistence invariants, or consequential integration work. Reserve the maximum setting for a clearly exceptional problem where the additional reasoning cost is justified. If the platform expresses effort through thinking budgets rather than named levels, choose the closest equivalent.

Record material model or effort downgrades when the platform cannot honor the requested routing.

## Establish scope and authority

1. Resolve the exact target, reviewed ref or working tree, exclusions, requested depth, output location, and whether the system is pre-release.
2. Read every applicable repository instruction file before judging or editing anything. Preserve unrelated and user-owned changes.
3. Keep the review inside the requested scope. Inspect adjacent code only when needed to understand an inbound or outbound boundary, and label it as context rather than silently expanding the review.
4. Treat current source, executable tests, schemas, generated contracts, runtime composition, and observed behavior as primary evidence. Use documentation, plans, spikes, and ADRs to understand reasoning and intended boundaries, but do not infer current truth from prose alone. Respect any authority explicitly assigned by repository instructions or the user.
5. Treat compatibility, versioning, hardening, and guardrail concerns according to the user's context. Do not let generic hardening dominate a maintainability review unless it materially affects correctness or the mental model.

## Create and maintain the live review

Read [review-rubric.md](references/review-rubric.md) completely before creating the review document.

Create the review document early, after the first orientation pass. Use the user's requested location; otherwise follow an existing repository convention, or use a scoped `.review/` directory when no convention exists.

Write findings while investigating. Mark uncertain items as hypotheses. As evidence changes, amend, merge, downgrade, or delete earlier findings instead of preserving a stale conclusion for appearances. Keep an explicit revision note when a materially important judgment changes.

Never hard-wrap prose merely to satisfy a column width. Preserve deliberate paragraph boundaries and follow any stricter repository Markdown convention.

## Perform the review

### 1. Build the reader's map

Inventory the target's entry points, public contracts, runtime composition, important services, domain concepts, persistence, state machines, external integrations, tests, and generated artifacts. Identify the likely "start here" path for a new developer.

Trace representative happy, failure, cancellation, and cleanup paths end to end. Prefer concrete call chains and state transitions over architecture labels.

### 2. Review by mental-model cost

Investigate at least the minimum angles in the rubric and add target-specific angles when evidence warrants them. Ask throughout:

- Can a reader find the authoritative owner of this behavior or fact?
- Does each layer, model, state, adapter, registry, or helper represent a real semantic difference?
- Does an abstraction remove more concepts and duplicated rules than it introduces?
- Can a developer make the next likely change locally, or must they reconstruct unrelated machinery first?
- Are lifecycle, transaction, concurrency, terminal, and cleanup guarantees explicit and localized?
- Do names and dependency directions tell the truth?
- Do tests teach the public behavior and stable seams, or private construction and incidental implementation details?
- Is this complexity required by the problem, or retained for hypothetical variation, compatibility, or an obsolete path?

Look specifically for duplicated DTOs and conversions, parallel contracts or state representations, pass-through services, reflective binding, service locators, capability matrices, speculative extension points, forwarding layers, catch-all modules, mutable flag combinations, import cycles, policy duplicated in prompts and schemas, test fakes that reproduce production internals, dead branches, stale aliases, and framework ceremony that obscures product behavior.

### 3. Use measurements as clues

Use available static metrics, dependency tools, searches, and test results when they help locate risk. Record the exact command, tool, version when relevant, scope, and result.

Do not equate cognitive-complexity scores, LOC, file length, branch count, class count, import count, or layer count with design quality. A metric identifies where to read; the finding must explain the human reasoning cost and the guarantee any proposed simplification must preserve.

Do not install new tooling merely to manufacture a score when existing evidence is sufficient.

### 4. Prove findings

For every material finding, include:

- a concise title and priority;
- affected files and lines where practical;
- direct code evidence, separated from inference;
- the reader task that becomes difficult;
- the accidental mental-model burden;
- the correctness or quality constraints that must survive;
- the smallest coherent recommendation;
- dependencies, implementation risk, and validation evidence needed;
- confidence and any unresolved question.

Avoid findings that say only "large file," "too many classes," "needs cleanup," or "not best practice." Explain the specific competing owners, repeated decisions, misleading boundary, non-local state, or unnecessary concept.

### 5. Challenge the review

Before finalizing, actively try to disprove the highest-priority findings. Trace the supposedly redundant boundary from both sides, inspect transaction and failure semantics, compare tests, and check whether an apparent duplication protects a trust, lifecycle, persistence, or external-contract boundary.

Revise the live document when later evidence changes the conclusion. Do not reward deletion that would make essential complexity implicit or weaken a guarantee.

### 6. Prioritize a coherent sequence

Rank recommendations by cognitive benefit, correctness risk, dependency order, and reviewability. For each recommendation, describe the intended after-state in plain language and the mental-model concepts it adds, removes, or consolidates.

Separate safe pruning and boundary clarification from high-risk state-machine, persistence, concurrency, or contract rewrites. Identify recommendations that should be deferred because the current implementation works and the rewrite risk exceeds the present cognitive benefit.

## Score the result

Score every investigated angle from 1 to 10 using the rubric anchors. Include evidence, confidence, and a realistic target where useful. Do not force a target of 10; a complex system can be excellent while retaining explicit essential complexity.

Include an overall score only after the per-angle scores. Explain weighting rather than hiding it in an arithmetic average. Distinguish measured static complexity from the review's human-maintainability scores.

## Complete the review before implementation

Finish with:

- the live review path;
- scope and exclusions;
- the current mental model in plain language;
- strongest qualities worth preserving;
- findings and prioritized recommendations;
- per-angle scores and overall assessment;
- validation and evidence gaps;
- the implementation dependency graph and major risk boundaries, without starting implementation.

Explain the workflow as three separately controlled phases:

1. **Review:** the current evidence-led assessment, completed without product-code changes.
2. **Implementation:** an optional phase for selected recommendations, entered only after explicit user approval.
3. **Re-review:** an optional fresh assessment of the integrated result, entered only after a second explicit user approval.

After the initial review, explain these immediate options:

1. Stop at the review.
2. Turn selected recommendations into self-contained prompts, issues, or an implementation plan.
3. Implement selected recommendations incrementally in the current task.
4. Orchestrate isolated Codex or Claude tasks/conversations in dependency order, review every result, integrate approved units, and rerun proportional validation.

Explain how each applicable option would work before asking: what the agent would create, where changes would happen, how dependencies and isolated writers would be handled, what the agent would review before integration, and which validation would follow. Mention that re-review remains available afterward as a separate third phase; do not bundle its approval into implementation.

Ask the user which option they want and which recommendations, if any, should be deferred. Do not infer implementation approval from the review request.

## Implement only after explicit opt-in

When the user opts in, freeze the selected recommendation set and create a dependency graph before editing or dispatching work.

The coordinating agent owns this skill's phase controls, frozen recommendation set, dependency graph, live-review updates, independent evaluation, correction loop, integration, user communication, and transition to re-review.

Do not instruct a delegated implementation worker to load or use this full skill merely because its unit originated from the review. Give the worker a self-contained prompt that distills the human-maintainability objective, task-local evidence, required after-state, protected guarantees, exclusions, prerequisites, validation, and handoff format. The worker must read applicable repository instructions and may use narrower technical or implementation skills that independently fit its task. Ask a worker to use this skill only when its assigned task is itself an independent cognitive-simplicity review or re-review, not a bounded implementation unit.

For each implementation unit:

- Restate the human-maintainability objective, scope, current evidence, required after-state, essential guarantees, exclusions, prerequisites, validation, and handoff format.
- Select the latest suitable platform model with at least `high` reasoning under the routing rule above.
- Keep one writer per checkout or worktree. Use isolated tasks, conversations, or worktrees only when the platform supports them and the user authorizes orchestration.
- Start a dependent unit only after its prerequisites are integrated and validated.
- Prefer small, reviewable, bisectable changes. Do not introduce compatibility layers, parallel contracts, or speculative abstractions unless explicitly required.
- Review the complete diff and validation evidence independently before integration. Never merge solely because the worker reports success.
- When that review finds a substantive problem, send a focused correction request back to the same worker or isolated task. Include the concrete evidence, the guarantee at risk, and the required after-state; require an updated diff and proportional validation, then independently re-review the result. Repeat only while the correction loop is making progress. Do not merge known defects or silently repair substantive worker mistakes in the coordinator checkout; report a blocker when the unit cannot reach an acceptable state.
- Integrate in dependency order, preserve user-owned work, resolve conflicts against the intended after-state, and rerun proportional checks in the destination.
- Notify the user periodically only for meaningful progress, merges, newly started dependent work, blockers, or decisions requiring input.
- Update the live review when implementation evidence confirms or contradicts a finding.

### Monitor orchestrated work practically

Use bounded task/thread waits during the initial handoff, even when scheduled monitoring is available. Confirm that each worker started from the intended state, read the applicable instructions, understood the assignment and protected guarantees, froze the correct scope, and chose a sound implementation direction. Inspect enough early progress to catch a mistaken plan before leaving the worker unattended, and intervene immediately when its direction would add accidental complexity, weaken quality, or conflict with the dependency graph.

Once the handoff and direction are trustworthy, stop actively waiting when the platform supports scheduled work. Prefer a conversation-attached scheduled heartbeat or recurring follow-up for the remaining long-running orchestration. Choose a practical interval for the expected task duration, give the scheduled run enough state to resume coordination, and have it check task progress, review completed work, integrate approved units, and start newly unblocked dependencies. Scheduled monitoring does not broaden the user's authorization or relax the review and validation requirements above.

When scheduled work is unavailable, continue with bounded task/thread waits. Also use a short bounded wait when completion is genuinely imminent and an immediate result is useful. Carry forward task cursors where supported and back off between unchanged checks; do not busy-poll.

After all selected units are integrated, finish the implementation phase with the integrated commits, validation evidence, unresolved risks, deferred work, and any implementation evidence that changed the original review. Do not automatically begin the re-review.

Explain the optional third phase in plain language: it independently tests whether the integrated code actually became easier to understand without losing quality, rather than merely checking that implementation tasks were completed. Describe the evidence it will revisit and ask whether the user wants to opt in.

## Re-review only after a separate opt-in

When the user opts into re-review, assess the integrated destination state as current evidence. Do not assume a merged recommendation worked, award score increases for deleted code, or treat the implementation ledger as proof of improvement.

During re-review:

- Re-read applicable instructions and confirm the reviewed ref, scope, exclusions, and changes since the baseline review.
- Retrace the representative execution, state, failure, cancellation, cleanup, persistence, and composition paths affected by the work.
- Revisit every original finding and score, but also look for new indirection, split ownership, hidden guarantees, or complexity displaced into another module.
- Compare equivalent measurements only when their tool, configuration, and scope are sufficiently consistent; explain any non-comparable evidence.
- Classify each recommendation as effective, partially effective, ineffective, regressed, reverted, deferred, or no longer applicable, with code evidence.
- Verify that correctness and the protected engineering qualities remain intact using proportional tests and direct inspection.
- Amend the live review transparently. Preserve the original baseline, record material reversals, and distinguish remaining essential complexity from accidental complexity.
- Recalculate every investigated angle and the overall assessment. Report unchanged or lower scores honestly when the integrated result does not justify improvement.

Finish with a concise before-and-after comparison, the strongest improvements, quality guarantees verified, remaining or newly introduced problems, residual risks, and any repository guidance worth adding to prevent regression. Further implementation still requires another explicit user opt-in.
