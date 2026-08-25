# Prompt 8 — Independent reconstruction audit

Use this prompt as a fresh task or, ideally, with a different coding agent.

Audit the completed Project Narrative processor before it is allowed to consume itself. Do not add
new features unless needed to correct a confirmed defect.

Verify independently:

1. Configuration defaults and schema-version failures are deterministic.
2. Fragment front matter rejects malformed, duplicate, missing, or unsupported values.
3. Dates are real, slugs and filenames agree, and required sections are non-empty and ordered.
4. Duplicate slugs fail and fragment ordering never depends on filesystem order.
5. Compilation is byte-identical and stale-output check mode never silently writes.
6. Initialization preserves existing configuration and preamble files.
7. CLI commands work directly and through an npm-style executable symlink.
8. Fragment collisions remain unique without corrupting metadata.
9. Summaries stay within bounds without careless sentence or word cuts.
10. Unmerged and unlabelled events have no side effects.
11. Qualifying events cannot proceed without explicit Context, Decision, and Consequences.
12. Pull-request prose is never executed or placed into Git command arguments.
13. Reruns update one predictable branch and do not create duplicate draft pull requests.
14. Tokens are neither logged nor written to fragments, commits, examples, or errors.
15. Action inputs, README examples, package metadata, and executable behavior agree.
16. Validation is local, dependency-free, network-free, model-free, and deterministic.

Run:

- a clean dependency installation;
- `npm run check`;
- `npm audit --audit-level=high`;
- `npm pack --dry-run`;
- `git diff --check`;
- the action tests against a local fake API, never a live repository.

Report findings by severity with exact file and line references. Fix confirmed defects, rerun the
verification, and provide a requirements-to-evidence table.

Confirm explicitly that the repository still does not contain self-hosting configuration,
fragments, generated `Narrative.md`, or local maintenance/validation workflows. Those are added only
after this audit passes.

Commit audit fixes locally if needed. Do not push or merge.
