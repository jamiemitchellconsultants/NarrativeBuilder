# Prompt 2 — Governed fragment parser

Using the previously supplied repository contract, implement Stage 2: parsing and validation for
authoritative narrative fragments.

A fragment is a Markdown file with constrained front matter followed by exactly these non-empty
level-two sections in order:

1. `## Context`
2. `## Decision`
3. `## Consequences`

Require these metadata fields:

- `date`: a real calendar date in `YYYY-MM-DD`;
- `slug`: lowercase kebab-case;
- `title`;
- `summary`;
- `kind`;
- `status`.

Allow kinds:

- `architecture`
- `product`
- `governance`
- `operational`
- `correction`
- `experiment`

Allow statuses:

- `proposed`
- `accepted`
- `superseded`

Implement and export `parseFragment(file, raw, config)` and `loadFragments(config)`.

Validation must:

- normalize CRLF and CR line endings before parsing;
- reject missing front matter, malformed lines, duplicate metadata keys, and missing fields;
- support plain scalar values and JSON-quoted strings without evaluating code;
- reject impossible dates, invalid slugs, unsupported kinds or statuses, and overlong summaries;
- require the filename's slug, after its numeric date prefix, to match the metadata slug;
- reject missing, empty, duplicated, or reordered required sections;
- reject duplicate slugs across files;
- ignore non-Markdown files;
- produce deterministic order by date and then filename, independent of filesystem enumeration.

Use only Node built-ins. Treat every fragment as untrusted text.

## Acceptance criteria

- Tests cover a valid fragment and every validation category above.
- Tests demonstrate deterministic ordering and duplicate-slug rejection.
- A title or section containing shell metacharacters remains inert text.
- `npm run check` succeeds.
- No compilation, CLI commands, GitHub API calls, or self-hosted narrative files exist yet.

Commit the completed stage locally. Do not push.
