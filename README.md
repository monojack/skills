# Agent Skills

Shared agent skills installable from GitHub with [`npx skills`](https://skills.sh/docs/cli).

## Skills

- `gh-workflow-loop`: run a unified GitHub workflow from either starting point: implement an issue through a ready PR, or handle an existing PR's review feedback with pre-push rechecks and heartbeat monitoring.
- `codebase-review`: perform a thorough whole-codebase review, consult relevant official framework documentation first, write findings under `reviews/`, and open GitHub issues for each actionable finding.
- `incremental-development`: deliver software changes as small, focused, review-gated, and bisectable commits with explicit approval before each commit.
- `pair-with-claude`: collaborate with Claude in the shared project environment, run bounded best-model evaluation loops over concrete candidates, or delegate work to supervised scratch/worktree workers.
- `pair-with-codex`: collaborate with Codex in the shared project environment, run bounded best-model evaluation loops over concrete candidates, or delegate work to supervised scratch/worktree workers.

The former `gh-issue-pr-loop` and `gh-pr-loop` skills were consolidated into `gh-workflow-loop`. Use `gh-workflow-loop` for both issue-to-PR implementation and existing-PR review workflows.

## Install

List the skills available in this repository:

```sh
npx skills add monojack/skills --list
```

Install interactively and choose the skills and agents to configure:

```sh
npx skills add monojack/skills
```

Install one skill globally and choose the agents to configure:

```sh
npx skills add monojack/skills \
  --skill gh-workflow-loop \
  --global
```

Install every skill globally and choose the agents to configure:

```sh
npx skills add monojack/skills \
  --skill '*' \
  --global
```

## Local Development

Use the local checkout as the source while developing a skill:

```sh
npx skills add . --list
npx skills add . --skill gh-workflow-loop
```

For a private repository, authenticate first with `gh auth login` or set `GH_TOKEN` / `GITHUB_TOKEN` on the target machine.
