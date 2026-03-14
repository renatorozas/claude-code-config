# claude-code-bootstrap

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
    │   └── worktree/                  # Create git worktree for parallel sessions
    ├── agents/
    │   ├── code-simplifier.md         # Simplify code without changing behavior
    │   ├── code-architect.md          # Architecture analysis and design review
    │   ├── staff-reviewer.md          # Skeptical plan review before implementation
    │   ├── verify-app.md              # Thorough post-change verification
    │   ├── build-validator.md         # Build, lint, test, bundle validation
    │   ├── oncall-guide.md            # Production incident diagnosis
    │   ├── product-manager.md         # Pressure-test features before engineering
    │   └── ui-designer.md             # UI/UX design review across platforms
    ├── rules/
    │   ├── code-style.md              # Code style conventions
    │   ├── guardrails.md              # Things Claude should NOT do
    │   └── git-workflow.md            # Commit and PR conventions
    └── settings.json                  # Pre-allowed permissions and PostToolUse hooks
```

## How to Use

### Option 1: Copy into an existing project

```sh
git clone https://github.com/renatorozas/claude-code-bootstrap.git
cp -r claude-code-bootstrap/.claude /path/to/your-project/
cp claude-code-bootstrap/CLAUDE.md /path/to/your-project/
cp claude-code-bootstrap/.mcp.json /path/to/your-project/
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

See [SETUP.md](SETUP.md) for a step-by-step checklist for adapting the bootstrap to your project.

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

| Skill             | What it does                                                 |
| ----------------- | ------------------------------------------------------------ |
| `/commit-push-pr` | Full git workflow: stage, commit, push, open PR              |
| `/quick-commit`   | Stage all changes and commit with a conventional message     |
| `/test-and-fix`   | Run tests, analyze failures, fix them, re-run                |
| `/review-changes` | Review uncommitted changes and suggest improvements          |
| `/grill`          | Adversarial code review — won't let you ship until it passes |
| `/techdebt`       | Find duplicated and dead code, clean it up                   |
| `/worktree`       | Create a git worktree for a parallel Claude session          |

### Agents

Each agent has YAML frontmatter declaring its description, allowed tools, and model. They auto-load project context via `@` imports.

Ask Claude to use an agent:

```
"Use the code-simplifier agent on the code I just wrote"
"Run verify-app to test everything works"
"Use staff-reviewer to review my plan before I start"
```

| Agent             | Purpose                                                       | Tools |
| ----------------- | ------------------------------------------------------------- | ----- |
| `code-simplifier` | Reduce complexity without changing behavior                   | R+W   |
| `code-architect`  | Design reviews and architectural analysis                     | Read  |
| `staff-reviewer`  | Skeptical plan review — finds problems before implementation  | Read  |
| `verify-app`      | Full verification: static analysis, tests, edge cases         | R+W   |
| `build-validator` | Clean build, typecheck, lint, test, bundle analysis           | R+W   |
| `oncall-guide`    | Incident response: assess severity, diagnose, mitigate        | R+W   |
| `product-manager` | Pressure-test feature proposals before they reach engineering | Read  |
| `ui-designer`     | UI/UX design review across web, mobile web, and native        | Read  |

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
