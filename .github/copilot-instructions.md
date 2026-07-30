# NarrativeBuilder — Agent Instructions

This repository's binding AI agent instructions live in [`CLAUDE.md`](../CLAUDE.md) at the repo
root. Read it in full before generating any code or proposing any change — every rule there is
binding, regardless of which AI tool is being used.

`CLAUDE.md` is the single source of truth. This file, and the equivalent files for other tools
(`AGENTS.md`, `GEMINI.md`, `.cursor/rules/`, `.windsurf/rules/`, `.clinerules/`), are thin pointers
to it — kept that way deliberately so the rules never have to be kept in sync across multiple
copies. If you are updating the instructions, edit `CLAUDE.md`, not this file.

Note in particular that this repository teaches Project Narrative but does not yet run it on itself,
and that in a consumer repository an entry is lost unless a pull request carries both the
`narrative-required` label and the three `## Narrative …` sections in its body. `CLAUDE.md` §2 has
the detail.

See also [`README.md`](../README.md) and [`START-HERE.md`](../START-HERE.md) for what this sequence
builds and in what order.
