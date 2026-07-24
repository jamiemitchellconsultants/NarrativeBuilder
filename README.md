# NarrativeBuilder

A staged prompt sequence for learning how to build Project Narrative with a coding agent.

Project Narrative is a deterministic, review-first processor for maintaining a repository's
decision history. The learner builds the processor itself: a dependency-free Node.js CLI, governed
Markdown fragments, deterministic compilation, and a GitHub Action that proposes reviewable
narrative entries after qualifying pull requests merge.

## New to GitHub or coding agents?

Start with [Start here: set up your repository and coding agent](START-HERE.md). It walks through
creating `MyNarrative`, cloning it locally, choosing one coding agent, connecting that agent to the
correct repository, and proving the setup with a read-only task.

## How to use the prompts

Use the same coding-agent task throughout when possible. Submit the reusable contract first, then
submit each implementation stage only after the previous stage has passed its acceptance checks.

1. [Reusable contract](prompts/00-reusable-contract.md)
2. [Project scaffold and configuration](prompts/01-project-scaffold-and-configuration.md)
3. [Governed fragment parser](prompts/02-governed-fragment-parser.md)
4. [Deterministic compiler and CLI](prompts/03-deterministic-compiler-and-cli.md)
5. [Fragment creation and safe summaries](prompts/04-fragment-creation-and-summaries.md)
6. [Merged pull-request interpretation](prompts/05-pull-request-interpretation.md)
7. [Proposal branch and draft pull request](prompts/06-proposal-automation.md)
8. [Composite action, documentation, and governance](prompts/07-delivery-documentation-and-governance.md)
9. [Independent reconstruction audit](prompts/08-independent-reconstruction-audit.md)
10. [Make the completed processor self-hosting](prompts/09-self-host-the-completed-project.md)

The sequence deliberately builds from local, deterministic behavior toward GitHub automation. Each
stage asks for executable evidence before the next begins. Do not paste every implementation prompt
into one message: the pauses are there so the learner can inspect the diff, run the commands, and
understand the increment.

The completed processor is not installed as a consumer of itself during the build. Prompt 9 is the
only stage that adds Project Narrative configuration, fragments, generated `Narrative.md`,
maintenance workflows, and Narrative-aware pull-request governance to the learner's repository.
That ordering avoids assuming the tool exists before the learner has built and audited it.

## Use your completed Narrative in another repository

After the build and self-hosting sequence is complete, use the optional
[existing-repository adoption prompt](prompts/10-adopt-freshly-built-narrative.md) in a coding-agent
task connected to a different repository. Replace its placeholders with the location and reviewed
commit SHA of the Narrative processor you just built.

This is a consumer example, not another build stage. It explicitly guides the installation pull
request, GitHub Actions permissions and approvals, and the two-pull-request lifecycle: merging a
qualifying project PR causes the workflow to open a separate draft PR containing the proposed
narrative entry. Workflows on that automation-created follow-up PR enter an approval-required state
and must be approved by someone with repository write access.

## Test your completed Narrative end to end first

Before adopting into a repository you care about, rehearse the whole lifecycle in a throwaway
repository. [Test your freshly built MyNarrative](TEST-MYNARRATIVE.md) walks through creating a
disposable `TestMyNarrative` repository, creating the `narrative-required` label, installing your
processor, making a real decision-bearing change, opening the labelled project PR, and watching the
maintenance workflow open the separate follow-up PR that carries the proposed narrative fragment.

## When a labelled pull request has no narrative

A pull request that carries `narrative-required` has opted in to supplying Context, Decision, and
Consequences. The maintenance workflow reads those three sections from the merged pull request's
description; if they are missing, or written as ordinary prose instead of the exact `## Narrative
Context`, `## Narrative Decision`, and `## Narrative Consequences` headings, the workflow has no
explicit decision to interpret. Because that requirement is checked as the pull request merges, the
omission surfaces as a failed maintenance run on a pull request that is already merged, and no
narrative proposal is produced for the decision. A repository can additionally add a pre-merge gate
that rejects the same omission earlier, but a contributor still needs a way to bring a labelled pull
request's description back into shape when it does not yet carry the required sections.

The [fix a narrative-required pull request](prompts/11-fix-narrative-required-pull-request.md)
prompt is a coding-agent task for exactly that situation. Like the adoption prompt, it is an
operational example rather than a build stage, and it names no specific processor because teams run
their own customised clones.
