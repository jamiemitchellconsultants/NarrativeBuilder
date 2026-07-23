# Reusable contract

Submit this contract before the implementation prompts. If stages run in separate coding-agent
tasks, prepend this contract to every stage.

You are creating a production-quality repository called Narrative in an empty or partially
initialized workspace.

## Product objective

Build Project Narrative: a deterministic, review-first processor that maintains the decision
history of a GitHub repository. It converts explicit evidence from qualifying merged pull requests
into proposed Markdown fragments and compiles reviewed fragments into `Narrative.md`.

It is not a changelog generator. It must never infer or invent rationale from a diff. A pull request
opts in through a configured label and supplies Context, Decision, and Consequences explicitly.
Generated wording is proposed separately for human review.

## Non-negotiable architecture

- Use Node.js 20 or newer and ECMAScript modules.
- Keep the CLI and action runtime dependency-free.
- Validation, ordering, compilation, and stale-output checks are local, model-free, network-free,
  and deterministic.
- Fragments are authoritative; generated `Narrative.md` is never hand-edited.
- Slugs are durable identities; displayed entry numbers are derived.
- Pull-request prose and event payloads are untrusted data and must never become shell commands.
- The maintenance action may use the GitHub API only for the documented proposal branch and draft
  pull-request flow.
- A qualifying merge creates a separate draft proposal; it never commits accepted history directly
  to the default branch.
- Human review remains the authority that accepts narrative wording.
- Do not introduce a database, model API, hosted service, or package dependency.
- Do not implement functionality beyond the current stage.
- Do not install or consume a completed Project Narrative processor during Stages 1–8. The
  repository becomes self-hosting only in the final stage, after its own implementation is audited.

## Working method

1. Inspect the workspace before editing.
2. Explain the current stage in plain language and state a short plan.
3. Implement the smallest coherent increment.
4. Add tests for success, rejection, and boundary cases.
5. Run the relevant checks and fix failures.
6. Show the learner the important diff and explain one or two key design choices.
7. Report files changed, commands run, results, assumptions, and remaining risks.
8. Commit locally when the stage asks, but do not push or open a pull request unless explicitly
   requested.
