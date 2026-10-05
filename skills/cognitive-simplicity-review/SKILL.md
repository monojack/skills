---
name: cognitive-simplicity-review
description: "Review a repository, subsystem, or feature for accidental cognitive complexity: readability, navigability, ownership clarity, execution-path traceability, unnecessary indirection, duplicated representations, tangled state, and speculative machinery. Findings must demonstrate a concrete burden on understanding or changing the code; this is not a general bug hunt, security audit, performance review, or best-practices review. Maintain a scored evidence-led report and offer planning, implementation, and re-review as separate opt-in phases."
---

# Cognitive Simplicity Review

Cognitive simplicity means that the code presents the smallest accurate mental model of the system. A developer should be able to find the owner of a behavior, follow its normal execution path, understand its state and boundaries, and make a local change without first reconstructing unrelated parts of the application.

Simplicity is one dimension of engineering quality, not a substitute for it. Never reduce line count, file size, layer count, model count, abstraction count, or apparent complexity at the expense of correctness, security, privacy, data integrity, transaction boundaries, concurrency semantics, performance, operability, testability, external-contract fidelity, or cohesive ownership. A longer or more structured design is better when it makes essential complexity explicit and keeps those guarantees understandable.

## Objective

Reduce accidental cognitive complexity without reducing engineering quality. Seek the smallest accurate mental model of the system, not the fewest files, lines, types, layers, or abstractions.

Treat correctness, security, privacy, data integrity, transaction and concurrency semantics, performance, operability, testability, and external-contract fidelity as constraints. Prefer a longer or more structured design when it makes essential complexity and guarantees easier to understand.

Default to review-only. Do not modify product code, begin a refactor, create implementation tasks, or integrate changes until the review is complete and the user explicitly opts into a next phase.

## Keep findings about cognitive simplicity

The review asks what a developer must understand to explain or change the system. It does not ask for every way the system could be improved. Correctness and the other engineering qualities above constrain proposed simplifications; they are not additional audit tracks. Inspect them to understand why complexity exists and what must survive a change.

Admit a finding only when the evidence establishes all three:

1. **A concrete reader task:** for example, locate the retry policy owner, trace cancellation, or add one supported event type.
2. **An avoidable reasoning burden:** identify the competing owners, representations to reconcile, hidden transitions, misleading names, or unnecessary concepts the reader must reconstruct, with direct code evidence.
3. **A simpler after-state:** explain what the reader would no longer need to infer, compare, or keep in mind, while preserving required behavior and boundaries.

Apply this counterfactual: **if the behavior were correct and all tests passed, would this understanding problem still be worth reporting?** A bug may reveal split ownership or hidden state, but the finding must independently prove that cognitive problem. Calling a defect "confusing," "hard to maintain," or "error-prone" does not establish it.

Standalone bugs, missing validation, missing tests, missing features, optimization opportunities, dependency upgrades, and generic best-practice recommendations do not qualify. Neither does the mere presence of a code smell from the investigation checklist. Report fewer findings, including none, when no material cognitive burden is demonstrated.

If a pre-existing incidental defect creates a concrete serious risk or blocks a proposed simplification, briefly flag it as outside this review's scope, with evidence and uncertainty. Do not turn it into a scored finding, expand into a bug inventory, or add its repair to the recommendation plan. Pursue broader review or repair of unrelated defects only when it is also in the user's requested scope.

Apply this boundary to delegated review prompts and screen returned findings before adding them to the live review. Read the rubric's contrasting examples before admitting findings.

## Route model and reasoning practically

Use the latest suitable model available on the active Codex or Claude platform. Do not hard-code dated model names or assume one model family is always best.

Never request reasoning effort below `high`. Use `high` for focused, well-bounded reviews and implementation units; use a stronger practical setting for broad architecture, ambiguous ownership, concurrency, distributed state, persistence invariants, or consequential integration work. Reserve the maximum setting for a clearly exceptional problem where the additional reasoning cost is justified. If the platform expresses effort through thinking budgets rather than named levels, choose the closest equivalent.

Record material model or effort downgrades when the platform cannot honor the requested routing.

## Establish scope and authority

