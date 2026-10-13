# Keeping project docs

Apply this guidance throughout every interview. Write small, accurate updates as terms and decisions become agreed. Do not wait until the end to reconstruct the reasoning or ask the user to opt into documentation.

## Find the existing home

Check repository instructions and existing glossary and decision records first. Reuse their locations, templates, numbering, and scope. Follow an existing glossary map when the project separates vocabulary by business context; do not merge distinct meanings into one global definition.

If there is no convention, use `docs/glossary.md` and `docs/decisions/NNNN-short-topic.md`, starting at the next unused number. Create a file or directory only when it has real content. If no project or writable destination is available, ask where the documents belong and continue the interview without writing to an arbitrary location. Report any write failure and keep the affected draft in chat until it can be saved.

## Capture vocabulary

Keep each entry about the business concept, with enough detail to distinguish it from nearby concepts:

- The chosen term and a concise definition within its context.
- An example or boundary when the meaning would otherwise be ambiguous.
- Alternative labels to avoid, when those labels caused confusion.

Exclude implementation choices, task lists, and unresolved speculation. A database table name need not become a business term. If a proposed definition conflicts with the existing glossary, resolve the conflict with the user before editing it. Preserve unrelated entries and use the new wording consistently in subsequent questions.

## Record decisions worth explaining later

Create an ADR when a meaningful choice would be costly to revisit, had credible alternatives, and would be difficult for a future reader to understand without its rationale. Routine preferences and readily reversible details can remain in the conversation. Do not produce one record per interview answer.

Follow the project's template. Otherwise use a short record containing:

- **Title, date, and status:** what was decided and whether it is accepted or superseded.
- **Problem and constraints:** the situation that made the choice necessary.
- **Decision and reasons:** the agreed direction, including why the relevant alternatives were rejected.
- **Consequences:** the costs, limitations, and follow-up implications the user accepted.

State whether a record describes intended behavior or something already implemented. Never mark an unanswered proposal as accepted, invent alternatives that were not considered, or fill gaps in rationale with guesses. If the user has explicitly accepted a working assumption, label it as an assumption rather than an observed fact and identify any outstanding verification. Recording it does not settle the unresolved interview question.

Update a record while the current discussion is still refining it. If a later choice replaces an established decision, retain the original reasoning and link the successor according to the repository's convention. Resolve numbering collisions without overwriting another record.

## Keep the conversation and files aligned

After a substantive agreement, save the relevant entry or decision and briefly mention the change without turning the interview into a file-editing log. Update affected documents when the user revises an answer. Check for contradictory wording and broken local links before wrapping up.

When continuing an existing interview, capture prior explicit agreements from the conversation without promoting earlier suggestions into decisions. At the end, report the actual saved paths and any remaining documentation gaps. If required content could not be saved, documentation remains incomplete even when the design questions are settled.
