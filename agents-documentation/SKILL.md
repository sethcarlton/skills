---
name: agents-documentation
description: Guidance for writing minimal, progressive-disclosure AGENTS.md docs
disable-model-invocation: true
---

# AGENTS.md Guidance

Keep `AGENTS.md` small. Put only instructions relevant to nearly every task.

## Core Rules

- Include one-sentence project description.
- Include package manager if not obvious.
- Include non-standard build, test, lint, typecheck commands.
- Avoid long style guides, file maps, stale path lists, and obvious rules.
- Move domain-specific guidance into linked docs or skills.
- Prefer capability descriptions over brittle file paths.

## Progressive Disclosure

Use `AGENTS.md` as index, not manual.

```md
This is a React component library for accessible data visualization.
Use pnpm workspaces.
For TypeScript conventions, see docs/TYPESCRIPT.md.
For test strategy, see docs/TESTING.md.
```

## Monorepos

Use root `AGENTS.md` for shared repo guidance. Use nested `AGENTS.md` files for package-specific context.

## Cleanup Checklist

When refactoring agent docs:

1. Find contradictions and ask which rule wins.
2. Extract only root-level essentials.
3. Group remaining guidance into docs by domain.
4. Link from root to deeper docs.
5. Flag redundant, vague, obvious, or stale instructions for deletion.

## Placement

| Location | Use |
| --- | --- |
| Root `AGENTS.md` | Relevant to every task |
| Separate docs | Relevant to one domain |
| Nested docs | Package-specific or scoped guidance |
| Skills | Task-specific workflows loaded on demand |

Goal: small root context, discoverable details, no ball of mud.
