---
name: dev-processes
description: Run dev servers, tests, builds, long-running commands, and watch processes in Herdr
---

Use Herdr for dev servers, tests, builds, long-running commands, and watch processes. Explicitly load the `herdr` skill and follow it for all process management.

If `HERDR_ENV` is not `1`, report that Herdr process management is unavailable and stop. Do not fall back to another multiplexer.

Before starting a process, inspect the existing panes and read the output of likely matches. Reuse a matching live process or report its existing output instead of starting a duplicate. If no matching process exists, create a Herdr pane according to the Herdr skill and run the command there.

## Cleanup

After a one-off task or when a Herdr tab is no longer needed, capture relevant output and close it unless another pane is still in use.

## Tabbed task runners

For Turborepo and similar task runners with an interactive or tabbed UI, use their plain streaming mode so output remains readable in Herdr. For Turborepo, always pass `--ui=stream`:

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
