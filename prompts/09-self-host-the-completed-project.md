# Prompt 9 — Make the completed processor self-hosting

Using the previously supplied repository contract, implement the final stage: make the completed,
audited Narrative repository consume its own local processor.

This is the first stage permitted to add Project Narrative's resulting decision record and
self-hosting governance. Use the implementation already built in this repository. Do not clone,
download, install, or invoke another Narrative repository.

## Add the local integration

Create:

- `.project-narrative.json` with schema version 1, a Narrative-specific title, standard fragment,
  preamble, and output paths, and the standard summary limit;
- `narrative/preamble.md`;
- `.github/pull_request_template.md` with change and classification guidance plus exact
  `## Narrative Context`, `## Narrative Decision`, and `## Narrative Consequences` headings;
- `.github/workflows/validate-narrative.yml`;
- `.github/workflows/maintain-narrative.yml`;
- the first authoritative fragment;
- generated `Narrative.md`.

Both workflows must invoke the checked-out local composite action with `uses: ./`, not a remote
action:

- validation runs only when configuration, fragments, the preamble, or `Narrative.md` changes and
  grants `contents: read`;
- maintenance runs for closed, merged pull requests, checks out full history, grants
  `contents: write` and `pull-requests: write`, and passes the repository `GITHUB_TOKEN`;
- maintenance remains gated by `narrative-required`.

## Bootstrap the adoption decision

The workflow is not present on the default branch yet, so it cannot capture the pull request that
installs itself.

Manually author one reviewed governance fragment recording:

- why a decision-history processor should follow its own published governance;
- the decision to use its local CLI, action, label, evidence headings, validation, and separate
  draft-proposal flow;
- the review and repository-settings obligations that follow;
- why the bootstrap fragment is necessary.

Compile `Narrative.md` with this repository's own `node bin/narrative.mjs compile`. Never hand-edit
the generated document. Review the fragment and generated diff separately.

## Add repository instructions

Extend the canonical `AGENTS.md` with the self-hosting authoring rules:

- meaningful product, architecture, governance, operational, correction, or experiment pull
  requests carry `narrative-required`;
- those PRs contain substantive Context, Decision, and Consequences under the exact headings;
- mechanical pull requests remove the headings and omit the label;
- fragments are authoritative and `Narrative.md` is generated;
- narrative-only proposal or repair pull requests omit the label to avoid recursion;
- human review of the separate draft accepts the wording.

Do not duplicate these rules across agent-specific instruction files; point them to `AGENTS.md`.

## Repository settings checklist

Document the manual GitHub settings that code cannot prove:

- create the exact `narrative-required` label;
- grant Actions read/write workflow permissions;
- allow GitHub Actions to create pull requests;
- permit the local action and checkout action;
- apply branch protection or rulesets appropriate to the repository;
- optionally require code-owner review for config, fragments, and generated output.

Do not claim those settings were changed unless they were actually verified on GitHub.

## Acceptance criteria

- `node bin/narrative.mjs validate` succeeds.
- `node bin/narrative.mjs check` proves `Narrative.md` is current.
- `npm run check`, `npm audit --audit-level=high`, and `git diff --check` succeed.
- The initial fragment accurately records the bootstrap decision and is not a changelog entry.
- Both workflows use the local completed action.
- The maintenance workflow will not recursively capture narrative-only proposals.
- Documentation clearly separates accepted project decisions from reviewed narrative wording.
- The final tree now contains the resulting built Narrative integration that every earlier stage
  deliberately omitted.

Commit locally but do not push. Explain to the learner that self-hosting becomes operational only
after this commit is published and merged to the default branch and the documented repository
settings are enabled.
