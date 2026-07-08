---
name: de-slop
description: De-slop code by deleting fake structure with proof. Use when asked to simplify code, collapse wrapper/helper/component/state/effect structure, or inline one-use types/schemas.
---

# De-Slop

Delete fake structure.

Default: smaller, clearer, more boring code with same observable behavior.
Every deletion is a proof obligation.
Boundary rule: keep abstractions only when they protect a real boundary.

## Steps

1. Pin behavior.
   Name public entry points, expected outputs, errors, side effects, persistence changes, logs, async ordering, resource lifecycle, serialization, React hook identity where relevant, and the proof check.
   Completion: the before/after behavior surface is explicit enough to compare.

2. Hunt fake structure.
   Scan touched files for abstractions that fail the Boundary Test, plus stale comments, generic names, and framework machinery leaking into domain code.
   If nontrivial `useEffect` replacement is needed, invoke `react-effects`; this skill only identifies effect slop.
   Completion: every touched file has been checked against the Boundary Test and Slop Examples.

3. Pick one cut.
   Choose the candidate with the clearest payoff, highest confidence, and lowest behavior risk.
   Skip aesthetic-only changes and changes whose behavior cannot be proven.
   Completion: one simplification target is chosen, with the proof needed to accept it.

4. State the proof.
   Before editing, write the equivalence check: same outputs, errors, ordering, side effects, logs, metrics, persistence, serialization, resource lifecycle, type errors, validation messages, and React hook identity where relevant.
   Completion: the proof would catch the most likely behavior regression.

5. Cut narrowly.
   Make one kind of deletion or collapse at a time.
   Do not rewrite the module, change public API or behavior, delete suspected dead code without proof, or add an abstraction to reduce lines.
   Completion: the diff removes structure while preserving the pinned behavior.

6. Verify.
   Run focused tests, typecheck, lint, or manual checks that prove the change.
   If proof is missing, leave the change unapplied and report the candidate.
   Completion: verification passed, or the unproven simplification is not landed.

## Boundary Test

Before keeping or adding a helper, component, class, hook, provider, state variable, type alias, interface, schema, constant, option bag, or barrel, ask:

- Does it protect an accessibility, behavior, lifecycle, layout, performance, domain, module, API, validation, or type-safety boundary?
- Does it remove a concept from the caller instead of renaming or repackaging it?
- Does it match existing project components, stores, forms, schemas, and patterns?
- Is it reused by independent callers or exported across a boundary?
- Would inlining make code more direct without weakening errors, validation messages, type inference, or ownership?

If mostly no, delete it and inline the idea.
If yes, keep it and name the boundary.

## Slop Examples

- Helpers that rename one call, forward arguments, or wrap one operation.
- `utils`, `helpers`, `services`, or barrel rollups that hide ownership or create import churn.
- Components with no accessibility boundary, behavior boundary, layout ownership, expensive subtree isolation, domain meaning, or design-system integration.
- Local classes that only hold static methods or duplicate plain functions and objects.
- Custom components that conflict with shadcn/ui or the local component system instead of composing it.
- Mirrored props, stored derived state, parallel booleans, duplicate handlers, and state better owned by a form, URL, router, server, framework data API, existing store, or render derivation.
- Manual form state rebuilt with scattered `useState` handlers when form state, server action state, or native form behavior would make ownership clearer.
- Raw `useEffect` or effect-driven state.
- Hoisted one-use type aliases, interfaces, schemas, constants, or Zod sub-schemas.
- Types that mirror generated API types, schema input/output, or a local object once.
- `Pick`, `Omit`, and wrapper aliases used only to avoid writing one local shape.
- Behavior flags, vague option bags, and generic names.
- Comments that are leftover plans, obvious narration, or claims the code no longer proves.

## Biases

Prefer:

- Plain functions and objects.
- Domain names over generic names.
- Explicit inputs, outputs, failures, and side effects.
- Straight-line code over clever chains.
- Fewer concepts, not merely fewer lines.
- Existing project patterns over local inventions.
- One type source: API boundary, schema, or local literal.

Avoid:

- Broad rewrites.
- Cosmetic churn.
- Nested ternaries.
- Boolean flags that change semantics.
- Moving complexity instead of deleting it.
- Removing abstractions that protect a real boundary.

## Stop

Stop and ask for direction when:

- The simplification changes public API or behavior.
- The proof surface is missing.
- The diff grows beyond the original simplification target.
- Two attempts fail to preserve behavior.
- The best fix is architectural rather than local.

## Report

End with:

- what structure was deleted or collapsed
- what boundary it failed
- why behavior is preserved
- what proof ran
- what slop remains, if any
