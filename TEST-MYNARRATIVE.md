# Test your freshly built MyNarrative in a throwaway repository

Use this guide after you have built, audited, and self-hosted your Project Narrative processor in
`MyNarrative`. It walks through proving the whole two-pull-request lifecycle end to end in a small,
disposable consumer repository so you can see the automation work before you adopt it into a
repository you care about.

This is a rehearsal, not a build stage. Nothing here modifies `MyNarrative`; you install the
processor you already built into a fresh repository called `TestMyNarrative`, drive a real
decision-bearing pull request through it, and watch the follow-up narrative pull request appear.

For a permanent adoption into a real project, use
[prompts/10-adopt-freshly-built-narrative.md](prompts/10-adopt-freshly-built-narrative.md) instead.
This guide is the low-stakes version you can safely throw away afterwards.

## Before you start

You need:

- a completed, reviewed `MyNarrative` processor that is published and reachable by GitHub Actions;
- the exact reviewed commit SHA of that processor (never a moving branch such as `main`);
- one connected coding agent, set up as described in [START-HERE.md](START-HERE.md).

Throughout this guide, replace the placeholders with your own values:

- `<NARRATIVE-OWNER>` — the account or organisation that owns your built `MyNarrative` processor;
- `<NARRATIVE-REPOSITORY>` — its repository name (for example `MyNarrative`);
- `<REVIEWED-COMMIT-SHA>` — the exact reviewed commit of the processor to run;
- `<LOCAL-NARRATIVE-CLI>` — the local command that invokes your built `bin/narrative.mjs`, if you
  have the processor available locally.

## 1. Create the `TestMyNarrative` repository

