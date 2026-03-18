---
description: Sync agents and skills from the upstream claude-code-config repo.
context: fork
agent: general-purpose
allowed-tools: Bash, Read
---

Sync `.claude/agents/` and `.claude/skills/` from the upstream claude-code-config repo using git.

If the user passes `--init`, also sync: `.claude/rules/`, `CLAUDE.md`, `docs/`, `SETUP.md`, `.mcp.json`, `.prettierignore`, `.claude/settings.json`.

Steps:

1. Check if the `claude-config` remote exists:

   ```
   git remote get-url claude-config 2>/dev/null
   ```

   If it doesn't exist, add it:

   ```
   git remote add claude-config https://github.com/renatorozas/claude-code-config.git
   ```

2. Fetch the latest from upstream:

   ```
   git fetch claude-config
   ```

3. Pull the files based on mode:

   **Default (sync):**

   ```
   git checkout claude-config/main -- .claude/agents/ .claude/skills/
   ```

   **With `--init`:**

   ```
   git checkout claude-config/main -- .claude/agents/ .claude/skills/ .claude/rules/ CLAUDE.md docs/ SETUP.md .mcp.json .prettierignore .claude/settings.json
   ```

4. Show what changed:

   ```
   git diff --cached --stat
   ```

5. If there are changes, commit them:

   ```
   git commit -m "chore: sync claude-code-config agents and skills"
   ```

   For `--init`, use: `"chore: add claude-code-config"`

6. If there are no changes, tell the user everything is already up to date.
