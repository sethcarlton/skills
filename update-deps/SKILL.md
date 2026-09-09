---
name: update-deps
disable-model-invocation: true
description: Update dependencies with the project's package manager in small, related groups. Check and commit each group before continuing.
---

# Update Dependencies

Detect the project's package manager from its package metadata, lockfile, and instructions. Ask the user if the choice is unclear. Use that manager and its lockfile throughout; do not create a competing lockfile.

Update one small, related group at a time to the latest eligible versions, including those outside current semver ranges. Cover the root project and all workspaces.

Preserve catalogs, workspace references, and dependency types. Update catalog entries, not their references. Keep manifests and the lockfile in sync.

Respect the project's minimum release age policy unless the user explicitly says otherwise.

For each group:
1. Update dependencies and make small compatibility fixes as needed.
2. Run relevant checks and review the diff.
3. Commit only that group's changes before moving on. Leave unrelated work untouched.

After each commit, check again for updates. Defer complex or uncertain updates and continue until no simple, eligible updates remain. Summarize commits, checks, and deferred updates with reasons. Ask how to proceed with complex or uncertain updates. Do not deploy anything.
