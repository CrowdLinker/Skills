---
name: github-commit-and-pr-conventions
description: Apply personal GitHub commit and PR conventions whenever making commits, creating PRs, or updating PRs via the GitHub MCP or any GitHub-related tool. Always use this skill when the user asks to commit, push, open a pull request, create a PR, or update a PR — even if they don't explicitly ask for formatting help. Also trigger when staging changes, writing commit messages, or interacting with branches.
model: claude-haiku-4-5-20251001
effort: high
user-invocable: true
disable-model-invocation: false
---
 
# GitHub Commit & PR Conventions
 
Apply these rules every time you make a commit or create/update a PR. No exceptions unless the user explicitly overrides.
 
---
 
## 1. Commit Message Format
 
Always use Conventional Commits prefix. Choose based on the **most significant** change in the diff:
 
| Prefix | When to use |
|--------|-------------|
| `feat:` | New feature, new capability, new endpoint, new component |
| `fix:`  | Bug fix, incorrect behavior corrected, crash resolved |
| `chore:` | Config changes, dependency updates, refactors, tooling, cleanup, CI/CD, docs |
 
**Priority rule**: If a commit contains both a `feat` and a `fix`, use `feat:`. If it contains a `fix` and a `chore`, use `fix:`. In short: `feat` > `fix` > `chore`.
 
### Commit message anatomy
 
```
<prefix>: <short imperative summary under ~72 chars>
 
<optional body: explain WHY, list notable changes, call out edge cases>
```
 
- **Title line**: Short, imperative, no period. E.g. `feat: add OAuth2 refresh token rotation`
- **Body** (via the description/body field, NOT the title): Use when there's meaningful context — breaking changes, migration notes, non-obvious side effects, or multiple logical changes bundled together. Bullet points are fine in the body.
- **Do NOT** pad the title to fit a limit; keep it concise and let the body carry detail.
- **Do NOT** repeat the title verbatim in the body.
### Examples
 
Good:
```
feat: add rate limiting middleware to auth routes
 
- Uses sliding window algorithm (Redis-backed)
- Configurable per-route via decorator options
- Returns 429 with Retry-After header on breach
```
 
Good (no body needed):
```
chore: upgrade NestJS to v11
```
 
Bad:
```
updated stuff
fix some things and also added a new feature and updated deps
```
 
---
 
## 2. Branch Targeting
 
Before committing or opening a PR, check whether a `develop` branch exists in the repository.
 
- **If `develop` exists** → target `develop` as the base branch (for PRs) or push there (for commits), unless the user explicitly specifies another branch.
- **If only `main`/`master` exists** → use that.
- **If user specifies a branch** → always honour the user's explicit choice.
To check: use the GitHub MCP tool to list branches and look for `develop` / `development` / `dev`.
 
---
 
## 3. Pull Request Rules
 
### Title
**Never use `feat:`, `fix:`, or `chore:` prefixes in PR titles.** PR titles should describe the key feature(s) or work done, in sentence case.

**Format rules:**
- Sentence case (only capitalize the first word and proper nouns)
- Be descriptive — name all significant areas touched
- Multiple changes: join with `, `, ` and `, ` + `, or prefix with an area name followed by `: `
- Titles can be longer than a typical commit subject — clarity beats brevity here

**Single focused change:**
```
Stories Kanban board view with drag-and-drop
Granola integration: connect, import notes, and AI processing
```

**Multiple areas bundled:**
```
Story/spec editor improvements, per-project settings, list sorting, and bug fixes
Onboarding: slug editing and mandatory first project step
Workspace role enforcement: integrations, billing, and sidebar
```

**With a story ID (single task):**
```
AST-15 - User forgot password
PIP-123 - Delete user & user related entities
SC-12314 - Admin: make encounter prompts dynamic
```

**With story IDs (multiple tasks):**
```
User forgot password, delete user & related entities, admin dynamic encounter prompts
```
(When multiple tasks, drop the story ID prefix from the title and list IDs in the References section instead.)
 
### Description / Body
Structure the PR body like this:
 
```markdown
## Summary
<1–3 sentence explanation of what changed and why>
 
## Changes
- <bullet: key change 1>
- <bullet: key change 2>
- ...
 
## Notes
<optional: migration steps, breaking changes, caveats, follow-ups>
 
### References to Tasks:
1. [AST-15]()
2. [PIP-123]()
```
 
**Always include the "References to Tasks" section at the end** if one or more story IDs are known. Omit it only when there are no associated stories.
 
**Never include a "Testing Plan" section** unless the user explicitly asks for one.
 
### Assignee
- **Creating a new PR**: Always auto-assign the currently authenticated GitHub user. Use the GitHub MCP `get authenticated user` (or equivalent) call to resolve the login, then set them as assignee.
- **Updating an existing PR**: Do **not** modify the assignee field, even if it's empty or set to someone else.
### Labels / Reviewers
Don't add labels or reviewers unless the user asks.
 
---
 
## 4. Keeping Commits Clean
 
- Prefer **one logical commit per PR** where possible. If staging multiple unrelated changes, ask the user whether to split them.
- Use `--no-verify` only if the user explicitly asks.
- Never squash or amend commits that have already been pushed to a shared branch without asking first.
- If the diff is large but cohesive (e.g., a single feature touching many files), one commit is still correct — use the body to explain the breadth.
---
 
## 5. Quick Decision Checklist
 
Before every commit/PR action, run through:
 
1. ✅ What's the most significant change type? → pick commit prefix
2. ✅ Is the commit title under ~72 chars and imperative?
3. ✅ Does the body (if needed) explain *why*, not just *what*?
4. ✅ Does `develop` branch exist? → target accordingly
5. ✅ Is this a **new** PR? → assign current user
6. ✅ PR title: no `feat:`/`fix:`/`chore:` prefix — use feature name(s), prepend story ID if single task
7. ✅ Are there story IDs? → add `### References to Tasks:` at end of PR body
8. ✅ Is this a PR body? → no Testing Plan section
---
 
## Tool Usage Notes (GitHub MCP)
 
When using the GitHub MCP or similar tools:
 
- To get the current user: call the "get authenticated user" or `whoami`-style endpoint before creating a PR, then pass that login as the assignee.
- To check for `develop`: list branches and search for names matching `develop`, `development`, or `dev` (case-insensitive).
- Set the PR body using the full markdown template above.
- When the commit API separates `message` (title) from `description` (body), always split them — don't jam everything into the title.
