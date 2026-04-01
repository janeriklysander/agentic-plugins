# code-review

A multi-agent code review plugin for Claude Code. It dispatches specialist agents to review your changes across 6 dimensions, normalizes findings by severity, and creates follow-up issues for anything descoped.

## Prerequisites

Install the plugin from a Claude Code session:

```text
/plugin install code-review@jel-claude-plugins
```

Optionally, install the [GitHub CLI](https://cli.github.com/) (`gh`) for PR diffing and follow-up issue creation. The plugin works without it but falls back to manual workflows.

## Skills and agents

**`/code-review`** — the coordinator skill. It gathers the diff, triages which specialists to dispatch, runs them, normalizes findings, and presents a structured report.

**6 specialist agents:**

| Agent | Model | Focus |
|-------|-------|-------|
| `correctness` | Opus | Logic bugs, edge cases, off-by-one, null handling, race conditions |
| `security` | Opus | OWASP Top 10, injection, auth bypass, secrets, input validation |
| `architecture` | Sonnet | Coupling, SOLID, API design, separation of concerns, pattern consistency |
| `performance` | Sonnet | Algorithmic complexity, N+1 queries, caching, resource leaks |
| `maintainability` | Sonnet | Clean code, naming, readability, dead code, duplication |
| `observability` | Sonnet | Logging, metrics, tracing, error reporting, alertability |

## Usage

Review a PR by number:

```text
/code-review 42
```

Review a PR by URL:

```text
/code-review https://github.com/owner/repo/pull/42
```

Review a branch against main:

```text
/code-review feature/my-branch
```

Review local uncommitted changes (auto-detected):

```text
/code-review
```

## Review flow

1. **Gather** — obtains the diff and reads full file contents for context
2. **Triage** — assesses which agents are needed and presents a table for confirmation
3. **Dispatch** — runs selected agents in parallel with the diff, file contents, and review guides
4. **Normalize** — deduplicates findings, validates severity, groups by severity then file
5. **Present** — outputs a structured report with Blockers, Suggestions, and Nitpicks
6. **Follow-up** — creates tracked issues for any descoped findings

## Severity levels

- **Blocker** — must fix before merge. "Will we need a hotfix?"
- **Suggestion** — should fix. "Will someone fix this within 3 months?"
- **Nitpick** — take or leave. "Would a senior engineer disagree?"

See [severity-rubric.md](skills/code-review/guides/severity-rubric.md) for the full rubric.

## Follow-up issues

Descoped findings get tracked as issues (GitHub Issues by default). The plugin checks the project's `CLAUDE.md` for a preferred issue tracker. If `gh` isn't available, it outputs copyable issue content instead.
