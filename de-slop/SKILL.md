---
name: de-slop
description: De-slop code by deleting fake structure with proof. Use when asked to simplify code, collapse wrapper/helper/component/state/effect structure, remove React effects, or inline one-use types/schemas.
---

# De-Slop

Delete fake structure without changing observable behavior.

Lint owns syntax-level anti-slop policy. This skill handles what lint cannot judge: boundaries, ownership, lifecycle, state, and needless concepts.

## Workflow

1. **Set the scope and pin behavior.** Use the scope the user named. If none was named, use the current worktree changes and their affected callers. List the files in scope, then name the public entry points, outputs, errors, side effects, ordering, persistence, serialization, resource lifecycle, and React identity that must remain stable. Choose the checks that prove them.

2. **Check the baseline.** Inspect every file in scope and run the repository's check or lint command, such as `bun check`, `bun run check`, or `bun run lint`. Fix in-scope anti-slop findings at their source. Do not suppress rules, weaken configuration, launder types, or add fake abstractions to pass lint. Record unrelated existing failures.

3. **Hunt the whole scope.** Apply the Boundary Test and Hunt list to every file in scope. Keep an inventory of proven candidates and candidates that lack enough evidence.

4. **Cut until exhausted.** Work through every proven candidate, one narrow cut at a time. After each cut, inspect affected callers and update the inventory. Do not mix in API changes, broad rewrites, or cosmetic churn. Revert any cut whose behavior cannot be proved and report it instead.

5. **Verify completion.** Run focused checks after risky cuts, then the repository's full check or lint command. Re-scan every file in scope. Finish only when no proven fake structure remains in scope; report anything left and why.

## Boundary Test

Keep an abstraction only when it protects a real accessibility, behavior, lifecycle, layout, performance, domain, module, API, validation, ownership, or type-safety boundary.

Ask:

- Does it remove a concept from the caller rather than rename or repackage it?
- Does it have independent callers, external ownership, or a clear project convention?
- Would inlining make the code more direct without weakening behavior, errors, validation, inference, or ownership?

If no, delete it and inline the idea. If yes, keep it and name the boundary.

## Hunt

Look for:

- Helpers that rename one call, forward arguments, or wrap one operation.
- Generic `utils`, `helpers`, or `services` modules and barrel files that hide ownership.
- Components, hooks, providers, or classes with no distinct behavior, lifecycle, layout, accessibility, performance, domain, or module boundary.
- Mirrored props, stored derived state, parallel booleans, duplicate handlers, vague option bags, and state owned by the wrong layer.
- Manual form state that a form, server action, or native form can own.
- Raw `useEffect` and effect-driven state.
- One-use types, interfaces, schemas, constants, and aliases that obscure a local idea or duplicate an existing type source.
- Leftover plans, obvious narration, generic names, and comments that claim more than the code proves.

## React Effects

Follow React's [You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect). An Effect is an escape hatch for synchronizing with a system outside React, not a general data-flow tool.

Before keeping an Effect:

- Derive values during render. Use `useMemo` only for an expensive pure calculation.
- Put work caused by an interaction in its event handler and update related state in that same interaction.
- Reset a subtree with a `key`. For a partial reset, store the smallest stable fact, such as an ID, and derive the rest. A guarded same-component render adjustment is a last resort; never update another component during render.
- Lift shared state. Let the parent own shared or fetched data instead of using a child Effect to update it.
- Prefer framework, router, or server data APIs. If an Effect must fetch, clean up stale requests and prevent races.
- Use `useSyncExternalStore` for external stores instead of copying them into React state.
- Run app-wide initialization at the entry point or module boundary.

Keep an Effect only when rendering must synchronize with a named external system or perform work because the component became visible. State the system and sync direction. Ensure cleanup or idempotence, handle races, and satisfy dependency lint without suppression. Remove Effect chains unless each step independently synchronizes with an external system.

Treat configured anti-slop lint findings as evidence that a value lost its owner, contract, or type information. Prefer inference, `satisfies`, an existing owner contract, boundary parsing, direct typed access, and genuine dependency seams. Do not duplicate the lint rules here.

## Guardrails

- Preserve public API and behavior.
- Prefer fewer concepts, not merely fewer lines.
- Prefer plain functions, objects, domain names, and straight-line code.
- Do not delete suspected dead code without proof.
- Do not replace one fake abstraction with another.
- Stop when the proof surface is missing, the work escapes the agreed scope, two attempts at one candidate fail, or the real fix is architectural.

## Report

End with:

- what was deleted or collapsed
- what boundary it failed
- why behavior is preserved
- what checks ran and their result
- what slop remains and why, if any
