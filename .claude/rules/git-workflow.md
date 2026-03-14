# Git Workflow

## Commit Conventions

- Use conventional commit prefixes: `feat:`, `fix:`, `refactor:`, `docs:`, `test:`, `chore:`
- Use imperative mood ("Add feature" not "Added feature")
- Keep commit messages brief and descriptive

## PR Process

1. Run `git status` and `git diff` to review changes
2. Stage appropriate files with `git add`
3. Commit with a clear, descriptive message
4. Push to remote branch (create with `-u origin <branch>` if needed)
5. Create PR with `gh pr create` — include a summary and any testing notes

## Before Committing

1. Run typecheck: `npm run typecheck`
2. Run tests: `npm test`
3. Run lint: `npm run lint`
4. Review `git diff` to verify changes are intentional
