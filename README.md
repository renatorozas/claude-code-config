# claude-code-config

A ready-to-use project setup for [Claude Code](https://code.claude.com). Drop it into any project to get a structured CLAUDE.md, living documentation, skills, agents, rules, and hooks — all working together out of the box.

Inspired by [Boris Cherny's best practices](https://x.com/bcherny/status/2007179832300581177) for workflow orchestration, verification loops, and self-improvement patterns.

## What's Included

```
├── CLAUDE.md                          # Project memory — how Claude should work
├── .mcp.json                          # MCP server configuration
├── docs/
│   ├── spec.md                        # Product specification by feature
│   ├── tech.md                        # Technical architecture by domain
│   └── lessons.md                     # Rules from past mistakes
└── .claude/
    ├── skills/
    │   ├── commit-push-pr/            # Commit, push, and open a PR
    │   ├── quick-commit/              # Fast commit with conventional message
    │   ├── test-and-fix/              # Run tests and fix failures
    │   ├── review-changes/            # Review uncommitted changes
    │   ├── grill/                     # Adversarial code review
    │   ├── techdebt/                  # Find and kill dead/duplicated code
    │   ├── worktree/                  # Create git worktree for parallel sessions
    │   └── sync-claude-config/       # Sync agents and skills from upstream
    ├── agents/
    │   ├── code-simplifier.md         # Simplify code without changing behavior
    │   ├── code-architect.md          # Architecture analysis and design review
    │   ├── staff-engineer.md          # Skeptical plan review before implementation
    │   ├── strategy-advisor.md        # Business plan and market strategy review
    │   ├── verify-app.md              # Thorough post-change verification
    │   ├── build-validator.md         # Build, lint, test, bundle validation
    │   ├── oncall-guide.md            # Production incident diagnosis
    │   ├── product-manager.md         # Pressure-test features before engineering
    │   ├── business-lawyer.md        # Entity, IP, contracts, regulatory exposure
    │   ├── tax-lawyer.md             # Tax nexus, sales tax, payment compliance
    │   └── ui-designer.md            # UI/UX design review across platforms
    ├── rules/
    │   ├── code-style.md              # Code style conventions
    │   ├── guardrails.md              # Things Claude should NOT do
    │   └── git-workflow.md            # Commit and PR conventions
    └── settings.json                  # Pre-allowed permissions and PostToolUse hooks
```

## How to Use

### Option 1: Add to an existing project

```sh
# Add claude-code-config as a git remote
cd /path/to/your-project
git remote add claude-config https://github.com/renatorozas/claude-code-config.git
git fetch claude-config

# Pull all config files (first-time setup)
git checkout claude-config/main -- .claude/ CLAUDE.md docs/ SETUP.md .mcp.json .prettierignore
git commit -m "chore: add claude-code-config"
```

Then follow [SETUP.md](SETUP.md) to customize for your project.

### Option 2: Start a new project

```sh
git clone https://github.com/renatorozas/claude-code-config.git my-project
cd my-project
rm -rf .git README.md LICENSE
git init
```

Then follow [SETUP.md](SETUP.md) to customize for your project.

## Keeping Up to Date

When upstream adds new agents, updates skills, or improves rules, sync with a single command:

```sh
git fetch claude-config
git checkout claude-config/main -- .claude/agents/ .claude/skills/
git commit -m "chore: sync claude-code-config"
```

This updates **agents and skills** only — your customized files (`CLAUDE.md`, `docs/`, `.claude/rules/`, `.claude/settings.json`) are never touched.

To also sync rules (will overwrite your customizations):

```sh
git checkout claude-config/main -- .claude/agents/ .claude/skills/ .claude/rules/
```

Or use the `/sync-claude-config` skill inside Claude Code to do this automatically.

## How It Works

### CLAUDE.md

The central file Claude reads at the start of every session. It defines:

- **How to work** — plan mode, verification loops, parallel work, bug fixing
- **Documentation system** — what each `docs/` file is for and when to update it
- **Task management workflow** — a 6-step process for every task
- **Principles** — simplest correct solution, find root causes, minimal blast radius
- **`@` imports** — pulls in `docs/spec.md`, `docs/tech.md`, and `docs/lessons.md` automatically

### Living Documentation (`docs/`)

| File         | Purpose                                                                   | Updates when         |
| ------------ | ------------------------------------------------------------------------- | -------------------- |
| `spec.md`    | What the product does — behaviors, business rules, constraints by feature | Behavior changes     |
| `tech.md`    | How it's built — stack, schema, infrastructure, technical decisions       | Architecture changes |
| `lessons.md` | Mistake patterns and rules to prevent them                                | After any correction |

Claude consults these docs at session start via `@` imports in CLAUDE.md and in each agent.

### Skills

Type `/` in Claude Code to see available skills. Each skill runs in a forked subagent with restricted tool access:

| Skill                 | What it does                                                 |
| --------------------- | ------------------------------------------------------------ |
| `/commit-push-pr`     | Full git workflow: stage, commit, push, open PR              |
| `/quick-commit`       | Stage all changes and commit with a conventional message     |
| `/test-and-fix`       | Run tests, analyze failures, fix them, re-run                |
| `/review-changes`     | Review uncommitted changes and suggest improvements          |
| `/grill`              | Adversarial code review — won't let you ship until it passes |
| `/techdebt`           | Find duplicated and dead code, clean it up                   |
| `/worktree`           | Create a git worktree for a parallel Claude session          |
| `/sync-claude-config` | Sync agents and skills from upstream claude-code-config      |

### Agents

Each agent has YAML frontmatter declaring its description, allowed tools, and model. They auto-load project context via `@` imports.

Ask Claude to use an agent:

```
"Use the code-simplifier agent on the code I just wrote"
"Run verify-app to test everything works"
"Use staff-engineer to review my plan before I start"
```

| Agent              | Purpose                                                                  | Tools |
| ------------------ | ------------------------------------------------------------------------ | ----- |
| `code-simplifier`  | Reduce complexity without changing behavior                              | R+W   |
| `code-architect`   | Design reviews and architectural analysis                                | Read  |
| `staff-engineer`   | Pressure-tests plans and proposals before code is written                | Read  |
| `strategy-advisor` | Business plan and market strategy review                                 | Read  |
| `verify-app`       | Full verification: static analysis, tests, edge cases                    | R+W   |
| `build-validator`  | Clean build, typecheck, lint, test, bundle analysis                      | R+W   |
| `oncall-guide`     | Incident response: assess severity, diagnose, mitigate                   | R+W   |
| `product-manager`  | Pressure-test feature proposals before they reach engineering            | Read  |
| `business-lawyer`  | Entity formation, corporate structure, IP ownership, regulatory exposure | Read  |
| `tax-lawyer`       | Stripe integration, payment flows, sales tax, fintech compliance         | Read  |
| `ui-designer`      | UI/UX design review across web, mobile web, and native                   | Read  |

_Read = Read, Grep, Glob. R+W = Read, Grep, Glob, Bash, Edit, Write._

### Rules

`.claude/rules/` contains always-loaded rules that apply to every session:

- **`code-style.md`** — Code style conventions (types, naming, error handling)
- **`guardrails.md`** — Things Claude should NOT do
- **`git-workflow.md`** — Commit conventions and PR process

### MCP Configuration

`.mcp.json` at the project root configures [Model Context Protocol](https://modelcontextprotocol.io/) servers. Add servers for databases, APIs, or other tools your project needs.

### Settings

`.claude/settings.json` includes:

- **Pre-allowed permissions** for common safe commands (npm, git, gh, node, etc.) so Claude doesn't prompt for approval on routine operations
- **PostToolUse hook** that auto-formats code after every Write/Edit operation

## Philosophy

This setup follows a few key ideas from Boris Cherny's workflow:

- **Plan before you build.** Start in plan mode, iterate until the plan is good, then let Claude 1-shot the implementation.
- **Verify everything.** Never mark a task done without proving it works. Run tests, typecheck, lint.
- **Learn from mistakes.** Every correction becomes a rule in `docs/lessons.md` or `.claude/rules/guardrails.md`.
- **Keep CLAUDE.md concise.** Every line competes for context window.
- **Make it a team habit.** Check CLAUDE.md and `docs/` into git. The whole team contributes.

## Credits

- [Boris Cherny](https://x.com/bcherny) — Creator of Claude Code, whose [workflow thread](https://x.com/bcherny/status/2007179832300581177) and [team tips](https://www.threads.com/@boris_cherny/post/DUMZr4VElyb) inspired this setup
- [0xquinto/bcherny-claude](https://github.com/0xquinto/bcherny-claude) — Community repo that provided the base `.claude/` configuration (commands, agents, settings)

## License

MIT
