---
name: react-effects
description: Avoid React useEffect. Use when reviewing/refactoring React components, removing effects, or choosing render derivation, events, keys, lifted state, framework data APIs, useSyncExternalStore, or rare external sync.
---

# React Effects

Default: no raw `useEffect`.

`useEffect` is escape hatch for non-React synchronization. Prove need before keep/add.

## Ladder

Try in order:

1. Derived value -> compute during render. No prop/state mirror.
2. Expensive pure compute -> compute during render. Use `useMemo` only when measured or project guidance says.
3. User action -> event handler. Use shared helper for multiple handlers.
4. Submit, POST, notification -> event handler, not Effect.
5. Whole subtree reset -> split component; pass `key`.
6. Partial reset -> store ID/minimal state; derive selected value. Last resort: same-component render update behind prev-value guard.
7. Parent notification -> callback in same interaction, or lift/control state.
8. Child data passed up -> own data in parent; pass down.
9. Data fetching -> framework/router/server data API. If impossible, custom hook. Raw fetch Effect only with stale-response cleanup.
10. External store -> `useSyncExternalStore`.
11. App init -> entry/module init or guarded idempotent root logic.
12. External system sync -> consider raw `useEffect`.

Completion: first matching replacement removed Effect, or no replacement fits.

## Never Keep

- Redundant render state.
- Event-specific logic.
- Effect chains that trigger each other.
- Child Effect updating parent.
- One-time init protected only by `[]`.
- Async Effect without stale-response handling.
- Subscription, timer, or listener without cleanup.

## Rare Keep Proof

Keep raw `useEffect` only if all true:

- Names non-React external system.
- Names sync direction.
- Explains why ladder alternatives fail.
- Has cleanup or idempotency.
- Handles async races when async.
- Stable dependencies pass lint without suppression.

## Completion Check

No raw `useEffect` exists without rare-keep proof.
