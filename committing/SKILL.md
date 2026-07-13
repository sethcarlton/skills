---
name: committing
description: Guidelines for git commit messages
---

Commit only when user requests or as part of an explicit defined task.

Before writing the message, inspect recent commits and follow the repository's established convention.

## Subject

- Use an appropriate prefix: `fix:`, `feat:`, `docs:`, `wip:`, etc.
- Describe the user-visible outcome, not the files or mechanics changed.
- Be specific and brief; the subject should work as a release-note entry.
- Avoid generic claims such as "improve agent experience."

## Body

For nontrivial changes, add a short body explaining the motivation, user impact, or important tradeoff that the diff does not make obvious. Do not repeat the subject or narrate the diff.

Include verification details or issue references only when useful. Trivial commits may remain one line.