1. Resolve the exact target, reviewed ref or working tree, exclusions, requested depth, output location, and whether the system is pre-release. When the user supplies no narrower scope, use the current repository as the target and discover its meaningful review scopes from the code. Do not require the user to enumerate subsystems or choose reader tasks before starting. Infer the repository from the current workspace or conversation; ask for a target only if it remains ambiguous.
2. Read every applicable repository instruction file before judging or editing anything. Preserve unrelated and user-owned changes.
3. Keep the review inside the user's explicit scope or the repository-wide target established above. Inspect code outside that target only when needed to understand an inbound or outbound boundary, and label it as context rather than silently expanding the review.
4. Treat current source, executable tests, schemas, generated contracts, runtime composition, and observed behavior as primary evidence. Use documentation, plans, spikes, and ADRs to understand reasoning and intended boundaries, but do not infer current truth from prose alone. Respect any authority explicitly assigned by repository instructions or the user.
5. Treat compatibility, versioning, hardening, and guardrail concerns according to the user's context. Inspect them only to establish a cognitive burden or a guarantee a proposed simplification must preserve; their absence or imperfection alone is not a finding.

## Create and maintain the live review

Read [review-rubric.md](references/review-rubric.md) completely before creating the review document.

Create the review document early, after the first orientation pass. Use the user's requested location; otherwise follow an existing repository convention, or use a scoped `.review/` directory when no convention exists.

During the initial review, write findings while investigating. Mark uncertain items as hypotheses. As evidence changes, amend, merge, downgrade, or delete earlier findings instead of preserving a stale conclusion for appearances. Keep an explicit revision note when a materially important judgment changes. Once the review is finalized, preserve its baseline and append later evidence as described in the rubric.

Never hard-wrap prose merely to satisfy a column width. Preserve deliberate paragraph boundaries and follow any stricter repository Markdown convention.

## Perform the review

### 1. Build the reader's map

Inventory the target's entry points, public contracts, runtime composition, important services, domain concepts, persistence, state machines, external integrations, tests, and generated artifacts. Identify the likely "start here" path for a new developer.

Use this inventory to build a scope map covering all meaningful areas within the target: features, packages or services, shared infrastructure, and behavior spanning their boundaries. Discover scopes from actual responsibilities and execution paths rather than assuming directory names define them. Update the map as new areas emerge, and track the evidence examined and remaining gaps for each scope.

Derive representative reader tasks from the discovered scopes and their interactions. Use them to investigate each area, without treating the first few tasks as the limit of the review. Trace representative happy, failure, cancellation, and cleanup paths to establish what a reader must follow and where ownership becomes difficult to reconstruct. Prefer concrete call chains and state transitions over architecture labels. This is not an exhaustive search for failing cases.

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

Use available static metrics, dependency tools, searches, and test results when they help locate cognitive burden or verify a guarantee relevant to a proposed simplification. Record the exact command, tool, version when relevant, scope, and result.

Do not equate cognitive-complexity scores, LOC, file length, branch count, class count, import count, or layer count with design quality. A metric identifies where to read; the finding must explain the human reasoning cost and the guarantee any proposed simplification must preserve.

Do not install new tooling merely to manufacture a score when existing evidence is sufficient.

### 4. Prove findings

For every material finding, include:

- a concise title naming the cognitive burden and a priority based on its impact on reader tasks;
- affected files and lines where practical;
- direct code evidence, separated from inference;
- the reader task that becomes difficult;
- the accidental mental-model burden;
- why that burden remains worth addressing even assuming correct behavior;
- the correctness or quality constraints that must survive;
- the smallest coherent recommendation;
- dependencies, implementation risk, and validation evidence needed;
- confidence and any unresolved question.

Avoid findings that say only "large file," "too many classes," "needs cleanup," or "not best practice." Explain the specific competing owners, repeated decisions, misleading boundary, non-local state, or unnecessary concept.

### 5. Challenge the review

Before finalizing, actively try to disprove the highest-priority findings. Trace the supposedly redundant boundary from both sides, inspect transaction and failure semantics, compare tests, and check whether an apparent duplication protects a trust, lifecycle, persistence, or external-contract boundary.

