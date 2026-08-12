# Runtime Adapters

Use this reference to open and continue one real conversation rooted in the target project. Treat command spellings as runtime-dependent; discover current support from read-only help before compiling an invocation.

## Adapter contract

Accept an adapter only when it can provide every capability required for the selected collaboration mode:

| Capability | Required proof |
| --- | --- |
| Target context | Report and verify the canonical target project root |
| New conversation | Return a unique task, thread, or session ID |
| Exact continuation | Send another turn to that exact ID |
| Observable completion | Distinguish running, completed, blocked, and failed |
| Structured result | Preserve the final response without terminal scraping when possible |
| Permission boundary | Show effective read/write, tool, approval, and network authority |
| Lifecycle | Wait, interrupt, and preserve useful state without selecting an unrelated session |
| Telemetry | Capture actual model, reasoning, usage, cost, and duration when exposed |

Require write-root enforcement as well when the counterpart will implement. Read-only discovery does not require a write-capable adapter.

## Native task or conversation API

Prefer a native API when it exposes tools equivalent to create, send, read, wait, interrupt, and identify the target project.

1. Discover the available task/thread tools and their schemas.
2. Verify that creation can bind to the exact target project, workspace, or worktree. A display title alone is not proof.
3. Create one task because the operator explicitly requested cross-project collaboration.
4. Record the returned task/thread ID and host/workspace identity.
5. Send follow-up rounds to that exact task and use bounded waits for progress.
6. Leave approval and user-input requests visible for the operator; never answer them by guessing.
7. Do not create replacement tasks merely because a round is slow. Start over only when identity, project, or trust boundary changes.

If the API creates a user-owned task visible in a sidebar, tell the operator which task is the counterpart. If the API cannot bind the requested target project, use a CLI adapter instead.

## Codex CLI

Discover support with harmless commands such as `codex --help`, `codex exec --help`, and the exact resume subcommand’s help. Verify, rather than assume:

- how to set the target working directory;
- non-interactive execution and machine-readable event output;
- the event carrying the new conversation ID and terminal result;
- continuation by exact ID;
- sandbox, approval, tool, model, and reasoning controls;
- effective configuration, project instructions, and usage telemetry.

For known modern CLIs, the relevant family is commonly `codex exec` with JSON events and `codex exec resume <exact-id>`, but use only syntax confirmed by the installed version. Set the process working directory to the canonical target root even when a directory flag also exists. Capture the conversation ID from structured output. Never use `resume --last` or an interactive recent-session picker.

Start read-only and non-interactive. Do not use a full-access or approval-bypass mode. Before granting target writes, verify effective OS-level containment or keep the counterpart read-only and broker a serialized patch handoff.

## Claude Code CLI

Discover support with harmless help and version commands. Verify, rather than assume:

- print/non-interactive mode and JSON or stream-JSON output;
- the result field carrying the session ID;
- continuation with an exact session ID;
- working-directory behavior;
- permission mode, allowed/disallowed tools, settings, hooks, plugins, and subagents;
- model, reasoning, usage, cost, and duration metadata.

For known modern CLIs, the relevant family is commonly `claude -p --output-format json` and `--resume <exact-id>`, but use only syntax confirmed by the installed version. Launch the process from the canonical target root and verify the target reported by the counterpart. Never use an implicit continue/recent selector.

Start without edit, shell, network, connector, workflow, or subagent authority. Claude permission settings are guardrails, not proof of OS containment. Keep the counterpart read-only unless its writable target boundary is independently enforced.

## Other providers

Build an adapter from semantics, not brand-specific guesses:

1. Discover the provider’s create/start, send/resume, result, wait, and cancel surfaces.
2. Start a new conversation with explicit target-root context.
3. Obtain a stable ID from the runtime, not from parsing model prose.
4. Resume only that ID for every round.
5. Preserve structured responses and exit status.
6. Verify project and permission boundaries on every material mode change.

Reject fire-and-forget commands, stateless one-shot prompts, and sessions that can only continue “the most recent” conversation. A pair of unrelated one-shot calls is not a dialogue.

## Invocation hygiene

- Construct argument arrays through a process API when possible; send dynamic task capsules on standard input or another data channel, never as shell source.
- Use the provider’s normal configured credential chain. Missing environment variables alone do not prove authentication failure.
- Do not print configuration files or secret-bearing environment values while checking readiness.
- Do not install, upgrade, initiate login, or mutate a live system during discovery.
- Bound waits and send concise progress updates during long turns.
- Retry one transient failure at most once. Preserve the session identity and partial evidence.
- Treat a successful process exit as necessary but insufficient; verify the structured terminal result and target-project evidence.

## Downgrade and failure handling

If exact continuation fails, stop calling the exchange a collaboration. Preserve the accepted evidence, report the broken session, and either start a new explicitly identified collaboration with the operator’s knowledge or continue independently.

If only read-only operation is safe, complete discovery and the agreement, then provide the target-side patch or change plan for a serialized handoff. If no real target-root conversation can be opened, report `counterpart not run` and the missing adapter capability.
