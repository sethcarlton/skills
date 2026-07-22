---
name: codebase-audit
description: Audit a codebase for architecture, security, integrity, reliability, production readiness, and salvageability.
compatibility: Requires HERDR_ENV=1 and the herdr CLI.
disable-model-invocation: true
---

# Codebase Audit

Act as an independent senior reviewer of architecture, app security, and production readiness. Favor sound evidence over a long findings list.

The current thread is the **orchestrator**. It sets scope, maps the repo, delegates reviews, checks each lead, assigns severity, decides whether to repair or replace the system, and writes the report. Subagents supply leads and independent analysis.

Keep the review read-only unless the user asks for edits. Redact secret values from all output.

## 1. Set Scope

Record:

- repo root and exact revision
- whether to run a fresh audit, check an existing report, or both
- key business flows, deployment model, threat model, deadlines, and limits
- requested report path and format

When the user omits these details, audit the current checkout and state that choice. Preserve uncommitted work. Ask before switching revisions, installing packages, changing lockfiles, running migrations against shared systems, or starting services that may change data.

When an existing report is in scope, read all of it before delegation. Treat each claim as unproven.

**Complete when:** the scope, revision, repo state, assumptions, and output target are explicit.

## 2. Check Herdr

Load and follow the `herdr` skill before using Herdr. Run:

```bash
test "${HERDR_ENV:-}" = 1
```

If the check fails, ask the user to rerun the audit inside Herdr and stop.

Use `herdr --help` and the relevant command help as the syntax source before changes. Target panes by explicit ID or `--current`, parse IDs from command output, and keep focus in the orchestrator pane. Close only resources made for this audit.

Use the agent executable the user names. Otherwise use the orchestrator's executable, then fall back to `pi`. Start agents in sibling panes, wait for each prompt, then send work with `herdr pane run`. Pass task prompts after launch rather than as launch arguments.

**Complete when:** Herdr is active, its command syntax is known, and the agent executable is set.

## 3. Map the Repo

Read all repo guidance. Map what applies:

- languages, frameworks, package managers, and runtime versions
- source, generated, vendored, fixture, and build-output bounds
- entry points, routes, RPCs, jobs, workers, CLIs, and schedules
- client/server and internal/external trust bounds
- auth, permissions, tenant selection, and privileged clients
- schemas, migrations, policies, triggers, stored code, and data access
- core entities, invariants, flows, and state changes
- outside services, webhooks, uploads, AI providers, and payments
- config, environment values, secret loading, and deploy files
- dependencies, scripts, tests, CI, releases, telemetry, runbooks, backups, and restore plans
- repo size and review coverage, using suitable file and line counts

Record Git status and the exact revision. Exclude generated and vendored code from normal source-quality review. Review how the system creates and trusts that code when it affects risk.

Run safe checks the repo already supports, such as build, typecheck, lint, tests, dependency audit, and migration checks. Prefer documented commands. Record each command, exit status, key output, and reason for any skip. A check passes only after a successful run. Use a normal Herdr command pane for long tasks when useful.

Prepare a shared reviewer brief with:

- scope, revision, and repo state
- repo map and stack versions
- repo guidance
- checks run and their results
- risky entry points and key flows
- user goals and limits
- existing report path and claim list, when present

**Complete when:** the map covers each relevant item above and the shared brief lets a reviewer start without repeating basic discovery.

## 4. Run Independent Reviews

Read [`REVIEWERS.md`](REVIEWERS.md) in full before drafting reviewer prompts. It defines the three review roles, shared rules, and response schema.

Start all three reviewers in parallel when panes and resources allow. Otherwise use waves. Give each reviewer the shared brief, its role brief, the shared rules, and the response schema. Keep first-pass reviews independent: do not share one reviewer's conclusions with another.

**Complete when:** all three reviewers return the full schema, account for their stated scope, and name all gaps.

## 5. Check Every Lead

Collect reviewer transcripts through Herdr. Treat each finding as a lead until the orchestrator checks it.

