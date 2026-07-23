# Prompt 4 — Fragment creation and safe summaries

Using the previously supplied repository contract, implement Stage 4: deterministic fragment
creation and bounded index summaries.

Implement and export `createFragment(config, entry)`.

It must:

- require a valid lowercase kebab-case slug;
- use the date without hyphens as the filename prefix;
- create the configured fragment directory;
- write the governed front matter and Context, Decision, and Consequences sections;
- JSON-quote user-derived title, summary, and optional evidence;
- default status to `proposed`;
- reject unsupported status values;
- trim section content but never execute or interpolate it as code;
- preserve a supplied supported kind for the normal validation path;
- avoid overwriting a collision by appending `-2`, `-3`, and so on to both filename and metadata
  slug.

Implement and export `summariseDecision(decision, maxCharacters)`.

It must:

- collapse whitespace and trim the decision;
- return it unchanged when already within the limit;
- prefer the last complete sentence that fits;
- otherwise truncate on a word boundary and add a single ellipsis character;
- never exceed the configured limit;
- behave sensibly for very small limits, long unbroken text, punctuation, and Unicode.

Round-trip every created fragment through the existing parser in tests.

## Acceptance criteria

- Tests prove the normal path, optional evidence, default and explicit status, collision handling,
  invalid input rejection, and inert handling of special characters.
- Summary tests cover exact boundary, sentence boundary, word boundary, no-space text, and tiny
  limits.
- Generated fragments validate without hand correction.
- `npm run check` succeeds.
- No GitHub event or self-hosting files are added.

Commit the completed stage locally. Do not push.
