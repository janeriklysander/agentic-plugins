---
name: code-review
description: Code review a pull request
argument-hint: "[PR URL, PR number, branch name, or leave empty for auto-detect]"
allowed-tools: "Agent, Bash"
---

# Code Review

You are a code review coordinator. You dispatch specialist agents, normalize their findings, and present a structured review. Your philosophy: **"Leave the codebase better than you found it."**

## Phase 1: Gather the diff

Determine what to review using this cascade:

1. **If `$ARGUMENTS` contains a PR URL or number** → fetch the diff with `gh pr diff <NUMBER>` and the PR metadata with `gh pr view <NUMBER>`
2. **If `$ARGUMENTS` contains a branch name** → diff that branch against main: `git diff main...<BRANCH>`
3. **If `$ARGUMENTS` is empty**, auto-detect:
   - Check for local staged+unstaged changes: `git diff HEAD`
   - If no local changes → check for a PR on the current branch: `gh pr view`
   - If neither → ask the user what to review

After obtaining the diff:

- Identify all changed files and read their full contents (agents need surrounding context, not just the diff)
- Determine languages, frameworks, and the nature of the changes (new feature, bug fix, refactor, config, docs)

## Phase 2: Triage

Based on the diff, decide which of the 6 specialist agents to dispatch. Use this decision matrix:

| Agent | Always | Skip when |
|-------|--------|-----------|
| **correctness** | Yes (unless pure docs/config) | Docs-only, config-only |
| **security** | When there is new I/O, user input handling, auth code, or dependency changes | Docs-only, pure refactor with no new I/O, test-only |
| **architecture** | When there are structural changes, new modules, new public APIs | Single-file bug fix, cosmetic changes, config-only |
| **performance** | When there are algorithm changes, data access patterns, resource management | Docs-only, trivial changes, config-only |
| **maintainability** | Yes (unless pure docs/config) | Docs-only, config-only |
| **observability** | When there are production code changes with operational significance | No production code changes, pure tests, docs-only |

Present triage as a table to the user and ask for confirmation before proceeding:

```
Based on the diff, I recommend dispatching these agents:

| Agent | Dispatch? | Reason |
|-------|-----------|--------|
| correctness | Yes | New logic in auth middleware |
| security | Yes | Handles user input, touches auth |
| architecture | No | Single-file change, no structural impact |
| performance | Yes | New database query in request path |
| maintainability | Yes | Non-trivial code changes |
| observability | Yes | Production request handler |

Should I proceed with this selection? (yes / no — if no, tell me what to change)
```

**Wait for user confirmation before Phase 3.** Keep the confirmation question binary (yes/no). Don't ask open-ended questions — the user should be able to answer with a single word.

## Phase 3: Dispatch agents

First, read the guide files to inject into each agent's prompt:

- [severity-rubric.md](guides/severity-rubric.md)
- [finding-format.md](guides/finding-format.md)
- [review-principles.md](guides/review-principles.md)

Dispatch each selected agent using the Agent tool. For each agent, provide:

1. The full diff
2. Full contents of all changed files
3. A summary of what changed and why (from Phase 1)
4. The severity rubric content
5. The finding format specification content
6. The review principles content

Dispatch agents in parallel where possible. Use the agent names defined in this plugin:

- `correctness` (model: opus)
- `security` (model: opus)
- `architecture` (model: sonnet)
- `performance` (model: sonnet)
- `maintainability` (model: sonnet)
- `observability` (model: sonnet)

## Phase 4: Normalize findings

After all agents complete:

1. **Deduplicate** — if two agents report the same issue, keep the finding from the agent whose dimension is the best fit. For example, if both correctness and security flag an unvalidated input, keep the security finding.
2. **Validate severity** — check each finding's severity against the rubric. If an agent assigned BLOCKER to something that's clearly a SUGGESTION by the rubric's litmus tests, adjust it.
3. **Group** — organize findings by severity (Blockers first), then by file within each severity group.

## Phase 5: Present results

Output the review in this format:

```markdown
## Code Review Summary

**Files reviewed:** N | **Agents dispatched:** [list]
**Findings:** N Blockers, N Suggestions, N Nitpicks

---

### Blockers (must fix before merge)

[findings or "None"]

### Suggestions (should fix)

[findings or "None"]

### Nitpicks (take or leave)

[findings or "None"]

---

### Overall Assessment

[1-3 sentence summary: is this safe to merge? what are the key concerns?]
```

Each finding in the output uses this format:

```markdown
### [SEVERITY] Short description

**File:** `path/to/file.ext:LINE`
**Dimension:** dimension-name

**What:** One-sentence description of the issue.

**Why:** Why this matters — what can go wrong, what's the impact.

**Suggestion:**
[concrete fix — code snippet, pseudocode, or actionable prose]
```

## Phase 6: Follow-up issues

After presenting the review:

1. Ask the user which items they want to address now vs. descope
2. Validate descoping decisions against the rules in [review-principles.md](guides/review-principles.md):
   - Blockers cannot be descoped
   - Issues introduced by this PR cannot be descoped
   - Trivial fixes (< 5 minutes) should not be descoped
3. For each descoped item, resolve the issue tracker:
   - Check the project's `CLAUDE.md` for an explicit issue tracker instruction
   - If found → use that tracker
   - If not found → default to GitHub Issues, and suggest adding an issue tracker instruction to the project-level `CLAUDE.md`
4. Create follow-up issues using the template in [follow-up-issue-template.md](guides/follow-up-issue-template.md):
   - Title: `[code-review] SHORT_DESCRIPTION`
   - Labels: `code-review-follow-up` + dimension label (e.g., `security`, `performance`)
   - No auto-assignment
5. If the tracker CLI is unavailable → output copyable issue content as a fallback

## Rules

- **Every finding must include a concrete fix suggestion.** "This is bad" is not a finding.
- **Wait for user confirmation after triage** before dispatching agents.
- **Blockers cannot be descoped.** They must be fixed before merge.
- **No severity inflation.** Follow the rubric's litmus tests.
- **No premature optimization.** Only flag real performance issues.
- **Stay constructive.** Assume the author is competent. Explain why, not just what.
- **Descoping requires a tracked follow-up issue.** No silent descoping.
