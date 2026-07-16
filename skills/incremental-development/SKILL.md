---
name: incremental-development
description: Deliver software changes as small, focused, review-gated, and bisectable commits. Use when the user asks to work incrementally, keep diffs or commits small, implement one logical unit at a time, pause for review, require approval before committing, or avoid mixing unrelated changes.
---

# Incremental Development

Work collaboratively in the smallest meaningful units. Keep every unit independently reviewable, validated, and safe to commit.

## Apply the gates

Treat review and commit approval as separate from general permission to work.

- Never create a commit before presenting the completed unit and receiving explicit approval to commit it.
- Do not treat approval of a plan, permission to edit, or a request to continue as approval to commit.
- After an approved commit, pause before starting the next unit unless the user explicitly requested uninterrupted execution.
- Even during uninterrupted execution, stop for review and explicit commit approval before every commit.
- Never amend a commit unless the user explicitly requests it.
- Never force-push unless the user explicitly requests it. Prefer `--force-with-lease` when force-pushing is authorized.
- Do not push unless the user requested or otherwise authorized pushing.

## Work one logical unit at a time

1. Understand the requested outcome and inspect the relevant repository instructions and current worktree state.
2. Identify the smallest meaningful step that produces a coherent, reviewable result.
3. State the current unit briefly before editing when the scope is not obvious.
4. Implement only that unit. Keep unrelated fixes, refactors, formatting, and generated changes out of the diff.
5. Validate the unit with the repository's required checks and the narrowest relevant tests.
6. Review the resulting diff for scope, correctness, accidental files, secrets, and unrelated user changes.
7. Present the unit for review and stop. Do not stage or commit it yet unless repository policy requires pre-review staging.
8. Apply requested corrections to the same unit, rerun validation, and present the updated result again.
9. After explicit commit approval, stage only the approved files or hunks, inspect the staged diff, and create one meaningful commit.
10. Report the commit hash and subject. Then pause before the next unit unless uninterrupted execution was explicitly requested.

Do not fragment work into changes that have no useful behavior or review value on their own. A trivial task may be one unit.

## Choose the unit

- Prefer a thin vertical slice that produces a complete, observable outcome across the required layers.
- Work risk-first when a major uncertainty could invalidate later units.
- Work contract-first when independently implemented components need a stable boundary.
- Choose the simplest implementation that completes the current unit. Avoid abstractions for hypothetical future needs.

## Keep units focused

- Give each unit one objective that can be described in a short sentence.
- Preserve unrelated user changes in a dirty worktree.
- Separate discovered improvements into proposed follow-up units instead of silently adding them.
- Include tests and documentation needed to make the current behavior complete. Do not defer required coverage merely to shrink the diff.
- Keep each commit bisectable. The repository should build and relevant tests should pass at that commit.
- Keep incomplete behavior disabled with safe defaults or feature flags when a partial unit could be deployed or merged.
- Prefer additive, independently revertible changes. Include rollback paths for migrations.
- Do not repeat successful validation unless relevant files or configuration changed.
- If required validation cannot pass or run, report the blocker and do not request commit approval as though the unit were complete.

## Present a unit for review

Before asking for approval, show:

- the objective completed;
- a concise explanation of what changed and why;
- `git diff --stat` output, or an equivalent file-level summary when Git is unavailable;
- validation commands and their actual results;
- relevant trade-offs, decisions, or known limitations;
- a clearly separated list of follow-up units, when any were discovered.

Ask for explicit approval to commit the presented diff. Do not begin the next unit while waiting.

For the final unit, also run the full relevant test suite and verify the complete outcome end to end before requesting commit approval.

## Create the approved commit

- Recheck the worktree before staging in case files changed during review.
- Stage only the reviewed unit. Prefer exact paths or hunks over broad staging commands.
- Inspect `git diff --cached --stat` and `git diff --cached` before committing.
- Follow the repository's commit convention. Use a clear imperative subject and Conventional Commits when no stronger local convention exists.
- Do not include newly changed or unrelated files without presenting them for review first.
- If the staged diff differs materially from the approved diff, stop and request another review.

After the commit succeeds, report its hash and subject. Start the next planned unit only when the applicable continuation gate allows it.
