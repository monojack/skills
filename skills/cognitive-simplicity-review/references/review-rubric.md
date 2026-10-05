# Cognitive simplicity review rubric

## Contents

- [Finding admission examples](#finding-admission-examples)
- [Scoring anchors](#scoring-anchors)
- [Minimum review angles](#minimum-review-angles)
- [Interpreting complexity evidence](#interpreting-complexity-evidence)
- [Finding priorities](#finding-priorities)
- [Live review structure](#live-review-structure)
- [Recommendation and implementation ledger](#recommendation-and-implementation-ledger)

## Finding admission examples

Apply the reader-task, avoidable-burden, and simpler-after-state criteria in `SKILL.md` before assigning priority or score. These pairs illustrate the evidence required; the cognitive finding is not implied by the defect in the left column.

| Does not qualify by itself | Qualifies when supported by code evidence |
| --- | --- |
| A retry loop makes one extra attempt. | Changing the retry limit requires reconciling three independently maintained policy owners; one owner would remove that repeated reasoning even if all three currently agreed. |
| An error path forgets to release a resource. | Ownership of release is split across callbacks and flags, so explaining who closes the resource requires reconstructing several lifetimes. A localized owner would make the existing cleanup contract explicit. |
| An endpoint lacks input validation. | Equivalent request types are repeatedly converted without any semantic or trust-boundary difference; a reader must compare them to discover that they mean the same thing. |
| A test case is missing. | Existing fixtures reconstruct private production wiring, so understanding one behavior requires learning a second implementation of the same composition rules. |
| A query could be faster, a library newer, or code more idiomatic. | A wrapper changes familiar framework semantics without expressing a product rule, forcing readers to learn and trace an unnecessary second API. |
| A function is long or a service has only one implementation. | A routine change crosses pass-through layers whose only effect is to rename the same data, with no independent policy, lifecycle, or contract to explain the extra steps. |

A short, readable function with an incorrect comparison can have high cognitive-simplicity scores. A fully tested, behaviorally correct flow can score poorly when a reader must reconcile unnecessary owners or representations. Neither defect count nor test success determines these scores. It is valid to report no cognitive-simplicity findings.

## Scoring anchors

Score the code a reader encounters now, not the architecture described in prose. Use the full 1–10 range, but do not manufacture precision beyond the evidence.

Record an assessment status for each angle before assigning a score:

| Status | Meaning | Numeric score |
| --- | --- | --- |
| `scored` | The angle applies and the examined evidence supports a defensible judgment. | 1–10 |
| `not assessed` | The angle applies, but it has not been investigated. | — |
| `insufficient evidence` | The angle was investigated, but the available evidence does not support a defensible score. | — |
| `not applicable` | The concern does not exist within the review target. | — |

Give a reason for every unscored status. For `not assessed` and `insufficient evidence`, identify the missing investigation or evidence; these are coverage gaps, not substitutes for reviewing applicable scopes. Never encode an unscored status as zero or silently count it as satisfactory. Withhold the overall numeric score when material gaps prevent a representative assessment, and record the appropriate unscored status instead.

| Score | Meaning |
| --- | --- |
| 1 | The behavior is effectively unreconstructable without specialist or historical knowledge; competing owners or hidden state make safe change extremely difficult. |
| 2 | Severe cognitive breakdown; ordinary work requires broad reconstruction and carries a high chance of changing the wrong path. |
| 3 | Poor; important behavior is discoverable only through extensive cross-file tracing, private knowledge, or comparison of parallel representations. |
| 4 | Weak; ownership exists but is obscured by substantial accidental indirection, duplication, or misleading boundaries. |
| 5 | Mixed; a developer can work successfully, but common changes require unnecessary context and repeated verification. |
| 6 | Adequate; the main model is understandable, with several localized areas of avoidable complexity. |
| 7 | Good; responsibilities and paths are mostly clear, and remaining friction is bounded or tied to real guarantees. |
| 8 | Strong; a newcomer can navigate representative work with little unrelated context, and essential complexity is explicit and localized. |
| 9 | Excellent; ownership, state, contracts, and dependency direction consistently support local reasoning, with only minor residual friction. |
| 10 | Exceptional for the problem's inherent complexity; the mental model is both minimal and complete, and further simplification would likely hide or weaken a real guarantee. |

Use a realistic target for each angle. Treat 8 as an excellent default target for a non-trivial production subsystem; reserve 9–10 for unusually clear implementations with strong evidence.

## Minimum review angles

Investigate every angle that exists in the target. Use the assessment statuses above to distinguish missing investigation, insufficient evidence, and concerns that do not apply. Add target-specific angles when they materially affect the reader's mental model. Each angle measures the cost of understanding the relevant code; it does not authorize a separate correctness, security, performance, coverage, or framework-compliance audit.

| Angle | Core question | Useful evidence |
| --- | --- | --- |
| Start-here navigability | Can a new developer find the right entry point and first owner without repository archaeology? | source maps, package layout, entry points, naming, local docs |
| Local readability | Can a function or module be understood with nearby context and truthful names? | representative functions, branch structure, mutation, error flow |
| Responsibility ownership | Does each important behavior, rule, and fact have one discoverable owner? | duplicate policies, god objects, scattered decisions, unclear service boundaries |
| Execution-path traceability | Can a reader follow a normal request, event, job, or tool call end to end without choosing between parallel paths? | concrete call chains, handler registration, runtime binding, forwarding hops |
| Dependency direction | Do imports and runtime dependencies flow in an explainable direction without cycles or back-reading? | import graph, composition root, callbacks, service locators, cross-layer imports |
| Abstraction and indirection value | Does every abstraction remove more concepts or duplicated decisions than it adds? | pass-through methods, one-implementation interfaces, factories, registries, reflection |
| Contract and representation economy | Does each DTO, event, schema, or projection represent a genuine semantic or trust-boundary difference? | conversion chains, model dumps/revalidation, aliases, wire/application/persistence models |
| State and lifecycle intelligibility | Are states, transitions, invariants, terminal outcomes, cancellation, and cleanup owned explicitly? | flags, sets, queues, tasks, reducers, state machines, callbacks, race tests |
| Persistence and transaction clarity | Are durable facts and atomic operations grouped by cohesive capability rather than god object or table-shaped ceremony? | ports, adapters, SQL sessions, lock order, cross-table invariants, repositories |
| Runtime composition and resource lifetime | Can a reader see what is constructed, shared, and closed without treating the composition root as product logic? | app bootstrap, dependency injection, lifespan, clients, background resources |
| Duplication, dead code, and speculative surface | Has obsolete, parallel, dormant, or hypothetical machinery been removed? | unused exports, compatibility paths, feature flags, capability matrices, stale migrations |
| Test comprehensibility | Do tests explain public behavior and stable seams without rebuilding or mutating private internals? | fixtures, fakes, private attributes, end-to-end flow tests, invariant tests |
| Framework and language fit | Does framework ceremony help expose the product behavior rather than obscure it? | official idioms, dependency patterns, validation, streaming, error mapping |
| Junior-developer changeability | Could a junior developer identify the correct change location, likely tests, and affected boundaries with bounded guidance? | simulate a likely change, count concepts and owners, identify false leads |
| Overall cognitive simplicity | Is the system's total mental model the smallest accurate one that preserves its guarantees? | weighted synthesis of the angles above, not a blind mean |

## Interpreting complexity evidence

Separate three kinds of evidence:

- Direct observation: what the code, tests, schema, runtime, or command output shows.
- Inference: the consequence for a concrete reader task, including what must be inferred, compared, or held in mind.
- Recommendation: the proposed change and why it should reduce total mental cost.

Treat quantitative measures only as locators:

- Cognitive-complexity or cyclomatic-complexity scores can reveal dense control flow, but not whether boundaries are truthful.
- LOC and file length can reveal concentration, but large cohesive transaction logic may be clearer than fragmented repositories.
- Import cycles can reveal confused ownership, but an acyclic graph can still contain excessive forwarding.
- Type or DTO counts can reveal representation proliferation, but boundary-specific models may protect real trust or durability semantics.
- Test counts and coverage can reveal exercised surfaces, but private-state tests may reinforce the wrong mental model.

When reporting a static metric, distinguish it from the 1–10 human-maintainability score and state the measured scope. Never set a recommendation target solely from a generic threshold.

## Finding priorities

Prioritize admitted findings by the scope, frequency, and severity of the demonstrated reasoning burden on human change. Defect severity, hypothetical incidents, and aesthetic dislike do not establish cognitive priority.

| Priority | Meaning |
| --- | --- |
| P0 | An opaque or contradictory mental model blocks an urgent necessary change: even the relevant owner or invariant cannot be established. Exceptional; a severe defect alone does not qualify. |
| P1 | Routine work on a central path requires broad reconstruction across competing owners, hidden state, or parallel representations; the burden substantially obstructs understanding or change. |
| P2 | Material accidental complexity affects common changes, onboarding, or multiple features; plan a coherent correction. |
| P3 | Localized friction or cleanup with bounded impact; fix opportunistically or combine with related work. |
| Observation | Useful context, strength, or hypothesis that does not yet justify a change. |

For every recommendation, state rewrite risk separately from finding priority. A P1 cognitive problem can still justify deferral when the existing code is behaviorally mature and a rewrite would endanger critical concurrency or transaction guarantees without sufficient tests.

## Live review structure

Use this structure as a starting point and adapt it to the target:

```markdown
# <Target> cognitive simplicity review

## Review status

- Scope:
- Reviewed state/ref:
- Exclusions:
- Live status:
- Last materially revised:
- Baseline finalized at:

## Executive summary

## Current system map

## Representative reader journeys

## Evidence and commands

## Findings

### <Finding ID and title>

- Priority:
- Confidence:
- Evidence:
- Reader task made difficult:
- Mental-model burden:
- Why this matters even assuming correct behavior:
- Guarantees to preserve:
- Recommendation:
- Dependencies and risk:
- Validation needed:
- Review status: hypothesis | confirmed | revised | removed | deferred

## Strengths to preserve

## Prioritized recommendations and dependency graph

## Scores

| Angle | Assessment status | Score | Realistic target | Confidence | Evidence summary or gap |
| --- | --- | ---: | ---: | --- | --- |

## Revisions to earlier judgments

## Evidence gaps and residual risk

## Optional next phase
```

During the initial review, keep removed or reversed high-impact judgments in `Revisions to earlier judgments` with a short reason. Delete trivial abandoned notes rather than turning the review into an archaeological log.

At finalization, date and preserve the reviewed state/ref, scope and exclusions, findings and evidence, assessment statuses, scores, and overall assessment as the baseline. Later implementation progress, confirmations, contradictions, and factual corrections belong in dated additions linked to the original finding or angle. Keep the baseline readable and unchanged; record fresh assessments only in the separately approved re-review addendum.

## Recommendation and implementation ledger

When the user opts into implementation, add a ledger to the live review:

| Unit | Recommendation | Prerequisites | Risk | Owner/task | State | Commits | Validation | Integration result |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |

Use states such as `planned`, `active`, `reviewing`, `merged`, `blocked`, and `deferred`. Record progress in this ledger and append dated evidence linked to the relevant finding IDs. Distinguish worker-reported results from evidence verified in the integrated destination state. Implementation updates must not overwrite baseline findings, assessment statuses, or scores, or assign revised scores; those belong to the separately approved re-review.

## Opt-in re-review addendum

Add this only when the user separately opts into the re-review phase:

```markdown
## Re-review

- Re-reviewed ref and date:
- Baseline review ref:
- Integrated change set:
- Scope or evidence differences:

| Angle | Baseline status | Baseline score | Re-review status | Re-review score | Evidence and comparability |
| --- | --- | ---: | --- | ---: | --- |

### Recommendation outcomes

### Guarantees re-verified

### Remaining, displaced, or new complexity

### Re-review conclusion
```

Do not overwrite baseline findings, statuses, or scores. If either assessment is unscored or the scopes are not comparable, explain the limitation rather than claiming a numeric improvement. Newly available evidence can justify a first score without demonstrating that the code improved. A re-review is an evidence comparison, not a completion ceremony; unchanged or lower scores are valid outcomes.