Reapply the finding-admission criteria to every finding, including delegated ones. Remove standalone defects and general improvements rather than retrofitting a cognitive-simplicity justification onto them.

Revise the live document when later evidence changes the conclusion. Do not reward deletion that would make essential complexity implicit or weaken a guarantee.

### 6. Prioritize a coherent sequence

Rank recommendations by cognitive benefit, correctness risk, dependency order, and reviewability. For each recommendation, describe the intended after-state in plain language and the mental-model concepts it adds, removes, or consolidates.

Separate safe pruning and boundary clarification from high-risk state-machine, persistence, concurrency, or contract rewrites. Identify recommendations that should be deferred because the current implementation works and the rewrite risk exceeds the present cognitive benefit.

## Score the result

Record an assessment for every review angle using the rubric's statuses. Assign a score from 1 to 10 only when the angle applies and the evidence supports a defensible judgment, using the rubric anchors. Otherwise use the distinct `not assessed`, `insufficient evidence`, or `not applicable` status with a reason. Include evidence, confidence, and a realistic target where useful. Do not force a target of 10; a complex system can be excellent while retaining explicit essential complexity.

Scores describe how understandable the code is, not how correct, secure, fast, or feature-complete it is. Bugs, missing coverage, and absent hardening do not lower a score without independent evidence of a cognitive burden.

Include an overall score only after the per-angle assessments and only when material evidence gaps do not prevent a defensible overall judgment. Explain weighting and coverage rather than hiding them in an arithmetic average. Distinguish measured static complexity from the review's human-maintainability scores.

## Complete the review before implementation

At finalization, preserve the reviewed state, scope, findings, supporting evidence, per-angle assessments, and overall assessment as the baseline. Implementation adds progress and evidence without overwriting that baseline or assigning revised scores. Fresh scores belong in the separately approved re-review comparison.

Finish with:

- the live review path;
- scope coverage, exclusions, and any unreviewed areas with reasons; do not claim repository-wide completion while discovered scopes remain unreviewed;
- the current mental model in plain language;
- strongest qualities worth preserving;
- findings and prioritized recommendations;
- per-angle scores or unscored statuses and overall assessment;
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

After the user explicitly opts into implementation, read [implementation-workflow.md](references/implementation-workflow.md) completely before editing, creating implementation branches or worktrees, or dispatching work. Follow it for both implementation in the current task and orchestrated implementation. It contains the requirements for selected scope, dependencies, Git strategy, worker assignments, review, integration, monitoring, and completion reporting.

Implement only the selected recommendations and validate their intended after-state and protected guarantees. Implementation verification is part of this phase; a fresh scored re-review remains a separately approved phase.

## Re-review only after a separate opt-in

When the user opts into re-review, assess the integrated destination state as current evidence. Do not assume an integrated recommendation worked, award score increases for deleted code, or treat the implementation ledger as proof of improvement.

During re-review:

- Re-read applicable instructions and confirm the reviewed ref, scope, exclusions, and changes since the baseline review.
- Retrace the representative execution, state, failure, cancellation, cleanup, persistence, and composition paths affected by the work.
- Revisit every original finding and score, but also look for new indirection, split ownership, hidden guarantees, or complexity displaced into another module.
- Compare equivalent measurements only when their tool, configuration, and scope are sufficiently consistent; explain any non-comparable evidence.
- Classify each recommendation as effective, partially effective, ineffective, regressed, reverted, deferred, or no longer applicable, with code evidence.
- Verify that correctness and the protected engineering qualities remain intact using proportional tests and direct inspection.
- Append the re-review findings, outcomes, and assessments as a comparison with the original baseline. Preserve that baseline, record material reversals, and distinguish remaining essential complexity from accidental complexity.
- Reassess every investigated angle and the overall assessment using the rubric's evidence statuses and scoring anchors. Report unchanged or lower scores honestly when the integrated result does not justify improvement; do not invent scores where evidence is insufficient.

Finish with a concise before-and-after comparison, the strongest improvements, quality guarantees verified, remaining or newly introduced problems, residual risks, and any repository guidance worth adding to prevent regression. Further implementation still requires another explicit user opt-in.
