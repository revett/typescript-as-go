# AGENTS.md

## Rules

Critically the following rules, must be followed when working in this project:

- 100 character line length limit for all files in the project
- List items (bullets or numbered) must be a single sentence, feel free to make use of commas and
  semicolons
- List items (bullets or numbered) should not end in a full stop

## Versions

The skill is versioned with semantic versioning, and `main` is the release channel, so it must
always equal the latest tagged release:

- Every pull request must bump the version, no exceptions
- Keep the two version stamps in lockstep, the git tag (`vX.Y.Z`, with the `v`) and the
  `metadata.version` frontmatter field in `SKILL.md` (`X.Y.Z`, without the `v`)

## Changlog

Every pull request must also update `CHANGELOG.md`, in step with the version bump above:

- Add a new `## vX.Y.Z` heading at the top of the entries, matching the new version, newest first
- Record the change as one or more list items describing what changed and why, in plain language
- Never edit or delete a released entry, the changelog is append only history
