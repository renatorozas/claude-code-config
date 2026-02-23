# CLAUDE.md

This file is Claude's project memory. Update it whenever Claude makes a mistake so it doesn't repeat it.

## Project Overview

<!-- Customize: tech stack, purpose, key dependencies -->

(Describe your project here)

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

## Code Style & Conventions

<!-- Customize for your project -->

- Prefer `type` over `interface`; never use `enum` (use string literal unions)
- Use descriptive variable names
- Keep functions small and focused
- Write tests for new functionality
- Handle errors explicitly — don't swallow them

## Commands Reference

```sh
# Build & verify (customize for your project)
npm run typecheck        # Type checking
npm run test            # Run tests
npm run lint            # Lint all files
npm run format          # Format code

# Git
git status              # Check current state
git diff                # Review changes before commit
```

## Principles

- **Simplest correct solution.** Don't over-engineer. Don't gold-plate.
- **Find root causes.** No temporary fixes. No band-aids.
- **Minimal blast radius.** Only touch what's necessary. Don't refactor unrelated code.
- **Own your mistakes.** When corrected, write a rule to prevent repeating it.

## Things Claude Should NOT Do

<!-- This is the most valuable section over time — add to it aggressively -->

- Don't use `any` type without explicit approval
- Don't skip error handling
- Don't commit without running tests first
- Don't make breaking API changes without discussion
- Don't add abstractions or refactor code you weren't asked to touch
- Don't assume intent on ambiguous bugs — ask first

## Project-Specific Patterns

<!-- Add patterns as they emerge from your codebase -->

---

_Every mistake is a rule waiting to be written._
