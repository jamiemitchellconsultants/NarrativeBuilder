# Prompt 1 — Project scaffold and configuration

Using the previously supplied repository contract, implement Stage 1: the dependency-free Node.js
project scaffold and configuration contract.

Create:

- `package.json` using ESM, Node.js 20 or newer, and no runtime dependencies;
- a `narrative` executable mapped to `bin/narrative.mjs`;
- `test` and `check` scripts using Node's built-in test runner and syntax checker;
- `bin`, `action`, `test`, and `examples` directories as needed;
- `.gitignore` and an MIT licence.

In `bin/narrative.mjs`, define and export the default configuration:

- `schemaVersion`: `1`
- `title`: `Project Narrative`
- `fragments`: `narrative/entries`
- `preamble`: `narrative/preamble.md`
- `output`: `Narrative.md`
- `summaryMaxCharacters`: `240`

Implement `loadConfig(path)`:

- return a fresh copy of the defaults when the file is absent;
- parse a present JSON file;
- reject any schema version other than `1`;
- merge supported overrides onto the defaults;
- allow malformed JSON and filesystem errors to fail visibly.

Do not implement fragment parsing, compilation, CLI commands, GitHub behavior, workflows, or a
repository narrative yet.

## Acceptance criteria

- `npm install` succeeds without adding runtime packages.
- `npm run check` succeeds.
- Tests prove missing-file defaults, valid overrides, invalid schema rejection, and malformed JSON
  rejection.
- The package declares the executable and minimum Node version.
- The repository does not yet contain `.project-narrative.json`, `Narrative.md`, `narrative/`,
  or Narrative maintenance workflows.

Commit the completed stage locally with a focused message, but do not push.
