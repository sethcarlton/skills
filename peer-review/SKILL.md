---
name: peer-review
description: Run three independent bug-focused Pi reviews through Herdr using GPT-5.6 at high and xhigh reasoning plus Claude Fable 5, then correlate their findings.
disable-model-invocation: true
---

# Peer Review

Use Herdr to run three independent Pi reviewers, then correlate their evidence-backed defects. Invoking this skill requests Herdr orchestration.

## 1. Pin The Change

Use the user's PR, commit, branch, ref, or paths when supplied. Otherwise review uncommitted changes; if none exist, review the last commit.

Confirm the target resolves and the diff is non-empty before starting reviewers. Capture the exact target, diff command, repository root, and any user guidance for reuse in each prompt.

## 2. Verify Herdr

Before any Herdr command, verify this agent runs inside Herdr:

```bash
test "${HERDR_ENV:-}" = 1
```

If it fails, say peer review requires a Herdr-managed pane and stop. Use the installed CLI as the authority; inspect `herdr --help`, `herdr pane`, or `herdr agent` when syntax is uncertain.

## 3. Start Three Pi Reviewers

Create three sibling panes in the current tab. Preserve the caller's working directory with `--cwd "$PWD"` and the user's focus with `--no-focus`. Inspect the current layout and split panes in directions that keep all four panes usable. Parse pane IDs from Herdr's JSON responses; never infer them.

Choose unique agent names for the run, then start these Pi agents:

```bash
herdr agent start <gpt-high-name> --kind pi --pane <pane-id> -- \
  --provider openai-codex --model gpt-5.6-sol --thinking high --ro

herdr agent start <gpt-xhigh-name> --kind pi --pane <pane-id> -- \
  --provider openai-codex --model gpt-5.6-sol --thinking xhigh --ro

herdr agent start <fable-name> --kind pi --pane <pane-id> -- \
  --provider anthropic --model claude-fable-5 --ro
```

Start the three agents concurrently when the available tool surface permits it. If `--ro` is unavailable, use Pi's available read-only controls and explicitly prohibit edits in the prompt.

## 4. Review Independently

Send all three agents the same self-contained prompt concurrently with `herdr agent prompt <name> <prompt> --wait --timeout <ms>`. Include:

- the repository root and exact review target
- the diff command and user guidance
- an instruction to read every full changed file and applicable repository guidance
- an instruction not to edit files
- the review brief below

Review brief:

- Focus first on logic errors, broken error handling, security defects, races, and realistic edge cases.
- Check structural fit and obvious unbounded performance problems.
- Investigate uncertainty instead of reporting speculation.
- Ignore unrelated pre-existing code and style-only preferences.
- Report findings by severity with file and line references, impact, evidence, and a concrete fix when useful.
- Account for every changed file. If there are no findings, say so and list residual testing gaps.

Do not share one reviewer's analysis with another.

If an agent blocks or a wait fails, inspect it with `herdr agent get` and `herdr agent read` before acting. Do not treat `unknown` as completion.

## 5. Collect And Correlate

Read each completed response with:

```bash
herdr agent read <name> --source recent-unwrapped --lines 200
```

If terminal scrollback omits part of a response, ask that reviewer to write its complete report as Markdown in a temporary directory and reply only with the path, then read the file directly.

Merge duplicate findings that share one root cause. Preserve a valid finding reported by only one reviewer; agreement raises confidence but is not required.

Report findings first, ordered by severity. For each finding, note which reviewer or reviewers found it. Include open questions or assumptions afterward. If no findings remain, state that explicitly and identify residual testing gaps.

Leave the reviewer panes open so the user can inspect them. Do not close panes or stop agents unless the user asks.
