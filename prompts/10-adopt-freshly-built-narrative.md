# Example prompt — Adopt your freshly built Narrative in an existing repository

Use this prompt only after completing and auditing your Narrative processor. Start a coding-agent
task in the **existing consumer repository**, not in NarrativeBuilder and not in the Narrative
processor repository.

Before submitting it, replace:

- `<NARRATIVE-OWNER>` with the GitHub account or organisation that owns your freshly built
  processor;
- `<NARRATIVE-REPOSITORY>` with its repository name;
- `<REVIEWED-COMMIT-SHA>` with the exact reviewed commit to run;
- `<CONSUMER-PROJECT-NAME>` with the existing repository's project name;
- `<LOCAL-NARRATIVE-CLI>` with a local command that invokes the freshly built
  `bin/narrative.mjs`, if the consumer is available locally.

Do not use an unreviewed moving branch such as `main` in the workflows. The processor repository
must already be published and accessible to the consumer repository's GitHub Actions. If it is
private, confirm the organisation and repository settings explicitly allow the consumer to use its
action.

---

You are adding my freshly built Project Narrative processor to this existing repository.

Use this exact reviewed action:

```text
<NARRATIVE-OWNER>/<NARRATIVE-REPOSITORY>@<REVIEWED-COMMIT-SHA>
```

Use this local CLI command when deterministic initialization or compilation is required:

```text
<LOCAL-NARRATIVE-CLI>
```

Do not copy the processor source into this repository and do not substitute a different Narrative
implementation.

## Inspect before editing

Read the existing repository instructions, contribution guide, pull-request template, workflows,
branch policies, and package tooling. Also read the freshly built processor's `README.md`,
`action.yml`, and configuration contract at the reviewed commit.

State:

- which existing files will be extended rather than replaced;
- whether workflow YAML already has related triggers or permissions;
- how the local CLI will be invoked;
- whether the action is public or requires private-action access;
- any repository settings that a file diff cannot verify.

Preserve unrelated project conventions and existing pull-request sections.

## Install the consumer files

Add:

- `.project-narrative.json` using schema version 1, a `<CONSUMER-PROJECT-NAME>` title, standard
  fragment, preamble, and output paths, and the standard summary limit;
- `narrative/preamble.md`;
- one manually reviewed bootstrap governance fragment recording the decision to adopt this
  processor;
- generated `Narrative.md`;
- `.github/workflows/maintain-narrative.yml`;
- `.github/workflows/validate-narrative.yml`;
- Narrative classification guidance and the exact required headings in the existing pull-request
  template.

The headings are an interface and must be exactly:

```markdown
## Narrative Context

## Narrative Decision

## Narrative Consequences
```

Meaningful product, architecture, governance, operational, correction, and experimental decisions
use the exact `narrative-required` label and substantive content under all three headings.
Mechanical changes omit the label and remove the three sections.

Do not hand-edit `Narrative.md`. Create the bootstrap fragment, then compile with my freshly built
local CLI. Commit the fragment and generated document together.

## Add the workflows

The maintenance workflow must:

- run when a pull request closes;
- continue only when the pull request was merged;
- check out full history;
- grant `contents: write` and `pull-requests: write`;
- invoke
  `<NARRATIVE-OWNER>/<NARRATIVE-REPOSITORY>@<REVIEWED-COMMIT-SHA>`;
- pass `${{ secrets.GITHUB_TOKEN }}`;
- require the exact `narrative-required` label.

The validation workflow must:

- run for pull requests that change `.project-narrative.json`, `narrative/**`, or `Narrative.md`;
- grant only `contents: read`;
- invoke the same reviewed action SHA in `check` mode.

Do not claim the integration is operational merely because these YAML files exist.

## Explain and verify the two-pull-request lifecycle

Call this out prominently in the README or contribution guide and in your final report:

1. A decision-bearing **project PR** carries `narrative-required` and supplies explicit Narrative
   Context, Decision, and Consequences.
