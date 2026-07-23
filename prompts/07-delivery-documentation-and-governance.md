# Prompt 7 — Composite action, documentation, and governance

Using the previously supplied repository contract, implement Stage 7: package the processor for
other repositories and document its complete operating contract.

Add `action.yml` with:

- action name, description, and author;
- optional `github-token`;
- `mode`, defaulting to `maintain`;
- `config`, defaulting to `.project-narrative.json`;
- `required-label`, defaulting to `narrative-required`;
- composite steps that run the CLI `check` path or the maintenance runtime;
- suitable action branding;
- no package-install step.

Add a consumer maintenance-workflow example that:

- runs when a pull request closes;
- proceeds only when it merged;
- checks out full history;
- grants only `contents: write` and `pull-requests: write`;
- invokes the published action and passes `GITHUB_TOKEN`.

Write a complete `README.md` covering:

- why Project Narrative is a decision history rather than a changelog;
- the labelled two-pull-request lifecycle;
- consumer configuration, preamble, fragment directory, and initial compilation;
- exact PR headings;
- CLI and action inputs;
- local and npx-style use;
- action pinning trade-offs;
- repository settings needed for Actions to create pull requests;
- validation workflow with `contents: read`;
- first end-to-end consumer test;
- fragment schema, kinds, statuses, and evidence;
- stale-output repair and rerun troubleshooting;
- security, permissions, and human-review boundaries.

Add canonical `AGENTS.md` repository instructions. Agent-specific instruction files may point to it
but must not duplicate it. Include the core CLI/action contracts, public compatibility surfaces,
testing expectations, untrusted-input rules, and least-privilege requirements.

Add `CLAUDE.md`, `GEMINI.md`, and `.github/copilot-instructions.md` as short pointers to the
canonical root instructions so different coding agents receive the same contract.

Do not add this repository's own `.project-narrative.json`, `narrative/` fragments,
`Narrative.md`, Narrative pull-request template, or self-hosting workflows in this stage. Examples
for consumers belong under `examples/` or fenced README snippets.

## Acceptance criteria

- `npm run check` succeeds from a clean install.
- `npm pack --dry-run` includes the CLI, action runtime, metadata, licence, and documentation.
- Every documented command and action input matches executable behavior.
- The exact command behind `action.yml` check mode succeeds against a temporary consumer fixture.
- Agent-specific instruction files contain pointers, not divergent copies of the rules.
- The dependency list remains empty.
- `git diff --check` succeeds.

Commit the completed stage locally. Do not push.
