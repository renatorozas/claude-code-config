# Setup Checklist

Step-by-step guide for adapting claude-code-bootstrap to your project.

## 1. Fill in CLAUDE.md

- [ ] Replace "(Describe your project here)" with your project overview
- [ ] Adjust verification commands if you don't use npm (e.g., `bun`, `pnpm`, `yarn`)

## 2. Fill in docs/

- [ ] `docs/spec.md` — Document your product's features, behaviors, and business rules
- [ ] `docs/tech.md` — Document your stack, schema, infrastructure, and key technical decisions
- [ ] `docs/lessons.md` — Start empty, it grows as you work

## 3. Customize rules/

- [ ] `.claude/rules/code-style.md` — Adjust for your language, framework, and conventions
- [ ] `.claude/rules/guardrails.md` — Add project-specific restrictions
- [ ] `.claude/rules/git-workflow.md` — Adjust commit conventions and PR process for your team

## 4. Update settings.json

- [ ] `.claude/settings.json` — Adjust pre-allowed commands for your toolchain
  - Replace `npm` with `bun`, `pnpm`, or `yarn` if applicable
  - Add any project-specific CLI tools you want auto-allowed
- [ ] Adjust the PostToolUse formatter hook if you use something other than Prettier

## 5. Configure MCP servers

- [ ] `.mcp.json` — Add any MCP servers your project needs (databases, APIs, etc.)
  - See [MCP documentation](https://modelcontextprotocol.io/) for available servers

## 6. Review agents and skills

- [ ] Remove agents you don't need from `.claude/agents/`
- [ ] Remove skills you don't need from `.claude/skills/`
- [ ] Add project-specific agents or skills if needed

## 7. Start using it

```sh
cd your-project
claude
```

Claude will automatically load CLAUDE.md, rules, and project docs at session start.