1. Sign in to GitHub and open [Create a new repository](https://github.com/new).
2. Choose your account or organisation as the owner.
3. Enter `TestMyNarrative` as the repository name.
4. Add a description such as `Throwaway repo for testing MyNarrative end to end`.
5. Choose **Private** while testing.
6. Select **Add a README file** so the repository is not empty.
7. Leave `.gitignore` and licence unselected.
8. Select **Create repository**.

Connect your coding agent to `TestMyNarrative` only — not to `MyNarrative` and not to
`NarrativeBuilder`. If you work locally, clone `TestMyNarrative` and open that folder. If you work in
the cloud, authorise only `TestMyNarrative`.

You can delete this repository once the test is complete, so it is safe to experiment here.

## 2. Create the required label

The maintenance workflow only proposes a narrative entry when a merged pull request carries an exact
opt-in label. Create it before testing so the workflow has something to match.

Give your agent this prompt:

```text
In this repository, create a GitHub label with the exact name narrative-required (lower case, single
hyphen, no trailing space). Give it a short description such as "Opt this PR into a Narrative entry"
and any colour. Confirm the label exists and report its exact name. Do not create any other labels.
```

The label name is an interface. `narrative-required` must match exactly — `Narrative-Required` or
`narrative_required` will silently prevent the automation from running.

## 3. Add MyNarrative to this repository

Now install your freshly built processor as a consumer of `TestMyNarrative`. Give your agent this
prompt, with the placeholders replaced:

```text
Add MyNarrative to this repo.

Use my freshly built Project Narrative processor at this exact reviewed action:

    <NARRATIVE-OWNER>/<NARRATIVE-REPOSITORY>@<REVIEWED-COMMIT-SHA>

Use this local CLI when deterministic initialization or compilation is required:

    <LOCAL-NARRATIVE-CLI>

Do not copy the processor source into this repository and do not substitute a different
implementation.

Install the consumer files:
- .project-narrative.json using schema version 1, a "TestMyNarrative" title, and standard fragment,
  preamble, and output paths;
- narrative/preamble.md;
- one manually reviewed bootstrap fragment recording the decision to adopt MyNarrative here;
- generated Narrative.md, produced only by running the local CLI (never hand-edited);
- .github/workflows/maintain-narrative.yml that runs when a pull request closes, continues only when
  it was merged, checks out full history, grants contents: write and pull-requests: write, invokes
  the reviewed action SHA, passes ${{ secrets.GITHUB_TOKEN }}, and requires the exact
  narrative-required label;
- .github/workflows/validate-narrative.yml that runs on pull requests touching
  .project-narrative.json, narrative/**, or Narrative.md, grants only contents: read, and invokes
  the same reviewed action SHA in check mode;
- the exact required headings (## Narrative Context, ## Narrative Decision, ## Narrative
  Consequences) plus classification guidance in the pull-request template.

Commit the bootstrap fragment and generated Narrative.md together. Report the files changed, the
pinned SHA, and any repository settings I still need to confirm by hand.
```

Review the diff before merging. In particular confirm that `Narrative.md` was generated by the CLI
and not written by hand, and that the workflows pin the reviewed SHA rather than a branch.

Merge this installation pull request to the default branch. The maintenance workflow cannot capture
the pull request that adds it, which is exactly why the bootstrap fragment records the adoption
decision manually.

### Confirm GitHub Actions settings

Before the automation can open a pull request, confirm these repository settings by hand:

1. Open **Settings → Actions → General → Workflow permissions**.
2. Select **Read and write permissions**.
3. Enable **Allow GitHub Actions to create and approve pull requests**.

Without both, the maintenance workflow cannot open the follow-up narrative pull request.

## 4. Update README.md to say this repo uses MyNarrative

Now make a small, genuine, decision-bearing change so there is something real for the automation to
narrate. Give your agent this prompt:

```text
Update README.md to say that this repo uses MyNarrative. Add a short section explaining that decision
history for this repository is maintained by MyNarrative (Project Narrative), that meaningful
decisions opt in with the narrative-required label and supply Narrative Context, Decision, and
Consequences, and that a generated Narrative.md is the compiled record. Make the change on a new
branch. Do not open the pull request yet.
```

This is the kind of decision-bearing change the processor is meant to capture: it records a real
choice about how the repository is maintained.

## 5. Create the pull request

Open the project pull request with all three narrative sections filled in. Give your agent this
prompt:

```text
Create the PR for the README change on this branch.

Fill in all three required sections in the pull-request body with substantive content:

## Narrative Context
Explain why we are recording that this repository uses MyNarrative.

## Narrative Decision
State the decision to adopt MyNarrative for decision history and document it in the README.

## Narrative Consequences
Describe what this means for contributors going forward.

Apply the exact narrative-required label to this PR. Ensure any normal checks pass. Report the PR
number and the label you applied. Do not merge it yet — I will review and merge it myself.
```

Review the pull request, confirm the `narrative-required` label is present and the three sections are
substantive, then merge it yourself.

## 6. Watch the follow-up narrative pull request appear

This is the behaviour the whole test is meant to prove:

**Merging a pull request that is marked `narrative-required` does not write the narrative entry
directly. It triggers the maintenance workflow, which opens a *second*, follow-up pull request
containing the proposed narrative fragment and the regenerated `Narrative.md`.**

The lifecycle is deliberately two steps:

1. Your **project pull request** (the README change) carried `narrative-required` and supplied
   explicit Context, Decision, and Consequences. Merging it accepted the *project* decision.
2. That merge started the maintenance workflow, which created a narrative fragment and regenerated
   `Narrative.md`, then opened a separate **follow-up draft pull request**.
3. A human reviews that follow-up pull request and merges it to accept the *wording* of the narrative
   entry.

The two reviews are separate on purpose: merging the first pull request accepts the decision;
merging the second accepts how the decision is worded in the permanent record.

After merging the project pull request:

1. Open the **Actions** tab and confirm the maintenance workflow ran.
2. If GitHub shows **Approve workflows to run** on the follow-up pull request, a maintainer with
   write access must inspect the workflow source and approve those runs. Because the follow-up pull
   request is created with the repository's own `GITHUB_TOKEN`, its checks start in an
   approval-required state and will not run on their own.
3. Confirm exactly one follow-up draft pull request was opened, containing a new fragment and an
   updated `Narrative.md`.
4. Review the fragment, its evidence, and the regenerated document.
5. Confirm the follow-up pull request does **not** carry `narrative-required` — if it did, merging it
   could recursively generate yet another narrative proposal.
6. Merge the follow-up pull request only after you are satisfied with its wording.

## 7. What a successful test looks like

- `TestMyNarrative` uses your freshly built processor pinned at the exact reviewed SHA.
- The `narrative-required` label exists with its exact name.
- Merging the labelled README pull request produced exactly one follow-up narrative pull request.
- The follow-up pull request contained a generated fragment and a byte-current `Narrative.md`.
- The follow-up pull request did not carry `narrative-required`.
- You approved the maintenance and validation workflow runs by hand where GitHub required it.

If any of these did not happen, check, in order: the exact label name, the workflow permissions
settings, whether a maintainer still needs to approve the queued workflow runs, and whether the
project pull request actually carried the label when it was merged.

## 8. Clean up

Once you have seen the follow-up pull request appear and merged it, you can delete `TestMyNarrative`
entirely. The point of this repository is the rehearsal, not the contents. When you are ready to use
your processor for real, follow
[prompts/10-adopt-freshly-built-narrative.md](prompts/10-adopt-freshly-built-narrative.md) in a
repository you intend to keep.
