---
name: dev-processes
description: Run dev servers, tests, builds, long-running commands, and watch processes in the appropriate multiplexer
---

Use a multiplexer for dev servers, tests, builds, long-running commands, and watch processes. Use descriptive labels such as `dev`, `test`, and `build`.

## Herdr

When `HERDR_ENV=1`, explicitly load the `herdr` skill and follow it for all process management. Herdr is the primary multiplexer in this environment; do not use tmux.

Before starting a process, inspect the existing panes and read the output of likely matches. Reuse a matching live process or report its existing output instead of starting a duplicate. If no matching process exists, create a Herdr pane according to the Herdr skill, label it `dev`, `test`, `build`, or another concise purpose, and run the command there.

## tmux fallback

When `HERDR_ENV` is not `1`, use tmux during the migration period. Use descriptive window names such as `dev`, `test`, and `build`.

If not in tmux (`$TMUX` empty), ensure a session exists:

```bash
SESSION="$(basename "$PWD" | tr . _)"
tmux has-session -t "$SESSION" 2>/dev/null || tmux new-session -d -s "$SESSION" -c "$PWD"
```

Check whether the window exists before creating it. If it exists, inspect its output instead of restarting it:

```bash
tmux select-window -t "$SESSION:dev" 2>/dev/null   # check exists
tmux capture-pane -pt "$SESSION:dev" -S -100       # inspect output
tmux neww -dn dev -t "$SESSION" 'bun run dev'      # create only when absent
```

## Turborepo

Always use `--ui=stream` so output remains readable in managed background panes:

```bash
turbo run dev --ui=stream
bun run dev -- --ui=stream
```

## SST

Use mono mode to avoid SST's nested multiplexer UI:

```bash
sst dev --mode=mono
bun sst dev --mode=mono
```
