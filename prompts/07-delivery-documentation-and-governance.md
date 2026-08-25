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

In the README's pull-request guidance, explain that Kind selection is bounded authoring judgement,
not a processor feature: a human or coding agent selects one canonical Kind using the canonical
`AGENTS.md` guidance, then the processor validates that explicit value and preserves it unchanged.
Do not present a fallback or a heuristic based on repository artefacts.

Add canonical `AGENTS.md` repository instructions. Agent-specific instruction files may point to it
but must not duplicate it. Include the core CLI/action contracts, public compatibility surfaces,
testing expectations, untrusted-input rules, and least-privilege requirements.

Add short pointers to the canonical root instructions so every tier-one coding agent receives the
same contract: `CLAUDE.md`, `GEMINI.md`, `.github/copilot-instructions.md`,
`.cursor/rules/agent-instructions.mdc` (with `alwaysApply: true` front matter),
`.windsurf/rules/agent-instructions.md` (with `trigger: always_on` front matter), and
`.clinerules/agent-instructions.md`.

An agent whose tool has no pointer file sees no project instructions at all, so a missing pointer is
not a cosmetic omission. Each pointer names the canonical file, states that its rules are binding
regardless of tool, and says that instruction changes belong in the canonical file rather than the
pointer. Do not list a pointer location the repository does not actually contain: a pointer to an
absent directory teaches a future reader that the set is maintained when it is not.

Each pointer must also surface the label-and-body-sections rule, because that is the rule an agent
most often needs before it can open a correct pull request. Keep `## Narrative Kind`,
`## Narrative Context`, `## Narrative Decision` and `## Narrative Consequences` each on a single
line — reflowing prose can split a heading name across a line break, which still renders but defeats
an agent grepping for the exact heading it has to emit. State that a qualifying pull request
supplies exactly one non-empty canonical kind under Narrative Kind and that kind is never inferred
or defaulted.

The canonical `AGENTS.md` must make Kind selection usable without prescribing mechanical evidence.
It must say that Kind is an explicit, human- or agent-authored judgement; use exactly `product`,
`architecture`, `governance`, `operational`, `correction`, or `experiment`; and apply this rule:

> Classify the primary nature of the decision being recorded, not the artefact changed or where the
> change is implemented.

Describe `product` as a product or domain decision; `architecture` as an architecture or integration
decision; `governance` as a governance or development-process decision; `operational` as an
operational policy or practice; `correction` as a correction to an earlier recorded decision or
shipped behaviour; and `experiment` as a bounded experiment whose outcome belongs in project memory.
Where more than one Kind appears plausible, the author chooses the primary nature and leaves that
explicit choice visible for human review. Do not direct an agent to determine Kind from the pull
request title, paths, filenames, labels, ADR metadata, technology names, or repository conventions.
The pull-request template exposes the field and canonical choices, while `AGENTS.md` is the
single source for this selection guidance; pointers remain pointers only.

Do not add this repository's own `.project-narrative.json`, `narrative/` fragments,
`Narrative.md`, Narrative pull-request template, or self-hosting workflows in this stage. Examples
for consumers belong under `examples/` or fenced README snippets.

## Acceptance criteria

- `npm run check` succeeds from a clean install.
- `npm pack --dry-run` includes the CLI, action runtime, metadata, licence, and documentation.
- Every documented command and action input matches executable behavior.
- The exact command behind `action.yml` check mode succeeds against a temporary consumer fixture.
- Agent-specific instruction files contain pointers, not divergent copies of the rules, and one
  exists for each of Claude, Gemini, Copilot, Cursor, Windsurf, and Cline.
- Canonical `AGENTS.md` gives agents the six decision-oriented Kind definitions, primary-nature
  tie-break rule, human-review boundary, and prohibition on mechanical classification; its pointers
  introduce no contradictory Kind-selection rule.
- No pointer cites an instruction-file location the repository does not contain.
- The dependency list remains empty.
- `git diff --check` succeeds.

Commit the completed stage locally. Do not push.
