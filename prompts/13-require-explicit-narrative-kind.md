# Prompt 13 — Require explicit Narrative Kind in an existing processor

Use this prompt in a coding-agent task connected to an already-built Project Narrative
implementation repository. Do not run it in NarrativeBuilder or a consumer repository.

## Scope and precedence

Prompts 00–12 are published historical instructions and must not be edited. An implementation built
through the original staged sequence, Prompts 00–09, may therefore still contain the earlier Prompt
5 behaviour that writes `kind: product` for every generated fragment.

This prompt intentionally supersedes that hard-coded behaviour in the already-built implementation.
Any apparent conflict with the earlier instruction is deliberate. This prompt takes precedence only
for the explicit-Narrative-Kind behaviour described below; preserve every unrelated constraint from
the earlier prompts.

The reviewed design is that a qualifying pull request explicitly supplies one Narrative Kind. Kind
selection is bounded authoring judgement by a human or coding agent, not processor inference and not
a semantic fallback. The processor validates and preserves the supplied canonical value.

## Inspect before editing

Read the implementation's existing parser, action runtime, proposal flow, tests, documentation,
canonical agent instructions, pull-request template, self-hosting configuration, and any existing
install command or fresh-install scaffolding. State the files that implement each surface below and
identify any existing consumer documentation that needs a deliberate migration note.

Do not rewrite accepted Narrative fragments, generated history to disguise prior behaviour, or
unrelated product contracts.

## Require explicit pull-request evidence

A pull request that qualifies through `narrative-required` must contain these exact level-two
headings in its body:

```markdown
## Narrative Kind

## Narrative Context

## Narrative Decision

## Narrative Consequences
```

Narrative Kind contains exactly one non-empty canonical value:

- `product`
- `architecture`
- `governance`
- `operational`
- `correction`
- `experiment`

Keep the existing qualification and skip behaviour for unmerged and unlabelled events.

## Update the processor

Update the pull-request parser and maintenance processor to:

- parse Narrative Kind as explicit pull-request evidence;
- reject a missing, empty, duplicate, unsupported, or otherwise non-canonical Kind visibly with an
  actionable error;
- preserve a valid supplied canonical Kind exactly in the generated fragment metadata;
- remove the hard-coded `kind: product` assignment; and
- never infer or default Kind from a title, diff, path, filename, label, ADR metadata, prose, or
  repository convention.

Classification failure must happen before a fragment, compiled output, branch, commit, or draft
proposal pull request is created. Do not weaken the existing security, qualification, or review
boundaries while making this change.

## Add tests

Add or update tests covering:

- every supported Kind;
- missing, empty, duplicate, unsupported, and otherwise non-canonical Kind;
- unmerged and unlabelled events retaining their existing no-side-effect behaviour; and
- classification failures producing no fragment or proposal side effects.

Run the normal project checks, including `npm run check` and `git diff --check`.

## Add canonical authoring guidance

Put the full Kind-selection guidance in the implementation repository's canonical agent instruction
file. Other agent instruction files must remain pointers rather than competing semantic copies.

State that a human or coding agent selects exactly one canonical Kind by classifying the primary
nature of the decision being recorded, not the artefact changed or where the change is implemented:

- `product` — product or domain decision;
- `architecture` — architecture or integration decision;
- `governance` — governance or development-process decision;
- `operational` — operational policy or practice;
- `correction` — correction to an earlier recorded decision or shipped behaviour; and
- `experiment` — bounded experiment whose outcome should remain in project memory.

Where several values seem plausible, choose the primary nature and leave the explicit choice for
human review. Do not replace this reviewed bounded-authoring model with processor inference or a
fallback value.

## Update templates, documentation, and installations

Update the implementation's pull-request template to expose all four headings and list the six valid
Kind names. Keep the full taxonomy and selection rule in the canonical instructions rather than
duplicating it in the template.

Update the product README, action and CLI documentation, and self-hosting configuration to the new
contract. Treat the already self-hosted Narrative repository as an existing consumer: deliberately
migrate its template and canonical instructions, but preserve its reviewed historical fragments.

If the implementation already provides a fresh-install command or scaffolding, update that existing
capability so new consumers receive the four-heading template and canonical Kind guidance. Keep that
installation behaviour non-destructive. If it has no fresh-install capability, do not introduce one
as part of this prompt.

Do not invent an automatic upgrade command or a migration framework. Document a manual migration path
for existing consumers that is appropriate to the capabilities present: deliberately update their
pinned Narrative implementation, pull-request template, and canonical instructions together before
relying on the new behaviour.

## Acceptance criteria

- A qualifying pull request provides exactly one canonical Narrative Kind and the three existing
  substantive Narrative sections.
- The processor validates and preserves Kind without inferring or defaulting it.
- Invalid Kind evidence fails visibly before fragment or proposal side effects.
- Tests cover the supported values, invalid evidence, skips, and no-side-effect failures.
- Templates, canonical guidance, self-hosting, and documented consumer migration all describe the
  same contract.
- If fresh-install capability already exists, its command or scaffolding produces the same contract.
- Existing accepted fragments remain unchanged.
- `npm run check` and `git diff --check` succeed.

Report the files changed, tests run, migration steps for existing consumers, and any assumptions.
Commit the completed change locally. Do not push or merge unless I explicitly ask.
