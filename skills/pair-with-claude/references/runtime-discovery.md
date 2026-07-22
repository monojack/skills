# Discover Claude CLI capabilities at runtime

Read this reference before constructing a Claude invocation. Treat the installed CLI and current official documentation as the source of truth for mechanism. Treat `SKILL.md` as the source of truth for authorization, safety boundaries, and lane behavior.

## Build a capability map

Create a task-local map with the requirement, evidence source, status (`verified`, `unverified`, or `blocked`), constraints, and fail-closed alternative for each capability. Do not infer support from a version string alone.

1. Locate the executable and inspect its version without printing credentials or configuration.
2. Read top-level help and only the relevant subcommand help. Use advertised read-only status or effective-configuration views only when their documentation says they do not reveal credentials. Check secret-bearing environment variables by presence, never value.
3. When help is incomplete or a behavior affects safety, consult the current official Claude Code documentation and reconcile it with the installed version. Let the binary establish accepted syntax, the documentation explain intended semantics, and effective managed policy determine permitted behavior. When they conflict, adopt the most restrictive interpretation.
4. Record whether the current environment supports each capability the selected lane needs:

   - foreground non-interactive requests, reliable exit status, and machine-readable results;
   - resumable sessions and a way to observe the actual model, errors, denials, and fallback;
   - aliases or identifiers for the latest Opus-class model and the best model available to the account;
   - supported reasoning controls and the deepest focused level, including environment or organization overrides;
   - tool availability controls, approval rules, hard denies, and path boundaries;
   - customization isolation and the managed settings or hooks that remain effective;
   - OS-enforced process, filesystem, and network containment that can fail closed;
   - background launch, identity, status, logs, input or approval handoff, cancellation, and terminal-state observation;
   - worktree or scratch isolation, base selection, lifecycle behavior, and treatment of dirty or untracked state;
   - session persistence, privacy controls, and safe continuation semantics.

Discovery must not start a login flow, update the CLI, launch a worker, edit configuration, execute project code, contact external systems, or probe destructive boundaries. If a model-routing or parser behavior cannot otherwise be confirmed, use at most one bounded neutral preflight from a non-project directory with no tools, no project context, no sensitive input, and no persistence when supported. Tell the user first when it incurs meaningful cost.

Treat an advertised authentication-status view or configured credential presence as preliminary evidence, not the only valid credential check. When authentication remains uncertain but every required safety and lane-boundary capability is verified, the first bounded intended request may establish it. The request must already satisfy the selected lane, contain no extra probe behavior, and fail closed; on authentication failure, do not retry, install, update, or initiate login without the user's request.

Keep the map in task-local working state. Treat missing, malformed, contradictory, or unexpectedly side-effecting evidence as `unverified`. Do not write environment-specific discoveries into the repository unless the user asks for compatibility documentation.

## Compile a lane into an invocation

Translate the lane's invariants into the discovered capabilities instead of copying a remembered command:

1. Start with no mutation, shell, network, workflow, connector, or external-messaging authority.
2. Intersect the lane requirements with the verified capability map. Add only required capabilities and prove that every non-negotiable deny still wins.
3. Use the canonical authorized root. Additional roots require explicit need and equivalent protection.
4. Construct a structured argument vector. Send dynamic dossiers as data, never as executable shell source.
5. Include the relevant teammate and lane contracts explicitly when configuration isolation may suppress project instructions.
6. Record requested and observed model/effort, session identity, root, snapshot, permission boundary, and any downgrade. Launch only when every required safety and lane-boundary map entry is `verified`; authentication alone may remain pending for the bounded intended request described above.

Before launch, inspect the effective settings, hooks, plugins, agents, commands, and environment overrides that can change those semantics. A troubleshooting or safe mode is useful only after verifying what it does and does not disable in the installed CLI.

Do not run an empirical write, permission, containment, or escape test merely to learn behavior. A disposable directory or worktree is collision isolation, not proof of containment. When installed help, official documentation, and effective policy cannot settle a safety-critical question, declare the capability unavailable.

## Degrade safely

- If read-only behavior cannot be established, use a reviewed context-only dossier in a contained environment or skip Claude.
- If command execution cannot be contained, keep the shell unavailable and let Codex broker reviewed commands.
- If writes cannot be isolated, serialize an explicitly authorized shared-checkout handoff; never delegate parallel writing there.
- If a background worker cannot be supervised and stopped reliably, use a foreground or brokered workflow.
- If one-use approval cannot be enforced, deny the operation or ask the user; never persist a broad rule as a shortcut.
- If the requested best model or deepest effort cannot be observed, disclose the uncertainty or downgrade. Do not count a degraded evaluator as the requested final gate without user acceptance.
- If web or network scoping cannot be verified, omit it and let Codex provide independently verified sources.
- If session continuation cannot preserve the boundary, start a new session with a bounded reviewed summary.
- If the executable or authentication is unavailable, continue independently. Do not install, update, or initiate login without the user's request.

## Re-check these hazards

Treat each item as a question for the installed CLI, not as a permanent fact:

- Does the permission mode authorize filesystem commands beyond built-in edit tools?
- Can shell subprocesses bypass file-tool rules, and is the OS sandbox truly fail-closed?
- Which managed policies or hooks remain active when customizations are disabled?
- Do worktrees share Git metadata, credentials, hooks, caches, services, or other host resources?
- Which base and working state does automatic isolation use, and who removes it?
- Can a background session commit, publish, deploy, or create external artifacts automatically?
- How are blocked prompts surfaced, approved once, denied, and resumed?
- Are completed, failed, and stopped workers still visible to lifecycle queries?
- Which launch constraints must be repeated or are restored when a session resumes?
- Can model aliases, fallback, environment variables, or organization policy change the requested model or effort silently?
- What permission-rule precedence and matching syntax does this version enforce?

## Official documentation entry points

- [CLI reference](https://code.claude.com/docs/en/cli-reference)
- [Programmatic sessions](https://code.claude.com/docs/en/headless)
- [Model selection and effort](https://code.claude.com/docs/en/model-config)
- [Permissions](https://code.claude.com/docs/en/permissions)
- [Permission modes](https://code.claude.com/docs/en/permission-modes)
- [Sandboxing](https://code.claude.com/docs/en/sandboxing)
- [Background agents](https://code.claude.com/docs/en/agent-view)
- [Worktrees](https://code.claude.com/docs/en/worktrees)
- [Hooks](https://code.claude.com/docs/en/hooks)