For every lead:

1. Read cited code in context, plus relevant callers, callees, policies, migrations, tests, and framework config.
2. Trace the full path or reproduce it with a focused safe check.
3. Search the whole repo for the same pattern.
4. Find controls or constraints the reviewer may have missed.
5. Merge symptoms that share one cause.
6. Remove claims that are stylistic, speculative, stale, unreachable, or based only on a deploy assumption.
7. Set severity and confidence from the checked evidence.

Keep a finding only when evidence shows:

- the trigger, input, or system state
- the trust or consistency bound
- the path to a protected action or concrete failure
- realistic impact and preconditions
- why current controls fail

Ask a different subagent to challenge each proposed Critical or High finding and each rewrite recommendation. Give it the exact claim and evidence. Ask for counterevidence and narrower readings, then settle the result yourself. Agreement raises confidence; it does not replace this check.

For an existing report, classify every factual claim as **Confirmed**, **Confirmed with qualification**, **Unsupported**, **Incorrect**, or **Stale**. Check counts, commands, file names, line refs, advisory totals, attack paths, and advice. Keep corrections visible rather than silently replacing claims.

**Complete when:** each lead is accepted, merged, softened, or rejected with a recorded reason; each high-risk claim has an independent challenge.

## 6. Set Severity and Confidence

Severity measures impact and the conditions needed to cause it:

- **Critical** — broad compromise, catastrophic data loss or corruption, or similar harm under ordinary conditions with little privilege or user action.
- **High** — serious unauthorized access, lasting integrity loss, or a broad outage under realistic conditions.
- **Medium** — meaningful but limited harm to security, correctness, reliability, or operations.
- **Low** — small concrete harm or a defense gap with a plausible path.

Hard fixes do not raise severity. Maintainability concerns need a concrete failure mode before they become findings.

Confidence measures evidence quality:

- **High** — source or reproduction establishes the claim.
- **Medium** — strong code evidence depends on one stated environment fact.
- **Low** — the claim still needs proof; report it as an open question in most cases.

**Complete when:** every accepted finding has a severity and confidence that match these definitions.

## 7. Choose a Repair Path

Choose one result:

1. **Continue and harden** — the base is sound and risks are local.
2. **Incrementally rehabilitate** — major flaws exist, but safe seams support staged fixes.
3. **Selectively replace** — replace bounded parts and keep the rest.
4. **Rewrite** — the current system cannot meet the stated goals through safe, cost-effective change.

Base the choice on:

- build and release repeatability
- core domain and data-model correctness
- consistent security bounds
- data and migration recovery
- runtime and dependency support
- coupling and safe seams for change
- characterization tests and ability to check changes
- failure handling and recovery
- the user's business limits

A rewrite needs the strongest proof. Recommend it only when deep flaws cross most core bounds, staged repair lacks safe seams, and replacement poses less risk after accounting for feature parity, data migration, security regression, and parallel operation. Style, weak test coverage, old packages, or a few severe bugs do not prove this case.

State the choice and confidence, evidence and assumptions, parts to keep, parts to fix or replace, a staged risk-reduction plan, and facts that could change the choice. Use ranges or qualitative terms when the evidence cannot support exact cost.

**Complete when:** one result follows from checked evidence and names the facts that could reverse it.

## 8. Check Outside Claims

Use outside sources only to check framework behavior, support status, standards, or advisories. Match the installed version. Prefer primary sources. Cite the URL and access date. Separate source facts from conclusions about this app. Newer docs do not establish behavior in an older installed version.

**Complete when:** each outside claim has a version-matched primary source or is marked as uncertain.

## 9. Write the Report

Read [`REPORT.md`](REPORT.md) in full before writing or editing the report. It defines the default sections, required details, and prose rules.

If the user asks to edit an existing report, preserve its requested format and style. Make evidence-backed changes and list each material change afterward.

**Complete when:** every accepted finding, rejected major lead, check result, scope gap, live unknown, and repair-path claim appears in the report, with no secret values exposed.
