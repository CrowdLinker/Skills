# Contributing

## Adding a skill

1. Create a directory at the repo root named after your skill (kebab-case):
   ```
   my-skill-name/
   └── SKILL.md
   ```

2. Write `SKILL.md` with required frontmatter:
   ```yaml
   ---
   name: my-skill-name
   description: One sentence — what this skill does and when to trigger it (max 200 chars)
   ---

   # Skill instructions in markdown...
   ```

3. Keep instructions model-agnostic — don't reference specific LLM capabilities.

4. Add the skill to the table in `README.md`.

5. Open a PR.

## Frontmatter fields

| Field | Required | Notes |
|-------|----------|-------|
| `name` | Yes | Max 64 chars, matches directory name |
| `description` | Yes | Max 200 chars, used by agents to decide when to invoke |
| `model` | No | Pin to a specific model (e.g., `claude-haiku-4-5-20251001`) |
| `effort` | No | `low`, `medium`, or `high` |
| `user-invocable` | No | `true` to allow manual invocation |
| `disable-model-invocation` | No | `true` to prevent agent auto-invocation |

## Skill directory structure

```
my-skill/
├── SKILL.md          # Required
├── references/       # Optional: supporting docs the skill can reference
├── scripts/          # Optional: executable helpers
└── assets/           # Optional: templates, static files
```
