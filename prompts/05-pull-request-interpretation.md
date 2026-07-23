# Prompt 5 — Merged pull-request interpretation

Using the previously supplied repository contract, implement Stage 5: safely interpret a GitHub
pull-request event and turn qualifying evidence into a local proposal.

Create the action runtime in `action/github-action.mjs`, but keep event interpretation separable
from network and Git operations so it can be tested without GitHub.

Read these environment inputs:

- `INPUT_GITHUB_TOKEN`;
- `INPUT_CONFIG`, defaulting to `.project-narrative.json`;
- `INPUT_REQUIRED_LABEL`, defaulting to `narrative-required`;
- `GITHUB_EVENT_PATH`;
- `GITHUB_REPOSITORY`;
- `GITHUB_API_URL`, defaulting to `https://api.github.com`.

Interpret the event as follows:

- unmerged pull requests exit successfully without creating a fragment;
- merged pull requests without the required label exit successfully;
- an empty required-label input means every merged pull request qualifies;
- qualifying pull requests require non-empty `## Narrative Context`,
  `## Narrative Decision`, and `## Narrative Consequences` sections;
- heading matching may be case-insensitive but must respect level-two section boundaries;
- missing evidence fails visibly instead of inventing rationale;
- the merge date supplies the fragment date;
- the pull-request title supplies a lowercase kebab-case slug, bounded to 72 characters, with a
  `pull-request-<number>` fallback;
- the configured summary limit is applied to the explicit Decision;
- evidence records the pull-request URL and merge commit;
- generated entries have kind `product` and status `accepted`;
- compilation runs after fragment creation.

At this stage, stop after producing the local fragment and compiled document. Do not configure Git,
push a branch, call the GitHub API, define `action.yml`, or add workflows.

## Security and test requirements

- Parse JSON with normal data APIs.
- Never put PR title, body, branch, or evidence into a shell command.
- Never log a token or include it in generated files.
- Test with temporary event files and repositories.
- Cover merged, unmerged, labelled, unlabelled, empty-label, missing-section, empty-section, unusual
  title, special-character prose, and slug-fallback cases.

## Acceptance criteria

- All event tests use disposable fixtures; this repository does not gain narrative configuration,
  fragments, or generated output.
- Skipped events make no filesystem changes and require no token.
- Qualifying events produce a valid fragment and byte-current compiled document.
- Missing explicit evidence fails with an actionable error.
- `npm run check` succeeds.

Commit the completed stage locally. Do not push.
