# NarrativeBuilder — Claude Code Instructions

> Read this file in full before changing anything. Every rule here is binding — regardless of which
> AI tool is reading it. This file is the single source of truth; `AGENTS.md`, `GEMINI.md`,
> `.github/copilot-instructions.md`, `.cursor/rules/`, `.windsurf/rules/`, and `.clinerules/` are
> thin pointers back to it (for Codex/OpenCode, Gemini CLI, GitHub Copilot, Cursor, Windsurf, and
> Cline respectively) so the rules never have to be kept in sync across multiple copies. Edit this
> file, not those.

Read [README.md](README.md) and [START-HERE.md](START-HERE.md) before your first change.

---

## §1 — What this repository is

A staged prompt sequence that teaches a learner to build **Project Narrative** — a deterministic,
review-first processor for a repository's decision history — using a coding agent. This repo
contains `prompts/`, not the processor. The processor itself lives in the `Narrative` repository.

Consequences for how you work here:

- A prompt is a specification handed to an agent working in a *different*, initially empty
  repository. It cannot assume anything exists that an earlier prompt did not create.
- Prompts are submitted in order, one per agent task, each only after the previous stage passed its
  acceptance checks. A change that reorders, renumbers, or splits a stage must keep that property
  true and must update `README.md`'s sequence listing in the same change.
- The reusable contract (`prompts/00-reusable-contract.md`) is submitted first and is referenced by
  every later stage. Changing it changes every stage; say so explicitly when you do.
- Never weaken an acceptance check to make a stage easier to pass. The sequence's value is that each
  boundary is real.

---

## §2 — Narrative discipline

This project exists to make decision history a reviewed artifact. Hold this repository to the
standard it teaches.

### Project Narrative is not yet installed on this repository

There is no `.project-narrative.json`, no `narrative/` directory, no `Narrative.md`, and no
maintenance or validation workflow here. That is a genuine gap in the builder for Project Narrative,
and it is recorded rather than hidden. Until it is closed, decisions about the prompt sequence live
only in pull-request history.

If you are asked to close it, the sequence's own Stage 12
(`prompts/12-one-command-install-and-agent-recipe.md`) describes the one-command consumer install;
apply it to this repository. The installing pull request must **not** carry `narrative-required`,
because the workflow cannot capture the pull request that first creates it.

### The contract every prompt must keep teaching

These are properties of the processor this sequence builds. A prompt that contradicts one of them is
wrong, and so is a prompt that leaves an agent able to satisfy it by accident:

- `Narrative.md` is **generated output**, and so is its index. It is never authored, hand-edited, or
  hand-merged. The only narrative file anyone writes by hand is a fragment under
  `narrative/entries/`. This holds for a merge conflict too: the correct resolution is to discard
  both sides and recompile, never to reconcile the markers, because fragments are the source of
  truth and merge cleanly while only the projection collides. Running the compiler is not authoring
  the file — compilation is deterministic and model-free, so the output is a function of the
  fragments and nothing else.
- A fragment is front matter plus non-empty `## Context`, `## Decision`, `## Consequences` sections,
  in that exact order. The plural on the last one matters; the validator rejects `## Consequence`.
- A decision-bearing pull request needs **two** things: the `narrative-required` label, and the
  headings `## Narrative Context`, `## Narrative Decision` and `## Narrative Consequences` in the
  pull-request **body**. A missing label makes the maintenance action exit silently; missing
  sections with the label present make it fail visibly.
- The maintenance action fires on the **merge event only**. Neither omission can be repaired by
  labelling afterwards — a missed entry has to be written by hand as a fragment.
- Supplying a pull-request body replaces the repository template wholesale. Doing that without
  carrying the three sections forward is the most common way an entry is silently lost.
- A narrative-only pull request carries no label, or it would recursively generate an entry about
  maintaining the narrative.
- Rationale is never invented from code or diffs. The processor proposes wording from evidence the
  author supplied, and a human accepts it.
- Slugs are durable identities; displayed entry numbers are derived and shift. Cite by slug.
- An accepted entry is never rewritten to read as though a later, better framing had been there all
  along. A reversal is a new entry of kind `correction` citing the original by slug — otherwise the
  record loses the evidence that the framing ever needed correcting.

---

## §3 — Git and review discipline

- Branch names follow `category/short-name` — `docs/`, `fix/`, `decision/`.
- **Do not stack a pull request on another pull request's branch.** If the base merges first, the
  stacked branch is orphaned: GitHub reports it merged while its commits never reach `main`.
- After pushing follow-up commits to a branch with an open pull request, say so explicitly. A pull
  request merged before later commits arrive drops them silently, and the merge looks clean.
- Verify what landed with `git log origin/main --oneline` after a merge rather than trusting the
  pull request's state.
- Commit or push only when asked. Never force-push a shared branch or delete a remote branch unless
  explicitly requested.

## §4 — Prose conventions

- Prompts wrap at 100 columns.
- No emoji.
- Absolute dates (`YYYY-MM-DD`), never relative ones.
- Do not report a skipped or unverifiable check as passing. If something cannot be executed in the
  current environment, label it a manual gate with the exact command and the expected result.
