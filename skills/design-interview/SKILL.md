---
name: design-interview
description: "Relentlessly examine a plan, proposal, or design until its decisions, dependencies, assumptions, and terminology have been explored and the user confirms a shared understanding. Use when the user wants an exhaustive interview before committing to an idea. Maintain decision records and a domain glossary during the conversation as an integral part of the interview."
---

# Design Interview

Keep questioning the design until every in-scope decision and its consequences have been examined. Challenge weak reasoning, vague terms, unexplained preferences, and answers that conflict with earlier agreements. Make each challenge concrete and useful. A plausible plan or polished set of documents is not evidence that the interview is complete.

## Document agreements throughout the interview

Use the plan and constraints already in the conversation. If the subject is missing, ask for it briefly; do not start a generic questionnaire.

Read [Keeping project docs](references/keeping-project-docs.md) at the start of every interview. Maintain a glossary of agreed terms and short architecture decision records (ADRs) for significant choices as agreements emerge. Documentation is a required part of the workflow; do not ask whether to enable it or offer a conversation-only mode.

Follow the project's existing documentation conventions and save updates during the discussion. Create documents when there is meaningful content, and use the reference's criteria to decide which choices warrant an ADR. Maintaining these documents does not require a separate permission check for each edit and does not authorize product changes.

Use an available interactive question tool when the current mode supports it; otherwise ask in chat.

## Keep track of what the design rests on

Before the first design question, survey the whole agreed scope and identify the decisions it entails from the proposal and available evidence. Maintain that inventory alongside the intended outcome and scope. Track what each decision depends on, what has been agreed, what remains unanswered, and what is waiting for evidence. Include the further questions created by each answer. Distinguish evidence about today's system from proposals about its future. Keep the working inventory in the conversation and persist agreed terms and significant decisions in the project docs; do not create a separate tracking artifact by default.

When a question depends on facts available from code, existing docs, tests, or other accessible sources, assign that investigation to a subagent. If delegation is unavailable, investigate directly. Do not ask the user to perform research you can do yourself. Keep questions that depend on missing evidence open while continuing with independent questions at the pace below. Ask the user only for facts that remain inaccessible, as well as goals, constraints, and decisions that require their judgment.

When code, documentation, and the user's description disagree, show the specific mismatch and establish whether the design changes existing behavior or needs correcting. Do not silently treat either the implementation or the proposal as the intended future behavior.

## Make questions and options understandable without technical knowledge

Write every user-facing question, option label, and option description for a person with no technical background. Use familiar words and a respectful, direct tone. The user must have enough context to reason about the choice rather than rely on your recommendation without understanding it.

Explain the concrete situation, the decision being made, and why it matters to the user or the people affected. Describe observable behavior before implementation details. Avoid unexplained jargon, acronyms, and code identifiers; when a technical term is necessary, explain its meaning in ordinary language where it appears. Use a small example or a familiar analogy when it clarifies the choice without hiding important limitations.

For every available option:

- Give it a descriptive, plain-language label rather than only a technical name.
- Explain what would happen if selected, its practical benefit, and its main downside or cost. Include material risks and uncertainty where relevant.
- Compare it with the other options using the same situation and a similar level of detail, so the user can see what changes and when each choice makes sense.

Explain the options before asking the user to select one. If the question interface limits label length, put the necessary explanation in the option description or the question's surrounding text; short labels must not remove the information needed to decide. Make the recommendation clear while presenting the alternatives fairly, including the recommended option's drawbacks.

Before presenting a question, check whether a non-technical reader could explain what they are deciding, how the options differ, and what accepting each one would mean. If not, rewrite it. If the user shows confusion, clarify with a concrete example before treating their response as a settled decision.

## Ask one question at a time, with optional independent batches

Identify which unanswered questions have their prerequisite decisions and evidence settled. Default to asking one focused question and waiting for the answer. You may present a short batch when the questions are independent and easy to answer together, but having several ready questions does not require batching. Number batched questions so the user can answer each separately. Keep every other ready question in the pending inventory; asking fewer questions per turn does not reduce the required coverage.

Never batch questions when an answer to one could change how another is asked or what answer you would recommend. Wait for the user's answers before advancing dependent decisions. Partial answers settle only the questions actually answered; carry the others forward. After each response, update dependencies, revisit conclusions affected by changed answers, remove questions made irrelevant, and uncover the next answerable questions. Check the rest of the agreed scope for omissions as well as following the latest answer's consequences; the current inventory is not proof of complete coverage.

For each question:

- Explain the decision and why its answer matters.
- Include a recommended answer, its rationale, and the main trade-off. Explain credible alternatives where they exist. If the design choice needs more evidence, recommend a specific investigation and leave the choice open until that evidence is available; do not invent confidence.
- Phrase the question so a simple affirmative answer accepts the stated recommendation. Make the recommendation's scope unambiguous.
- Let the user propose a different answer. Avoid presenting your preferred option as already decided.
- Wait for the answer before making dependent decisions. A default selection, silence, or your own recommendation is not agreement.

Probe the proposal through situations it must handle. Explore who benefits, how success is recognized, what is excluded, ownership, information flow, failure and recovery, operating constraints, and the cost of changing course wherever they apply. Follow each answer into its consequences and boundary cases. Cover the whole agreed scope rather than treating a few representative questions as sufficient. Do not invent unrelated requirements or expand the scope without the user.

Pay attention to ambiguous nouns and verbs. Test their meaning with a small example and a counterexample, then record and use the agreed vocabulary consistently. Distinguish concepts that happen to share a label; different parts of a system may legitimately use different meanings.

If an answer stays vague, narrow the question to a concrete case, expose the trade-off, or bring more evidence. Change the approach rather than repeat the same wording. An unknown answer calls for investigation or a more precise question; labeling it an assumption or postponing it does not resolve it. Examine any proposed assumption's consequences and distinguish a user-approved constraint or accepted risk from an unverified claim about the system. Deferrals remain open unless the user explicitly removes the decision from scope after understanding the implications.

## Establish completion explicitly

Before declaring the design covered, check that every in-scope line of inquiry has been explored, its decisions are settled, no prerequisite research remains outstanding, and no ambiguity, contradiction, or unsupported assumption is being treated as an answer. Having no immediately answerable questions is not completion when other questions are blocked or deferred. Continue until those gaps are resolved.

If the user asks to stop, or a user-set time limit is reached, honor it. Provide an explicitly incomplete handoff listing the unresolved decisions, missing evidence, and consequences of stopping there. Do not lower the completion standard or quietly drop questions to fit the available time.

Once coverage is complete, check that agreed terms and qualifying decisions have been saved and that the documents reflect the latest answers. Give a concise account of the resulting plan, major choices and reasons, agreed constraints and accepted risks, and any explicit scope exclusions. Link the documents created or updated and report any unsaved content as incomplete documentation. Ask the user to confirm that this accurately represents their intent and the shared understanding. If they correct it, reopen the affected questions, work through their dependencies, and update the documents before seeking confirmation again.

Require that confirmation before starting implementation or other follow-on work, even if the user previously authorized a next step. Maintaining the interview documents remains part of the work throughout. Confirmation establishes agreement on the design; it does not itself authorize additional work. After confirmation, follow any explicit next-step authorization already given by the user.
