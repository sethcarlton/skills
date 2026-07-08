---
name: orchestrate-agents
description: Orchestration for delegating or routing work across agents, models, or harnesses. Use when a task needs parallel agents, independent judgment, broad codebase analysis, UI verification, or a routing policy for Fable, Codex, Claude, opencode, Pi, or subagents.
---

# Orchestrate Agents

Orchestration keeps lead context lean while bounded agents do separable work. Do not make a team by reflex. Use one agent when work is small, sequential, or needs one coherent judgment.

## Defaults

- Lead: Fable 5 orchestrator when available.
- Task workers: `gpt-5.5 high` unless the role clearly needs another model, tool, or harness.
- Effort: high by default; max only when evidence shows high is insufficient.
- Mechanical work: use faster or cheaper agents when quality risk is low and verification is cheap.
- Computer or visual work: route to the harness with the required UI, browser, or screen tools.

## Process

1. Decide whether orchestration earns its cost.
   Use orchestration only for separable research, implementation, review, UI verification, codebase analysis, or high-stakes judgment. If work is tightly coupled or mostly coordination, keep it in one thread. Gate: delegation reason named, or single-agent path chosen.

2. Name the lead.
   Lead plans, decomposes, assigns work, keeps shared context small, and synthesizes. Use the default lead unless planning quality, tool access, or cost says otherwise. Gate: lead, effort, and job are explicit.

3. Assign narrow roles.
   Give each agent one bounded output. Route deep reasoning to a reasoning agent, mechanical edits and tests to a fast worker, UI or computer-use verification to the capable harness, and fresh review to a peer agent. Gate: every role has one returnable artifact or answer.

4. Preserve independence.
   For high-stakes choices, ask two agents the same question without showing either the other's answer, then have the lead compare. For routine mutable work, delegate sequentially to avoid merge noise. Gate: independence protected or intentionally unnecessary.

5. Require terse returns.
   Require each agent to report: result, why it matters, files or evidence touched, checks run, and remaining risk. For code changes, prefer per-file summary with reason. Gate: lead can synthesize without rereading full transcripts.

6. Synthesize, then verify.
   Lead chooses one answer, resolves conflicts, runs or requests relevant checks, and calls out uncertainty. Do not finish with a pile of agent reports. Gate: one recommendation, patch, or decision has evidence.

## Routing Rules

- Keep routing policy in repo instructions, skills, or harness config, not in memory.
- Offload token-hungry work such as broad codebase analysis, computer use, and visual verification.
- Use parallel agents for independent judgment, not for work that shares mutable state.
- Prefer tools and harness primitives over custom agent frameworks until the workflow proves it needs more.
- Make any shared capability inventory explicit: which harness has which tools, MCPs, skills, commands, hooks, memories, and schedules.

## Prompt Shape

```text
Goal: <outcome>
Context: <files, constraints, risks>
Defaults: Lead with Fable 5 orchestrator when available. Use gpt-5.5 high for task workers unless a role needs another model, tool, or harness.

You are the lead. Decide whether orchestration earns its cost. If yes, assign narrow roles, preserve independence where it matters, and require terse evidence-backed returns. Show the plan first, execute, synthesize one result, and verify it.
```
