# Reviewer Briefs

Use this file when preparing and judging the three independent reviews.

## Reviewer A: Architecture and Change

Check:

- real system bounds, dependency direction, ownership, and coupling
- fit of the domain model and where the code enforces invariants
- transaction bounds and consistency across modules
- APIs and contracts, including internal RPCs versus stable public APIs
- change spread, safe seams for tests, deploys, and replacement
- duplication or drift that causes a proven change or correctness risk
- correct use of framework lifecycle and state
- runtime and package support, plus limits on upgrades
- design blocks, dead ends, and strengths to keep
- evidence for staged repair, selective replacement, or full rewrite

File size, colocation, layering taste, and missing abstractions are not findings on their own. Tie each concern to observed harm.

## Reviewer B: Security and Trust

Build a threat model from the repo. Check what applies:

- auth, sessions, token checks, recovery, signup, and invites
- object, role, and tenant permissions on each sensitive path
- caller input traced to protected actions
- database policies, grants, privileged functions, tenant consistency, and raw-write bypasses
- service or admin credentials and privileged clients
- injection, request forgery, traversal, unsafe decoding, XSS, CSRF, and open redirects
- uploads, MIME trust, object access, quotas, malware checks, and orphaned writes
- webhook signatures, replay protection, machine auth, scopes, and rate limits
- secrets, environment values, cryptography, sensitive logs, and outside data transfer
- package advisories whose affected behavior is reachable in the installed version
- security headers and version-specific same-origin behavior

Separate facts proved by the repo from live config that needs a check. Treat identifiers and token secrecy as preconditions, not permission checks.

## Reviewer C: Integrity, Reliability, and Operations

Check what applies:

- domain invariants, state changes, and bypass routes
- atomic multi-write work, races, idempotency, retries, and partial failure
- numeric precision, rounding, cumulative limits, immutable history, and audit trails
- error flow, ignored storage or network failures, and false success states
- timeouts, resource limits, pagination, backpressure, caches, and unbounded work
- job delivery, order, duplicate handling, and poison messages
- repeatable builds, safe migrations, rollback, CI gates, and deploy order
- telemetry, health checks, alerts, backups, restore steps, and incident runbooks found in the repo
- tests of risky behavior rather than raw coverage totals
- concrete runtime, accessibility, or user-facing frontend failures

Tie frontend structure, effects, data libraries, and request patterns to a correctness, accessibility, or visible speed failure before reporting them.

## Rules for Every Reviewer

- Stay skeptical in both directions: seek major gaps and test claims that may be too strong.
- Report behavior and harm, not taste. Unusual code can be sound.
- Recommend an abstraction only when shared callers, testable domain rules, harmful drift, a transaction bound, or a concrete bug supports it.
- Accept documented framework features unless their use is unsafe or wrong for the installed version.
- Label source facts, reasoned inferences, and deploy-dependent unknowns.
- Treat live ACLs, identity settings, storage privacy, WAF rules, telemetry, backups, deployed secrets, and infrastructure as unknown unless the repo proves them.
- Base severity on impact, reach, realistic preconditions, and current controls.
- Trace security and integrity claims from caller input or event, through checks and permissions, to the protected action or broken invariant.
- Turn a package advisory into an app finding only after proving the affected version, runtime context, reachable behavior, and impact.
- Prefer the smallest fix that removes the cause. Keep sound framework features and boundaries.
- Search the repo for similar cases after finding a pattern.
- State sampled areas and gaps. Match coverage claims to files actually checked.
- Keep the repo read-only.

## Required Response

Return all six sections:

1. **Coverage** — folders, entry points, flows, and commands checked; clear gaps.
2. **Findings** — for each lead:
   - stable candidate ID and title
   - proposed severity and confidence
   - concrete impact and realistic preconditions
   - exact file and line evidence
   - full code or failure path
   - current controls and why they fail
   - smallest sound fix
   - check or regression-test idea
3. **Strengths and controls** — parts that lower risk or support reuse.
4. **Live unknowns** — each with an exact check.
5. **Change assessment** — parts to keep, repair, or replace, with evidence.
6. **Rejected leads** — suspicious patterns checked but not supported.

State when an area has no actionable issue.