2. Merging that project PR accepts the project decision and starts the maintenance workflow.
3. The workflow creates a fragment and regenerated `Narrative.md`, then opens a separate
   **follow-up draft PR**.
4. A human reviews that follow-up PR and merges it to accept the wording of the narrative entry.
5. The follow-up narrative PR must not carry `narrative-required`, otherwise it can recursively
   generate another narrative proposal.

Do not describe the first merge as automatically accepting the generated narrative wording. The
second PR and its human review are deliberate governance boundaries.

## Require GitHub Actions approval and settings checks

Create a checklist for the repository owner and stop short of claiming success until it is
confirmed. Use GitHub's current documentation for
[workflow permissions](https://docs.github.com/en/organizations/managing-organization-settings/disabling-or-limiting-github-actions-for-your-organization)
and
[`GITHUB_TOKEN`-created pull-request runs](https://docs.github.com/en/actions/concepts/security/github_token)
if the interface differs:

- create the exact `narrative-required` label;
- open **Settings → Actions → General → Workflow permissions**;
- select **Read and write permissions**;
- enable **Allow GitHub Actions to create and approve pull requests**;
- allow the checkout action and my freshly built Narrative action;
- when the automation-created follow-up PR shows **Approve workflows to run**, a repository
  maintainer with write access must inspect the workflow source and explicitly approve those runs;
- if another repository or organisation policy presents **Approve and run workflows**, follow the
  same inspect-then-approve rule;
- confirm organisation rules do not block actions from the processor repository;
- apply appropriate branch protection or rulesets;
- optionally require code-owner review for the configuration, fragments, and generated output.

Be explicit that workflow runs can remain queued or skipped until a maintainer approves them, and
that without write permission plus permission to create pull requests the maintenance workflow
cannot open the follow-up narrative PR.

Because the maintenance workflow creates the follow-up pull request using the repository's own
`GITHUB_TOKEN`, its `pull_request` workflows are created in an approval-required state. They do not
run until someone with repository write access selects **Approve workflows to run**. Do not treat a
queued or absent validation result as a pass: inspect the deterministic diff, run the freshly built
CLI's `check` command locally, and approve the GitHub workflow before merging.

## Bootstrap and end-to-end verification

The workflow is not yet on the default branch, so it cannot capture the installation PR that adds
it. Record adoption with the manually reviewed bootstrap fragment and merge the installation before
testing automation.

After the installation is on the default branch and the settings are approved:

1. Create a small, genuine, reversible decision-bearing change on a new branch.
2. Open a project PR with all three substantive Narrative sections.
3. Apply `narrative-required`.
4. Ensure normal checks pass and merge it.
5. Inspect and, if GitHub requests it, explicitly approve the maintenance workflow run.
6. Confirm it opens one follow-up draft narrative PR.
7. Review the fragment, evidence, summary, and generated `Narrative.md`.
8. Select **Approve workflows to run** on the automation-created follow-up PR and confirm the
   deterministic validation check passes.
9. Keep `narrative-required` off the follow-up PR.
10. Merge the follow-up PR only after human review accepts its wording.

## Acceptance criteria

- The consumer uses my freshly built processor at the exact reviewed SHA.
- The existing repository's instructions and pull-request template are extended without losing
  unrelated content.
- The bootstrap fragment validates and `Narrative.md` is byte-current.
- Maintenance and validation permissions are least-privilege for their roles.
- Documentation explains the separate project PR and follow-up narrative PR.
- Documentation says clearly that required GitHub Actions runs need maintainer approval.
- The repository-settings checklist distinguishes verified settings from unverified manual work.
- Local validation and existing project tests pass.
- `git diff --check` succeeds.

Report the files changed, commands and tests run, the pinned processor SHA, settings still awaiting
human confirmation, and the exact next manual action. Do not push, change repository settings,
approve a workflow, or merge either PR unless I explicitly ask.
