# skills

A collection of reusable AI agent skills. Compatible with Claude Code, Codex CLI, Gemini CLI, Cursor, GitHub Copilot, and any agent that supports the [Agent Skills](https://agentskills.io) standard.

## Install

**Single skill:**
```sh
npx skills add crowdlinker/skills github-commit-and-pr-conventions
```

**All skills:**
```sh
npx skills add crowdlinker/skills
```

Skills install to `.claude/skills/` (project) or `~/.claude/skills/` (global).

## Skills

| Skill | Description |
|-------|-------------|
| [github-commit-and-pr-conventions](./github-commit-and-pr-conventions/SKILL.md) | Conventional Commits format, branch targeting, and PR structure rules for GitHub |

## Manual installation (other AI agents)

For agents that don't support `npx skills add`, copy the `SKILL.md` content into your agent's system prompt or custom instructions.

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

[MIT](./LICENSE)
