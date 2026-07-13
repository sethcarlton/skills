---
name: peer-review
description: Run three independent bug-focused reviews and correlate their findings.
disable-model-invocation: true
---

# Peer Review

Use three independent reviewers to find evidence-backed defects, then correlate their results.

## 1. Pin The Change

Use the user's PR, commit, branch, ref, or paths when supplied. Otherwise review uncommitted changes; if none exist, review the last commit.

Confirm the target resolves and the diff is non-empty before delegating.

## 2. Review In Parallel

Launch three `reviewer` subagents in one parallel tool call. Give each the same target and user guidance, but do not share one reviewer's analysis with the other.

Both reviewers must:

- read every full changed file and applicable repository guidance
- focus first on logic errors, broken error handling, security defects, races, and realistic edge cases
- check structural fit and obvious unbounded performance problems
- investigate uncertainty rather than reporting speculation
- ignore unrelated pre-existing code and style-only preferences
- report findings by severity with file and line references, impact, and a concrete fix when useful

Completion: both reviewers independently account for every changed file.

## 3. Correlate

Merge duplicate findings that share one root cause. Preserve a valid finding reported by only one reviewer; agreement raises confidence but is not required.

Report findings first, ordered by severity. Include open questions or assumptions afterward. If no findings remain, state that explicitly and identify residual testing gaps.
