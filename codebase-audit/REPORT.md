# Audit Report Guide

Use the user's format when supplied. Otherwise write Markdown with the sections below.

## Prose

Write active, plain English. Prefer short words and sentences. Cut filler and stale figures of speech. Use technical terms when they add precision. Favor clear, natural prose over rigid rules.

State facts directly. Mark inference and uncertainty where they arise. Avoid fake precision.

## A. Executive Conclusion

State:

- overall technical and security posture
- top proven risks
- launch or operating advice, when relevant
- repair-path verdict and confidence
- claims that need live checks

## B. Scope, Method, and Limits

Name the revision, repo state, coverage, subagent roles, and key gaps.

## C. System and Trust Map

Give a concise map of architecture, data flow, privileged parts, and outside interfaces.

## D. Confirmed Findings

Order findings by severity. For each, include:

- ID, title, category, severity, and confidence
- concrete impact and preconditions
- exact evidence and full path
- why current controls fail
- smallest sound fix and a verification test

## E. Architecture and Change Assessment

Cover strengths to keep, structural risks with concrete effects, and proposed bounds or selective replacements.

## F. Repair-Path Decision

Give evidence, assumptions, assets to keep, a staged plan, and unknowns that could change the verdict.

## G. Production Readiness

Assess builds, tests, deploys, migrations, telemetry, recovery, runbooks, and open launch gates.

## H. Existing-Report Check

When an existing report is in scope, include a table with each claim, classification, evidence, and needed correction. Name claims to remove or soften. Supply exact replacement text for material corrections.

## I. Verification Results

List exact commands, exit results, counts, and skipped checks with reasons.

## J. Live Check Plan

List exact environment checks. Present unknowns as unknowns rather than confirmed gaps.

## K. Prioritized Fix Plan

Group work into:

- immediate containment
- near-term security and correctness fixes
- staged design changes
- clear non-priorities that limit rewrite or abstraction creep

## L. Rejected or Softened Leads

Name major claims rejected or reduced during review and give the reason. Keep weak claims out of the findings list.

When no actionable finding survives, say so. Then state remaining coverage gaps and live checks.
