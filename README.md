# skills

A collection of reusable AI agent skills. Compatible with Claude Code, Codex CLI, Gemini CLI, Cursor, GitHub Copilot, and any agent that supports the [Agent Skills](https://agentskills.io) standard.

## Install

**Single skill (global):**
```sh
npx skills add crowdlinker/skills --skill github-commit-and-pr-conventions --agent claude-code -g -y
```

**All skills (global):**
```sh
npx skills add crowdlinker/skills --agent claude-code -g -y
```

**Project-level (current repo only):**
```sh
npx skills add crowdlinker/skills --agent claude-code -y
```

> **Note:** The `--agent claude-code` flag is required. Without it, the interactive prompt may install to a different agent (e.g. Warp) and the skill won't appear in Claude Code. Global skills install to `~/.claude/skills/`; project skills install to `.claude/skills/`.

After installing, run `/reload-plugins` in Claude Code to pick up the new skill without restarting.

## Skills

| Skill | Description |
|-------|-------------|
| [github-commit-and-pr-conventions](./github-commit-and-pr-conventions/SKILL.md) | Conventional Commits format, branch targeting, and PR structure rules for GitHub |
| [github-pr-review](./github-pr-review/SKILL.md) | Opinionated, collaborative PR review style for GitHub pull requests |

## Manual installation (other AI agents)

For agents that don't support `npx skills add`, copy the `SKILL.md` content into your agent's system prompt or custom instructions.

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

[MIT](./LICENSE)
