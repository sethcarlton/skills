---
name: peer-review
description: Review code changes with two parallel reviewers and one aggregator, then report the findings by severity.
disable-model-invocation: true
---

# Peer Review

Load `/herdr`.

Use the user's guidance to focus the review on the named paths, changes, or risks.

## Choose The Changes

Use a PR, commit, branch, ref, or set of paths when the user gives one. Otherwise, use the first target below that exists and has a non-empty diff:

1. uncommitted changes
2. an open PR for the current branch, when one exists
3. the current branch against its direct parent branch, when you can identify it
4. the current branch against `main`
5. the last commit

Resolve the target and confirm its diff before you start.

## Start The Aggregator

Create one aggregator tab in the caller's workspace. Start an interactive Pi agent with GPT-5.6 at high reasoning so its progress remains visible.

Tell the aggregator to load `/herdr`, coordinate the review itself without invoking another review workflow, create two reviewer panes, and run these reviewers at the same time:

- GPT-5.6 at xhigh reasoning
- Claude Fable 5 at high reasoning

Start each reviewer as an interactive Pi agent so its progress remains visible. Save all three Pi sessions and record their session IDs.

Give each reviewer the same diff, target, user guidance, and review brief. Do not share one reviewer's work with another.

Review brief:

- Perform the review yourself. Do not invoke another review workflow or delegate it.
- Read every changed file in full and follow all repo rules that apply.
- Look for logic bugs, broken error handling, security flaws, races, and likely edge cases.
- Check whether the changes fit the nearby design and add work that can grow without a bound.
- Resolve doubt before reporting a finding. Ignore old code outside the change and matters of style alone.
- Report findings by severity with file and line references, harm, proof, and a clear fix when useful.
- Cover every changed file. If there are no findings, say so and list gaps in test cover.
- Do not edit files.

## Aggregate The Reviews

Tell the aggregator to wait for both reviewers and collect their full replies. It must merge findings with the same cause, keep sound findings reported by only one reviewer, and produce one report ordered by severity. The report must name which reviewers found each issue, then list open questions, assumptions, and gaps in test cover. It must also include all three Pi session IDs and a reopen command for each session.

Wait for the aggregator to finish and read only its final output. Do not read the two reviewer outputs.

After you have the full output and all session IDs, close the review tab.

Load the `/bro` skill and use it to restate the aggregator's result to the user in plain, brief language. Include the main findings and their severity, but do not repeat the full review. Keep the full details in the saved Pi sessions.

End with each agent name, its Pi session ID, and its reopen command so the user can inspect the full reviews:

```bash
pi --session <session-id>
```
