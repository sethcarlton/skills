---
name: de-slop
description: De-slop agent-written or recently modified code by deleting fake structure with proof. Use when asked to simplify, refactor for clarity, reduce duplication, remove extraneous helpers/components/classes, cut code smells, collapse wrappers, reduce React state, or simplify effect-heavy components.
---

# De-Slop

Delete fake structure.

Default: make code smaller, clearer, and more boring without changing what it does.
Every simplification is a proof obligation: the smaller program must be observably equivalent to the larger one.

## Steps

1. Pin behavior.
   Name the public entry points, expected outputs, errors, side effects, persistence changes, logs, async behavior, and tests or manual checks that prove them.
   Completion: the behavior that must survive is explicit enough to compare before and after.

2. Hunt slop.
   Find extraneous functions, convenience rollups, components for the sake of components, local classes, duplicated state, raw `useEffect`, tangled handlers, stale comments, behavior flags, vague option bags, generic names, and framework machinery leaking into domain code.
   Completion: every touched file has been checked for these slop types.

3. Pick one lever.
   Choose the candidate with the clearest payoff, highest confidence, and lowest behavior risk.
   Skip changes that are only aesthetic or cannot be proven.
   Completion: one simplification target is chosen, with the proof needed to accept it.

4. State the proof.
   Before editing, write the equivalence check: same outputs, errors, ordering, side effects, logs, metrics, serialization, resource lifecycle, and React hook identity where relevant.
   Completion: the proof would catch the most likely behavior regression.

5. Edit narrowly.
   Make one kind of simplification at a time.
   Do not rewrite the module, change public behavior, delete suspected dead code without proof, or introduce a new abstraction just to reduce lines.
   Completion: the diff removes or collapses structure while preserving the pinned behavior.

6. Verify.
   Run the focused tests, typecheck, lint, or manual checks that prove the change.
   If proof is missing, stop and report the candidate instead of landing it.
   Completion: verification passed, or the unproven simplification was left unapplied.

## Slop To Delete

- Helper functions that only rename one call, forward arguments, or create a fake abstraction.
- Convenience rollups such as broad `utils`, `helpers`, `services`, barrel exports, or "one import to rule them all" files that hide ownership or create import churn.
- Components with no accessibility boundary, behavior boundary, layout ownership, expensive subtree isolation, or domain meaning.
- Local classes that only hold static methods, wrap one operation, or duplicate what plain functions and objects already express.
- Custom components that conflict with shadcn/ui or the local component system instead of composing it.
- Mirrored props, derived state stored separately, parallel booleans, duplicate handlers, and state that belongs in a form, URL, router, server, or existing store.
- Raw `useEffect` or effect-driven state. For any nontrivial effect change, use `react-effects`; this skill only flags the slop, that skill chooses the replacement.
- Manual form state rebuilt with scattered `useState` handlers when form state, server action state, or native form behavior would make ownership clearer.
- Comments that are leftover plans, obvious narration, or claims the code no longer proves.

## Biases

Prefer:

- Plain functions and objects.
- Domain names over generic names.
- Explicit inputs, outputs, failures, and side effects.
- Straight-line code over clever chains.
- Fewer concepts, not merely fewer lines.
- Existing project components and patterns over local inventions.

Avoid:

- Broad rewrites.
- Cosmetic churn.
- Nested ternaries.
- Boolean flags that change semantics.
- Generic helpers with one caller.
- Moving complexity instead of deleting it.
- Removing abstractions that protect a real boundary.

## React Bias

Prefer fewer components, fewer state variables, and almost no raw `useEffect`.

Use components only when they create a real boundary: accessibility, reusable behavior, layout ownership, expensive subtree isolation, domain meaning, or integration with the design system.

Use state only for values that cannot be derived during render and must survive across interactions.
Prefer form state, URL or router state, server or framework data, and derived render values when they fit.

## Abstraction Test

Before keeping or adding a helper, component, class, hook, provider, or state variable, ask:

- Does this create a real boundary?
- Does it remove a concept from the caller?
- Does it match the project's existing component, form, and state patterns?
- Would deleting it make the code more direct?

If the answer is mostly no, delete the abstraction and inline the idea.

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
- why behavior is preserved
- what proof ran
- what slop remains, if any
