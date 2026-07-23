# Prompt 6 — Proposal branch and draft pull request

Using the previously supplied repository contract, implement Stage 6: publish the locally generated
narrative change as a separate review proposal.

After a qualifying event has created and compiled the fragment:

1. Configure the Git author as `github-actions[bot]`.
2. Create `automation/narrative-pr-<source-pr-number>`.
3. Stage only the configured fragment directory and compiled output.
4. Commit with `docs: propose narrative for PR #<number>`.
5. Force-update that exact remote automation branch so reruns are repeatable.
6. Query open pull requests for the same head branch.
7. If none exists, open a draft pull request against the event repository's default branch.
8. If one exists, update the branch but do not create a duplicate proposal.

Use argument-array process execution such as `execFileSync`; do not construct an interpolated shell
command. Keep the branch name derived only from the numeric pull-request number.

Implement a small GitHub API helper using built-in `fetch`:

- send the bearer token only in the authorization header;
- set GitHub's JSON accept header and API version;
- use the configured API base URL so tests can use a local fake server;
- encode query values;
- handle `204` without parsing JSON;
- fail with the response status and useful body text, without exposing the token.

Validate required runtime inputs before mutating Git state. A skipped unmerged or unlabelled event
must not require a token.

## Acceptance criteria

- Tests or a controlled harness prove the exact Git argument arrays.
- A local fake HTTP server proves create, existing-PR, error, and `204` API behavior without calling
  real GitHub.
- Tests prove malicious PR prose cannot change command arguments or request targets.
- Rerunning for one source PR targets the same branch and never opens a second draft.
- Generated pull-request wording says it is an interpretation requiring review.
- `npm run check` succeeds.

Commit the completed stage locally. Do not push or run the action against a live repository.
