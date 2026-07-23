# Prompt 3 — Deterministic compiler and CLI

Using the previously supplied repository contract, implement Stage 3: rendering, compilation, stale
output detection, initialization, and the command-line interface.

Implement and export:

- `render(config, fragments)`;
- `compile(configPath, check = false)`;
- `init(configPath)`.

The rendered document must:

- begin with a generated-file warning;
- use the configured preamble, or a deterministic default when it is absent;
- contain an index with number, date, title, kind, and decision summary;
- escape table-breaking pipes and replace embedded newlines in cells;
- give each entry a durable anchor derived from its slug;
- derive displayed entry numbers from the sorted fragment order;
- reproduce each fragment's status and three-section body;
- end consistently so repeated compilation is byte-identical.

`compile` writes the configured output, creating its parent directory if necessary. In check mode it
must not write; it must fail when the output is missing or differs after normalizing line endings.

`init` must:

- create the default config only when missing;
- load the selected config;
- create the configured fragment and preamble directories;
- create a simple preamble only when missing;
- preserve existing configuration and preamble content;
- compile the initial output.

Add the commands:

- `narrative init`
- `narrative validate`
- `narrative compile`
- `narrative check`
- optional `--config <path>` for each command

Unknown commands and a missing `--config` value must print useful usage and exit non-zero. The CLI
must run both directly and through the symlink shape used by npm/npx.

## Acceptance criteria

- Unit tests prove deterministic rendering, escaping, durable anchors, and derived numbering.
- Integration tests invoke the executable in a temporary project for every command.
- Tests prove initialization is non-destructive and check mode detects stale output without fixing
  it.
- Two compilations from identical inputs are byte-for-byte identical.
- `npm run check` succeeds.
- Do not add GitHub automation or make this repository a consumer of itself.

Commit the completed stage locally. Do not push.
