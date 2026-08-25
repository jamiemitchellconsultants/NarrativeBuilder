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

The gate requires the pull-request **description** — not the commit messages — to contain three
level-2 Markdown headings, spelled exactly, each followed by substantive content:

```markdown
## Narrative Context

## Narrative Decision

## Narrative Consequences
```

Rules the gate enforces:

- The sections must be `## ` headings with those exact names. Bold labels such as `**Context:**` do
  not count.
- Each section must have real content beneath it; empty sections fail.
- The sections live in the pull-request description, editable in the GitHub UI or with
  `gh pr edit <number> --body-file <file>`.

## Inspect before editing

Read the pull request's diff and commits to understand what actually changed and why. Read the
repository's pull-request template and Narrative configuration so your sections match the required
interface exactly. Confirm the pull request genuinely carries the `narrative-required` label.

## Draft the sections from evidence

Draft Context, Decision, and Consequences from the change itself:

- **Context** — the situation or problem that prompted the change.
- **Decision** — what was chosen and the approach taken.
- **Consequences** — the trade-offs, new dependencies, follow-ups, and what changes for other
  contributors.

Do not infer or invent rationale from a diff. If the reason for the change is not evident, ask me
before guessing. Narrative records an explicit decision; it is not a changelog.

## Update the pull request

Add the three sections to the pull-request description, preserving any existing Summary, Testing, or
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
