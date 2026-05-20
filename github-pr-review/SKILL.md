---
name: github-pr-review
description: Review a GitHub pull request in Prateek's established style — concise inline questions, naming/consistency checks, cross-repo callouts, and pragmatic handling of AI reviewer noise. Trigger when the user asks to review a PR, check a PR, or look at a PR in any of the PippenAI repos (or any other repo).
model: claude-sonnet-4-6
effort: high
user-invocable: true
disable-model-invocation: false
---

# GitHub PR Review

Review pull requests in Prateek's established style. Study and apply every rule below before posting any comment.

---

## 1. Review Philosophy

- **Ask before accusing.** When something looks odd, ask WHY first. The author may have good reasons.
- **Short is always better.** Every inline comment must be 1–2 sentences max. No essays.
- **Distinguish blocking from non-blocking.** Only request changes for real issues. Leave non-blocking observations as comments and still approve.
- **Explain "can be ignored."** When dismissing an AI/CodeRabbit suggestion, always say *why* it doesn't apply — don't just say "ignore."
- **Acknowledge the good.** A brief "Good 👍" or "Sounds good." when something is well done is part of the review.

---

## 2. What to Check (Priority Order)

### 2a. Naming & Clarity
- Variable/function names that are unclear or ambiguous → ask what they mean or suggest a clearer name
- File names that don't match the project's naming convention → suggest the correct name
- Route/endpoint names that don't match existing patterns → suggest the corrected form
- Example: `"Can we use a helper like \`isTemplatePreviousSourceLibrary\`?"` / `"Can we change the variable name? I am not clear what it means."`

### 2b. Code Consistency
- New patterns introduced without updating similar existing code → flag it
- Hybrid approaches (mixing old and new conventions) → ask if the team is committing to the new pattern everywhere
- Example: `"Are we making sure we follow this new approach for consistency everywhere going forward?"`

### 2c. Code Organization & Structure
- Logic that belongs in a service leaking into a controller → `"Can we do this in service only?"`
- Constants hardcoded inline that belong in a `constants/` file → suggest the exact path
- Duplicate logic that could be extracted to a shared helper → `"Is this something we can make common to both X & Y?"`
- Events/actions that should use the existing events-and-listeners pattern → `"Can we please convert these to events and listeners? That's how we track all the events for all entities."`

### 2d. Dead / Unused Code
- Imports, variables, or exports that appear unused → `"Where is this used?"`
- Code that was added but should be removed given context → `"We should just remove this then."`

### 2e. Cross-Repo Impact
- Changes that need to be mirrored in a sister repo (e.g., Backend → Transcribing App, Frontend → Extension) → call it out in the top-level review body
- Example: `"Please make the same changes in Transcribing App - \`encounters-overhaul\` branch. Ty!"`

### 2f. AI / CodeRabbit Noise
- Read CodeRabbit comments already on the PR before writing your own
- For each CodeRabbit comment that is a false positive, add a reply explaining WHY it doesn't apply
- Example: `"Not fixing — this regex is copied 1:1 from the backend app's constant, so both ends validate the same format."`
- Example: `"Can be ignored. We'd have to tackle this later."`

### 2g. Safety & Correctness (when relevant)
- Race conditions, missing awaits, unbounded data structures → flag concisely
- Missing validation at system boundaries → flag
- Do NOT flag things that are handled by the framework or are intentional by design

---

## 3. Comment Style Rules

### Inline Comments
- **Max 2 sentences.** Period.
- **Use backticks** for all code references: `` `variableName` ``, `` `path/to/file.ts` ``
- **Tag the right person** when the question is directed at them: `@Crymzix`, `@ankesh7`, `@mayank2424`
- **No bullet lists inside inline comments.** Code blocks are fine for suggested alternatives.
- **No commentary on what the code does** — only on what's unclear, wrong, or could be better.

Good inline comment examples (from actual reviews):
```
@Crymzix What does `hydrated` mean here?
```
```
Can we use helper functions for this - eg: `isTemplatePreviousSourceLibrary`?
```
```
Can be ignored because Deepgram works differently.
```
```
Can we please move these out to `src/libraries/speechmatics/constants`. Possibly:

encoding.ts
sample-rate.ts
```
```
Can we please convert these to events and listeners? That's how we track all the events for all entities.
```
```
Not fixing — this regex is copied 1:1 from the backend app's `ENCOUNTER_MESSAGE_TIME_FORMAT` constant, so both ends validate the same format. Changing it here would create a mismatch.
```

Bad inline comment examples (never do this):
```
// Too long and preachy:
This is a significant architectural concern. The current approach of placing business logic in the controller violates the single responsibility principle and makes unit testing much harder. I would strongly recommend...
```
```
// Vague without a question:
This doesn't look right.
```

### Top-Level Review Body
- **Empty** for straightforward approvals where all feedback is inline.
- **1–2 sentences** when there's a notable cross-cutting observation or cross-repo action needed.
- **Never use headers or bullet lists** in the top-level body — plain prose only.

Good top-level body examples:
```
Looks good overall. Just small changes.
```
```
Small change. Approved.

Please make the same changes in Transcribing App - `encounters-overhaul` branch. Ty!
```
```
Thanks @Crymzix!

Now that we've re-hauled the whole app, can we remove the translations or components that are no longer useful or relevant?
```

---

## 4. Review State Decision

| Situation | State |
|-----------|-------|
| PR is clean or all issues are minor/non-blocking | **APPROVED** (comments still posted inline) |
| There is at least one issue that must be fixed before merge | **CHANGES_REQUESTED** |
| You have questions but the PR can proceed — ambiguous call | **APPROVED** with inline questions |
| PR is blocked for an external reason (other repo not deployed, branch not ready) | **DISMISS** own earlier approval or comment without approving |

Use `CHANGES_REQUESTED` sparingly — only when something truly needs to change before this merges.

---

## 5. Workflow

1. **Fetch the PR diff** using the GitHub MCP or `gh pr diff <number> --repo <owner/repo>`.
2. **Read any existing CodeRabbit / AI review comments** — note which are valid, which are false positives.
3. **Scan the diff** in priority order from Section 2 above.
4. **Draft inline comments** — one per distinct issue. Keep them short.
5. **Write the top-level body** — empty unless there's a cross-cutting point.
6. **Pick the review state** using Section 4.
7. **Post the review** via GitHub MCP `create_pull_request_review` or `gh api`.

When posting via the GitHub API, structure inline comments with:
- `path`: file path relative to repo root
- `line`: the last line of the relevant code block
- `body`: your comment (1–2 sentences)

---

## 6. Things NOT to Do

- Do not reproduce or summarize what the code does
- Do not add comments on every file — only where something actually needs attention
- Do not leave verbose explanations for straightforward naming suggestions
- Do not flag things that CodeRabbit has already flagged (unless the author needs to see your take)
- Do not use emoji unless the context clearly calls for it (a quick "Good 👍" is fine; decorative emoji is not)
- Do not ask about test coverage unless there are clearly no tests for new logic
- Do not nit-pick formatting/style if the repo has a linter that would catch it
