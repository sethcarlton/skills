---
name: dotfiles
description: Manage personal dotfiles through the `dot` CLI and bare Git repo. Use when asked to inspect, edit, add, diff, or commit dotfiles; install or configure tools and apps through `dot`; update tracked home config; or work with the `dot` or `dot git` commands.
---

## Overview

`dot` CLI manages my dotfiles via bare git repo at `~/.dotfiles` with `$HOME` work tree.

Use `dot -h` for full help & usage instructions.

General commands:
  - `git [args]`        Git wrapper for dotfiles repo
  - `tool [name]`       Install CLI tools
  - `cask [name]`       Install macOS applications
  - `configure [name]`  Configure tools

## Git Operations

Use `dot git <command>` for all dotfile version control — standard git, just prefixed.

**View tracked files:**

```bash
dot git ls-files                                  # cwd
dot git ls-tree -r --full-tree HEAD --name-only   # all
```

**Open code**

OpenCode config stored at `~/.config/opencode/`.

**Tracking new files:** Always prompt user first. Dotfiles repo is selective — not everything in `$HOME` should be tracked. Be very intentional with `dot git add -A`; prefer explicit paths.

**Commits:** Follow `committing` skill.
