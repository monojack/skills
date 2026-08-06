# Cognitive simplicity review rubric

## Contents

- [Scoring anchors](#scoring-anchors)
- [Minimum review angles](#minimum-review-angles)
- [Interpreting complexity evidence](#interpreting-complexity-evidence)
- [Finding priorities](#finding-priorities)
- [Live review structure](#live-review-structure)
- [Recommendation and implementation ledger](#recommendation-and-implementation-ledger)

## Scoring anchors

Score the code a reader encounters now, not the architecture described in prose. Use the full 1–10 range, but do not manufacture precision beyond the evidence.

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

Investigate every angle that exists in the target. Mark an angle `not applicable` rather than inventing a score. Add target-specific angles when they materially affect the reader's mental model.

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
- Inference: the likely human or operational consequence of that observation.
- Recommendation: the proposed change and why it should reduce total mental cost.

Treat quantitative measures only as locators:

- Cognitive-complexity or cyclomatic-complexity scores can reveal dense control flow, but not whether boundaries are truthful.
- LOC and file length can reveal concentration, but large cohesive transaction logic may be clearer than fragmented repositories.
- Import cycles can reveal confused ownership, but an acyclic graph can still contain excessive forwarding.
- Type or DTO counts can reveal representation proliferation, but boundary-specific models may protect real trust or durability semantics.
- Test counts and coverage can reveal exercised surfaces, but private-state tests may reinforce the wrong mental model.

When reporting a static metric, distinguish it from the 1–10 human-maintainability score and state the measured scope. Never set a recommendation target solely from a generic threshold.

## Finding priorities

Prioritize by impact on safe human change, not by aesthetic dislike.

| Priority | Meaning |
| --- | --- |
| P0 | The mental model actively causes catastrophic or irreversible correctness risk; address immediately. Rare in a maintainability-focused review. |
| P1 | A central path or state model is dangerously difficult to reason about and likely to cause serious defects or stalled development. |
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
- Guarantees to preserve:
- Recommendation:
- Dependencies and risk:
- Validation needed:
- Status: hypothesis | confirmed | revised | removed | implemented | deferred

## Strengths to preserve

## Prioritized recommendations and dependency graph

## Scores

| Angle | Current | Realistic target | Confidence | Evidence summary |
| --- | ---: | ---: | --- | --- |

## Revisions to earlier judgments

## Evidence gaps and residual risk

## Optional next phase
```

Keep removed or reversed high-impact judgments in `Revisions to earlier judgments` with a short reason. Delete trivial abandoned notes rather than turning the review into an archaeological log.

## Recommendation and implementation ledger

When the user opts into implementation, add a ledger to the live review:

| Unit | Recommendation | Prerequisites | Risk | Owner/task | State | Commits | Validation | Integration result |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |

Use states such as `planned`, `active`, `reviewing`, `merged`, `blocked`, and `deferred`. Update a recommendation's evidence and score only after reviewing the integrated destination state, not merely after a worker reports completion.

## Opt-in re-review addendum

Add this only when the user separately opts into the re-review phase:

```markdown
## Re-review

- Re-reviewed ref and date:
- Baseline review ref:
- Integrated change set:
- Scope or evidence differences:

| Angle | Before | After | Change justified by |
| --- | ---: | ---: | --- |

### Recommendation outcomes

### Guarantees re-verified

### Remaining, displaced, or new complexity

### Re-review conclusion
```

Do not overwrite the baseline scores. A re-review is an evidence comparison, not a completion ceremony; unchanged or lower scores are valid outcomes.
