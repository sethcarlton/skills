---
name: orchestrate-agents
description: Orchestrate work across agents, models, or harnesses. Use when a task should be delegated, parallelized, routed between Fable, Codex, Claude, opencode, Pi, or subagents, or when the user asks for an agent orchestration instruction, workflow, or routing policy.
---

# Orchestrate Agents

Use orchestration to keep the lead agent's context lean while other agents do bounded work. Do not make a team by reflex. Use one agent when the work is small, sequential, or needs one coherent judgment.

## Process

1. Decide whether orchestration earns its cost.
   Use orchestration only when the task has separable research, implementation, review, UI verification, codebase analysis, or high-stakes judgment. If the work is tightly coupled or mostly coordination, keep it in one thread. Complete this step when the reason for delegating is explicit.

2. Name the lead.
   The lead plans, decomposes, assigns work, keeps the shared context small, and synthesizes the result. Use the strongest reasoning model as lead only when planning quality matters more than token cost. Complete this step when the lead has a clear job and effort level.

3. Assign narrow roles.
   Route deep reasoning to a reasoning agent, mechanical edits and tests to a fast worker, UI or computer-use verification to Codex when available, and fresh-perspective review to a peer agent. Treat peer agents as independent thinkers, not rubber-stamp reviewers. Complete this step when every delegated role has one output to return.

4. Preserve independence.
   For high-stakes choices, ask two agents the same question without showing either one the other's answer, then have the lead compare the results. For routine work, delegate sequentially to avoid merge noise. Complete this step when independence is either protected or intentionally unnecessary.

5. Require terse returns.
   Each agent should report: what it changed or learned, why it matters, files or evidence touched, tests or checks run, and remaining risk. For code changes, prefer a per-file summary with the reason for each file. Complete this step when the lead can synthesize without rereading everything.

6. Synthesize, then verify.
   The lead chooses the answer, resolves conflicts, runs or requests the relevant checks, and calls out uncertainty. Do not finish with a pile of agent reports. Complete this step when there is one recommendation, patch, or decision with evidence.

## Routing Rules

- Keep routing policy in repo instructions, skills, or harness config, not in memory.
- Prefer high effort over max effort unless the task proves it needs more.
- Offload token-hungry work such as broad codebase analysis, computer use, and visual verification.
- Use parallel agents for independent judgment, not for work that shares mutable state.
- Prefer tools and harness primitives over custom agent frameworks until the workflow proves it needs more.
- Make any shared capability inventory explicit: which harness has which tools, MCPs, skills, commands, hooks, memories, and schedules.

## Prompt Shape

```text
Goal: <outcome>
Context: <files, constraints, risks>

You are the lead. Decide whether this needs delegation. If it does, assign narrow roles, keep agents independent where that matters, and ask each agent for a terse evidence-backed return. Show the plan first, then execute. Synthesize into one result and verify it.
```
