# claude-code-bootstrap

A ready-to-use project setup for [Claude Code](https://code.claude.com). Drop it into any project to get a structured CLAUDE.md, living documentation, slash commands, subagents, and hooks — all working together out of the box.

Inspired by [Boris Cherny's best practices](https://x.com/bcherny/status/2007179832300581177) for workflow orchestration, verification loops, and self-improvement patterns.

## What's Included

```
├── CLAUDE.md                          # Project memory — how Claude should work
├── docs/
│   ├── spec.md                        # Product specification by feature
│   ├── tech.md                        # Technical architecture by domain
│   └── lessons.md                     # Rules from past mistakes
└── .claude/
    ├── commands/
    │   ├── commit-push-pr.md          # Commit, push, and open a PR
    │   ├── quick-commit.md            # Fast commit with conventional message
    │   ├── test-and-fix.md            # Run tests and fix failures
    │   ├── review-changes.md          # Review uncommitted changes
    │   ├── grill.md                   # Adversarial code review
    │   ├── techdebt.md                # Find and kill dead/duplicated code
    │   └── worktree.md                # Create git worktree for parallel sessions
    ├── agents/
    │   ├── code-simplifier.md         # Simplify code without changing behavior
    │   ├── code-architect.md          # Architecture analysis and design review
    │   ├── staff-reviewer.md          # Skeptical plan review before implementation
    │   ├── verify-app.md              # Thorough post-change verification
    │   ├── build-validator.md         # Build, lint, test, bundle validation
    │   └── oncall-guide.md            # Production incident diagnosis
    └── settings.json                  # Pre-allowed permissions and PostToolUse hooks
```

## How to Use

### Option 1: Copy into an existing project

```sh
git clone https://github.com/renatorozas/claude-code-bootstrap.git
cp -r claude-code-bootstrap/.claude /path/to/your-project/
cp claude-code-bootstrap/CLAUDE.md /path/to/your-project/
cp -r claude-code-bootstrap/docs /path/to/your-project/
rm -rf claude-code-bootstrap
```

### Option 2: Use as a starting point for a new project

```sh
git clone https://github.com/renatorozas/claude-code-bootstrap.git my-project
cd my-project
rm -rf .git README.md LICENSE
git init
```

### After setup

1. Edit `CLAUDE.md` — fill in the Project Overview, customize Code Style & Conventions and Commands Reference for your stack
2. Edit `docs/tech.md` — fill in the Stack table and Project Structure
3. Edit `.claude/settings.json` — adjust pre-allowed commands if you use something other than npm (e.g., `bun`, `pnpm`, `yarn`)
4. Start Claude Code in your project directory — it reads the config automatically

## How It Works

### CLAUDE.md

The central file Claude reads at the start of every session. It defines:

- **How to work** — plan mode, verification loops, parallel work, bug fixing
- **Documentation system** — what each `docs/` file is for and when to update it
- **Task management workflow** — a 6-step process for every task
- **Principles** — simplest correct solution, find root causes, minimal blast radius
- **Things Claude should NOT do** — a living list of mistakes to avoid (the most valuable section over time)

### Living Documentation (`docs/`)

| File | Purpose | Updates when |
|------|---------|-------------|
| `spec.md` | What the product does — behaviors, business rules, constraints by feature | Behavior changes |
| `tech.md` | How it's built — stack, schema, infrastructure, technical decisions | Architecture changes |
| `lessons.md` | Mistake patterns and rules to prevent them | After any correction |

Claude consults `docs/spec.md`, `docs/tech.md`, and `docs/lessons.md` at session start for context, and updates them as part of the task management workflow.

### Slash Commands

Type `/` in Claude Code to see available commands:

| Command | What it does |
|---------|-------------|
| `/commit-push-pr` | Full git workflow: stage, commit, push, open PR |
| `/quick-commit` | Stage all changes and commit with a conventional message |
| `/test-and-fix` | Run tests, analyze failures, fix them, re-run |
| `/review-changes` | Review uncommitted changes and suggest improvements |
| `/grill` | Adversarial code review — won't let you ship until it passes |
| `/techdebt` | Find duplicated and dead code, clean it up |
| `/worktree` | Create a git worktree for a parallel Claude session |

### Subagents

Ask Claude to use a subagent:

```
"Use the code-simplifier agent on the code I just wrote"
"Run verify-app to test everything works"
"Use staff-reviewer to review my plan before I start"
```

| Agent | Purpose |
|-------|---------|
| `code-simplifier` | Reduce complexity without changing behavior |
| `code-architect` | Design reviews and architectural analysis |
| `staff-reviewer` | Skeptical plan review — finds problems before implementation |
| `verify-app` | Full verification: static analysis, tests, edge cases |
| `build-validator` | Clean build, typecheck, lint, test, bundle analysis |
| `oncall-guide` | Incident response: assess severity, diagnose, mitigate |

### Settings

`.claude/settings.json` includes:

- **Pre-allowed permissions** for common safe commands (npm, git, gh, node, ls, grep, etc.) so Claude doesn't prompt for approval on routine operations
- **PostToolUse hook** that auto-formats code after every Write/Edit operation

## Philosophy

This setup follows a few key ideas from Boris Cherny's workflow:

- **Plan before you build.** Start in plan mode, iterate until the plan is good, then let Claude 1-shot the implementation.
- **Verify everything.** Never mark a task done without proving it works. Run tests, typecheck, lint.
- **Learn from mistakes.** Every correction becomes a rule in `docs/lessons.md` or the "Things Claude Should NOT Do" section of CLAUDE.md.
- **Keep CLAUDE.md concise.** Every line competes for context window. The file is ~120 lines and ~2k tokens — under Boris's recommended ceiling of 2.5k tokens.
- **Make it a team habit.** Check CLAUDE.md and `docs/` into git. The whole team contributes.

## Credits

- [Boris Cherny](https://x.com/bcherny) — Creator of Claude Code, whose [workflow thread](https://x.com/bcherny/status/2007179832300581177) and [team tips](https://www.threads.com/@boris_cherny/post/DUMZr4VElyb) inspired this setup
- [0xquinto/bcherny-claude](https://github.com/0xquinto/bcherny-claude) — Community repo that provided the base `.claude/` configuration (commands, agents, settings)

## License

MIT
