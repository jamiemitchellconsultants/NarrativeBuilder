# Example prompt — Fix a narrative-required pull request

Use this prompt when a pull request carrying the `narrative-required` label has failed, or is about
to fail, the narrative gate because its description does not contain the required narrative
sections. Start a coding-agent task connected to the repository that holds the pull request.

This is an operational remediation prompt, not a build stage. It assumes a Narrative processor is
already installed in the repository. Because processors are cloned and customised per team, this
prompt names no specific action or repository: rely on whatever Narrative checks the target
repository actually runs.

---

You are fixing a pull request so it satisfies this repository's `narrative-required` gate.

The gate requires the pull-request **description** — not the commit messages — to contain four
level-2 Markdown headings, spelled exactly. Narrative Kind has exactly one non-empty supported
canonical value; the other three headings each have substantive content:

```markdown
## Narrative Kind

## Narrative Context

## Narrative Decision

## Narrative Consequences
```

Rules the gate enforces:

- The sections must be `## ` headings with those exact names. Bold labels such as `**Context:**` do
  not count.
- Narrative Kind must be exactly one of `architecture`, `product`, `governance`, `operational`,
  `correction`, or `experiment`; it is never inferred or defaulted. Missing, empty, duplicate, or
  unsupported Narrative Kind fails visibly.
- Context, Decision, and Consequences must each have real content beneath them; empty sections fail.
- The sections live in the pull-request description, editable in the GitHub UI or with
  `gh pr edit <number> --body-file <file>`.

## Inspect before editing

Read the pull request's diff and commits to understand what actually changed and why. Read the
repository's pull-request template and Narrative configuration so your sections match the required
interface exactly. Confirm the pull request genuinely carries the `narrative-required` label. Use
this inspection only to understand and repair Context, Decision, and Consequences, not to classify
the pull request.

## Obtain Narrative Kind

If Narrative Kind is missing, stop and ask the user to supply or confirm one canonical kind before
writing it. Do not derive a kind from the diff, commits, title, paths, labels, ADR metadata, Context,
Decision, or Consequences prose, or repository conventions. The same rule applies if an existing
kind is empty, duplicate, or unsupported: obtain human confirmation rather than choosing a value.

## Draft the sections from evidence

Draft Context, Decision, and Consequences from the change itself:

- **Context** — the situation or problem that prompted the change.
- **Decision** — what was chosen and the approach taken.
- **Consequences** — the trade-offs, new dependencies, follow-ups, and what changes for other
  contributors.

Do not infer or invent rationale from a diff. If the reason for the change is not evident, ask me
before guessing. Narrative records an explicit decision; it is not a changelog.

## Update the pull request

Add the four sections to the pull-request description, preserving any existing Summary, Testing, or
other content. Then confirm the repository's narrative checks are satisfied: the pre-merge body gate
if the repository has one, and the post-merge maintenance job.

## Notes

- The maintenance job typically runs only when the pull request merges and reads the body from the
  merge event, so an already-merged pull request cannot have its failed check turned green
  retroactively. Fixing the description matters for open pull requests, before they merge.
- If a change is genuinely mechanical and should not carry narrative at all, the correct fix is to
  remove the `narrative-required` label, not to write hollow sections.

Report the pull request affected, the sections you added, and the resulting check status. Do not
merge, push unrelated changes, or alter the label unless I explicitly ask.
