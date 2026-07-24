# Prompt 12 — One-command consumer install and agent recipe

Using the previously supplied repository contract, implement this enhancement to the completed
processor: collapse consumer onboarding into a single non-destructive command and add a recipe
written for an installing coding agent.

This is a build enhancement, not an operational example. It assumes Stages 1–8 are complete: the CLI
already provides `init`, `validate`, `compile`, and `check`, and `action.yml`, both consumer
workflow examples, `README.md`, and `AGENTS.md` already exist. Apply it in a coding-agent task
connected to the Narrative processor repository itself.

## Why

Consumer onboarding is currently a long manual guide: author `.project-narrative.json`, a preamble,
two workflows, and a pull-request template by hand, then toggle repository settings. That copy-paste
sequence is a high wall for humans, and in practice adopters install the processor by asking a
coding agent to "add this Narrative to my repository." Those agents otherwise have to reverse-engineer
intent from a README aimed at humans and from `AGENTS.md`, which governs *developing* the processor
rather than *installing* it. The result varies by tool. This enhancement makes both paths
deterministic.

## Implement the `install` command

Add and export an `install(configPath)` function and wire a `narrative install` command into the CLI
dispatcher and usage string.

`install` must:

- run `init` first, so it reuses the existing non-destructive config, preamble, fragment-directory,
  and initial-compilation behavior;
- then scaffold the files `init` does not create: the maintenance workflow, the validation workflow,
  and the pull-request template;
- **never overwrite an existing file** — an existing file at any target path is kept untouched;
- return a structured result listing the paths it created, the paths it kept, and the manual
  follow-ups it cannot perform.

Keep the scaffolded file contents byte-identical to the workflow example and pull-request template
the README already documents, so there is one source of truth. Define the consumer action reference
(`owner/Repo@ref`) and the required label as single exported constants and interpolate them into the
scaffolded workflows and template, so repointing to a reviewed tag or SHA later is a one-line edit.

The maintenance workflow must run on pull-request close, proceed only when merged, check out full
history, grant only `contents: write` and `pull-requests: write`, invoke the action, pass
`GITHUB_TOKEN`, and pass the required label. The validation workflow must run the action in `check`
mode with only `contents: read`. The pull-request template must carry the three exact
`## Narrative Context`, `## Narrative Decision`, `## Narrative Consequences` headings.

The command must print each scaffolded path as created or kept, then print the manual follow-ups it
cannot perform:

1. enable **Read and write permissions** and **Allow GitHub Actions to create and approve pull
   requests** in repository Actions settings;
2. create the trigger label spelled exactly;
3. commit and merge the scaffolded files to the default branch before the first decision capture,
   because workflows only run from files already on the default branch.

`install` must remain local, model-free, network-free, and deterministic. It performs no GitHub API
calls and never edits repository settings or creates the label itself — those stay out of scope by
design and are only reported.

## Add the agent-facing install recipe

Add `INSTALL.md` written **to the installing coding agent**, distinct from `AGENTS.md`. It must:

- state the single `install` invocation (local and npx-style);
- describe the non-destructive scaffold set and the existing-template caveat (an existing template
  must gain the three exact headings or labelled pull requests fail visibly; do not silently
  overwrite it);
- list the manual follow-ups the CLI cannot perform;
- give the deterministic verification step (`narrative check`);
- state the prohibitions: do not hand-edit the generated output, do not label the installation pull
  request itself, do not invent narrative content, do not change the heading names or label
  spelling.

## Update documentation and contracts

- Add a quick-install section to `README.md` above the step-by-step guide, and add `install` to the
  CLI reference. Present the step-by-step guide as the explanation of what `install` scaffolds.
- Record the `install` contract in `AGENTS.md`: non-destructive consumer scaffolding that reports
  the follow-ups it cannot perform, and note that `INSTALL.md` is the agent-facing consumer-install
  recipe, distinct from `AGENTS.md`.

## Acceptance criteria

- Unit or integration tests prove that `install` scaffolds every expected file, that a rerun keeps a
  pre-existing target file byte-for-byte unchanged, and that the scaffolded workflows reference the
  action constant and the required label.
- The scaffolded workflow and template contents match the README's documented versions exactly.
- The command reports created paths, kept paths, and the three manual follow-ups.
- `install` adds no dependency and makes no network or model call.
- Every documented command matches executable behavior.
- `npm run check` succeeds and `git diff --check` succeeds.

Commit the completed enhancement locally. Do not push.
