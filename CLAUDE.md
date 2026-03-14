# CLAUDE.md

This file is Claude's project memory. Update it whenever Claude makes a mistake so it doesn't repeat it.

## Project Overview

(Describe your project here)

@docs/spec.md
@docs/tech.md
@docs/lessons.md

## How to Work

### Planning

- Enter plan mode for ANY non-trivial task (3+ steps or architectural decisions)
- Write detailed specs upfront to reduce ambiguity — a good plan means a 1-shot implementation
- If something goes sideways, STOP and re-plan. Don't keep pushing.
- Use plan mode for verification steps, not just building

### Verification

Every change follows this loop before it's considered done:

1. Make changes
2. Run typecheck: `npm run typecheck`
3. Run tests: `npm test`
4. Lint: `npm run lint`
5. Before PR: run full suite, diff behavior against main when relevant

Never mark a task complete without proving it works. Ask yourself: "Would a staff engineer approve this?"

### Parallel Work

- Use subagents to keep the main context window clean and focused
- One task per subagent — offload research, exploration, and analysis
- Only one agent should edit a given file at a time
- For fully parallel workstreams: `git worktree add .claude/worktrees/<n> origin/main`

### Bug Fixing

- Clear reproduction? Investigate and fix directly — check logs, errors, failing tests
- Fix failing CI without being told how
- Ambiguous bug? Clarify scope before proceeding

## Project Documentation (`docs/`)

These files are the project's living knowledge base. Consult them at session start alongside this CLAUDE.md.

- **`docs/spec.md`** — Product specification: feature behaviors, business rules, constraints. Source of truth for what the product does.
- **`docs/tech.md`** — Technical architecture: stack, schema, infrastructure, key technical decisions.
- **`docs/lessons.md`** — Rules derived from past mistakes. Review at session start.

### When to update docs

- **`docs/spec.md`**: When behavior changes — new features, modified flows, changed business rules. Not for refactors or internal-only changes.
- **`docs/tech.md`**: When architecture changes — new dependencies, schema migrations, infrastructure decisions.
- **`docs/lessons.md`**: Immediately after any correction.

Keep all docs concise and well-structured. These files consume context window — verbosity costs you.

## Task Management Workflow

For every task:

1. **Plan First**: Write a plan with checkable items before starting.
2. **Verify Plan**: Check in with the user before starting implementation.
3. **Track Progress**: Mark items complete as you go.
4. **Explain Changes**: Provide a high-level summary at each step.
5. **Update Docs**: Update `docs/spec.md` and `docs/tech.md` if behavior or architecture changed.
6. **Capture Lessons**: If corrected, update `docs/lessons.md` immediately.

## Principles

- **Simplest correct solution.** Don't over-engineer. Don't gold-plate.
- **Find root causes.** No temporary fixes. No band-aids.
- **Minimal blast radius.** Only touch what's necessary. Don't refactor unrelated code.
- **Own your mistakes.** When corrected, write a rule to prevent repeating it.

---

_Every mistake is a rule waiting to be written._
