# Prompt 14 — Correct Narrative Kind authorship guidance in an existing processor

Use this prompt in a coding-agent task connected to an already-built Project Narrative
implementation repository after Prompt 13. Do not run it in NarrativeBuilder or a consumer
repository.

## Scope and precedence

Prompts 00–13 are published historical instructions and must not be edited. This prompt corrects a
repeatable Builder-specification learning discovered while independently replaying Prompt 13: older
INSTALL guidance can be mechanically extended to say that Kind, Context, Decision, and Consequences
are all “authored by a human.” That wording contradicts Prompt 13 for Kind.

This prompt supersedes only guidance or comments in the built Narrative implementation that
explicitly or necessarily state or imply that Narrative Kind must be human-authored. Silence about
who selects Kind is not a contradiction. Compatible generic wording such as “the author selects”
is not a contradiction when it does not exclude coding-agent selection. It does not supersede the
existing authorship contract for Context, Decision, or Consequences, unless that contract already
permits coding-agent authorship. Preserve every unrelated constraint from the earlier prompts.

The corrected contract is:

- a human or coding agent may select exactly one Narrative Kind using Prompt 13’s bounded,
  canonical six-value guidance;
- this selection remains explicit authoring judgement, not processor inference or a fallback;
- human review remains the authority for adopting the proposed record; and
- the processor validates and preserves an explicit supplied Kind and never infers or defaults it.

## Inspect before editing

Read the implementation’s canonical agent instructions, pointer instruction files, INSTALL guidance,
README and other product documentation, templates, comments, tests, and any installed or scaffolded
consumer guidance. Find every statement or comment that explicitly or necessarily says or implies
that Kind is authored only by a human. Do not treat silence about who selects Kind, or compatible
generic wording that does not exclude coding-agent selection, as contradictory. State the files and
surfaces found before editing.

Do not prescribe a filename or line number as the sole correction target: correct the contradictory
guidance wherever it occurs in the built Narrative implementation. Do not rewrite accepted Narrative
fragments, generated history, or unrelated product contracts.

## Correct the guidance

Amend only the contradictory Kind-authorship wording so all affected guidance consistently permits
a human or coding agent to select Narrative Kind from the bounded canonical guidance already
established by Prompt 13. A genuinely contradictory comment—for example, one that calls all PR
evidence “human-authored”—must still be corrected. Do not duplicate or expand the full
Kind-selection guidance into already-compatible surfaces merely for consistency or completeness.
Keep the full Kind taxonomy and selection rule in the canonical agent instruction file; it remains
the authoritative home for the Kind-selection semantics. Pointer instruction files must remain
pointers rather than competing semantic copies.

Preserve the existing review-first boundary. The correction must not imply that a coding agent may
author Context, Decision, or Consequences where the existing contract reserves them to a human. It
must not turn Kind selection into processor classification, inference, or defaulting.

Do not make runtime, parser, validation, action, CLI, template-structure, or generated-output
behaviour changes merely to perform this wording correction. In particular, do not change the rule
that the processor validates and preserves an explicit supplied Kind and never infers or defaults
it.

Do not make any decision about heading-matching semantics, duplicate Context, Decision, or
Consequences headings, whitespace tolerance, or other unresolved heading behaviour. Do not address
Windows symlink portability, Narrative.md freshness, fresh-install validation gaps, or any other
unrelated issue.

## Prove no behavioural regression

Add or update only tests or checks appropriate to prove that this guidance correction causes no
behavioural regression. Run the normal project checks, including `npm run check` and
`git diff --check`. If existing tests cover explicit supplied Kind, also run the relevant tests and
confirm they continue to prove that valid Kind is preserved and missing, invalid, or duplicate Kind
fails without processor inference or a default.

## Acceptance criteria

- Published Prompts 00–13 remain unchanged.
- Every corrected guidance surface permits a human or coding agent to select Kind using the bounded
  canonical guidance.
- Silence about who selects Kind and generic compatible wording that does not exclude coding-agent
  selection remain unchanged; the correction does not expand Kind guidance into those surfaces.
- The canonical agent instructions remain the authoritative home for the full Kind-selection
  semantics.
- No corrected wording broadens coding-agent authorship to Context, Decision, or Consequences beyond
  the existing contract.
- Human review remains the adoption authority.
- Processor behaviour is unchanged: it validates and preserves explicit Kind and never infers or
  defaults it.
- No unrelated heading, portability, generated-output freshness, or fresh-install issue is changed.
- Appropriate checks demonstrate no behavioural regression, and `npm run check` and
  `git diff --check` succeed.

Report the contradictory surfaces corrected, files changed, tests and checks run, and any
assumptions. Commit the completed change locally. Do not push or merge unless I explicitly ask.
