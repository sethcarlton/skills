---
name: create-spec
description: create-spec
---

Create a simple spec in .specs/.

Ask user for:
1) Goal
2) Definition of complete / validation (must include lint + typecheck)
3) Supporting refs? (shot examples, documentation, skills, links; optional)
4) Add tests? (yes/no, default no if blank)
5) Backward compatible? (yes/no, default no if blank)

Always include lint and typecheck in validation requirements.

Do not ask for title. Derive a concise summarized title from goal.

Title rules:
- 4-8 words, Title Case.
- Keep core action + object + scope.
- Drop filler words and extra clauses.

Use filename pattern: spec-<short-kebab-summary>.md.

Filename rules:
- Derive from summarized title (not full goal text).
- 3-6 meaningful words, lowercase kebab-case.
- Remove stop words (a, an, the, with, and, to, of, for, in, on, our, needs).
- Target <= 40 chars after `spec-`; hard max 60 by further summarizing.
- If goal is empty, use: spec-untitled-YYYYMMDD-HHmmss.md.

Example:
- Goal: add gateway with default configuration but with authentication. bind it to our api...
- Title: Add Authenticated AI Gateway API Proxy
- Filename: spec-ai-gateway-api-proxy.md

After collecting required inputs, run a brief brainstorming pass.
If you spot high-impact ideas, risks, or ambiguity, ask up to 3 concise optional questions and suggest up to 3 concrete improvements.
Keep this non-blocking: if user skips or leaves blank, proceed with sensible defaults and note assumptions.
Do not expand scope without explicit user confirmation.

Use this spec format:

```md
# Spec: <summarized derived title>

## Goal
<goal>

## Supporting References
- <shot examples / docs / skills / links> (optional)

## Requirements Checklist
- [ ] Validation: <definition of complete / verification method + lint + typecheck>
- [ ] Automated tests are required. (only if tests=yes)
- [ ] Develop implementation using the tdd skill. (only if tests=yes)
- [ ] Backward compatibility is required. (only if backward-compatible=yes)

## Non-Requirements
- Automated tests are not required for this spec. (if tests=no/blank)
- Backward compatibility is not required for this spec. (if backward-compatible=no/blank)
```

If user provided initial guidance, use it as goal context before asking follow-up questions.
