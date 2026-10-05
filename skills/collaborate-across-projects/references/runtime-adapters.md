# Runtime Adapters

Use this reference to open and continue one real conversation rooted in the target project. Discover the current runtime's capabilities instead of relying on remembered product-specific commands.

## Preserve runtime affinity

Select the adapter matching the current conversation surface before considering any other adapter:

| Current surface | Required first choice |
| --- | --- |
| Desktop application | Native project and conversation lifecycle capabilities; create a visible conversation in the same application |
| CLI | The same CLI runtime's new-session and exact-resume capabilities |
| Web, IDE, or hosted platform | That platform's native project selection and conversation lifecycle capabilities |
| Other runtime | That runtime's native equivalent |

Detect the current surface from runtime metadata and available capabilities, not from which unrelated executables or applications happen to be installed. Do not probe or select a CLI while a desktop application's native capabilities can satisfy the collaboration. Do not switch from a CLI to a desktop application merely because it is available.

Fall back across runtimes only when the matching adapter fails the adapter contract below. Record the exact missing capability, announce the fallback and destination runtime before launching it, and include the downgrade in the final report. An unavailable target project may justify a fallback; familiarity with another runtime does not.

## Adapter contract

Accept an adapter only when it can provide every capability required for the selected collaboration mode:

| Capability | Required proof |
| --- | --- |
| Target context | Report and verify the canonical target project root |
| New conversation | Return a unique conversation identity |
| Exact continuation | Send another turn to that exact identity |
| Observable completion | Distinguish running, completed, blocked, and failed |
| Structured result | Preserve the final response without terminal scraping when possible |
| Permission boundary | Show effective read/write, execution, approval, and network authority |
| Lifecycle | Observe, wait, interrupt, and preserve useful state without selecting an unrelated conversation |
| Telemetry | Capture actual model, reasoning, usage, cost, and duration when exposed |

Require write-root enforcement as well when the counterpart will implement. Read-only discovery does not require a write-capable adapter.

## Desktop application adapter

When the current conversation runs in a desktop application, use that application's native project and conversation capabilities.

1. Discover capabilities equivalent to listing projects, creating a conversation, sending a follow-up, inspecting state, waiting for completion, and interrupting execution.
2. Resolve the operator-specified target to the application's exact saved-project identity. Verify the canonical root, repository status, and execution host rather than matching only a display name.
3. Create one new project conversation because the operator explicitly requested cross-project collaboration. Follow the application's current local-checkout versus isolated-workspace rules and the operator's explicit preference.
4. Record the stable conversation identity and any host or workspace identity. If setup returns only a pending operation identity, wait for the stable conversation identity before attempting continuation.
5. Send every round to that exact conversation and use bounded waits for progress. Do not use a shell or CLI as the message bridge.
6. Leave approval and operator-input requests visible; never answer them by guessing.
7. Do not create replacement conversations merely because a round is slow. Start over only when identity, project, or trust boundary changes.

Tell the operator which visible desktop conversation is the counterpart. Use another adapter only if the application cannot resolve or bind the requested target project, cannot create or exactly continue a conversation, or cannot enforce the required permission boundary; announce that fallback before launching it.

## CLI adapter

Use this adapter first only when the current conversation itself runs in a CLI, or as an announced fallback when the current runtime cannot satisfy the adapter contract.

Discover support with harmless version and help commands. Verify, rather than assume:

- how to set and verify the target working directory;
- how to start a non-interactive conversation and obtain machine-readable output;
- which structured result contains the new conversation identity and terminal status;
- how to continue by the exact conversation identity;
- effective file, command, network, approval, model, and reasoning controls;
- configuration layers, project instructions, extensions, child-agent behavior, and usage telemetry.

Launch the process from the canonical target root even when a directory option also exists. Capture identity from structured runtime output, never from model prose. Never use an implicit recent-session selector.

Start read-only and non-interactive. Do not use full-access or approval-bypass modes. Before granting target writes, verify effective containment around the target root or keep the counterpart read-only and broker a serialized patch handoff.

## Other runtime adapter

For a web application, IDE integration, hosted orchestrator, or another platform, build the adapter from semantics rather than brand-specific guesses:

1. Discover capabilities equivalent to create, continue, inspect, wait, interrupt, and obtain a final result.
2. Start a new conversation with explicit target-project context.
3. Obtain a stable conversation identity from the runtime, not from parsing model prose.
4. Continue only that identity for every round.
5. Preserve structured responses and terminal status.
6. Verify project and permission boundaries on every material mode change.

Reject fire-and-forget commands, stateless one-shot prompts, and runtimes that can continue only “the most recent” conversation. A pair of unrelated one-shot calls is not a dialogue.

## Invocation hygiene

- Construct argument arrays through a process API when possible; send dynamic collaboration capsules on standard input or another data channel, never as shell source.
- Use the runtime's normal configured credential chain. Missing environment variables alone do not prove authentication failure.
- Do not print configuration files or secret-bearing environment values while checking readiness.
- Do not install, upgrade, initiate login, or mutate a live system during discovery.
- Bound waits and send concise progress updates during long turns.
- Retry one transient failure at most once. Preserve the conversation identity and partial evidence.
- Treat a successful process exit as necessary but insufficient; verify the structured terminal result and target-project evidence.

## Downgrade and failure handling

If exact continuation fails, stop calling the exchange a collaboration. Preserve the accepted evidence, report the broken conversation, and either start a new explicitly identified collaboration with the operator's knowledge or continue independently.

If only read-only operation is safe, complete discovery and the agreement, then provide the target-side patch or change plan for a serialized handoff. If no real target-root conversation can be opened, report `counterpart not run` and the missing adapter capability.
