# docs-system

A documentation skill for Claude Code. It guides you through writing any type of technical document — READMEs, quickstarts, how-to guides, API references, ADRs, RFCs, migration guides, and runbooks — using the [Diátaxis framework](https://diataxis.fr/).

## Prerequisites

Install the plugin from a Claude Code session:

```text
/plugin install docs-system@jel-claude-plugins
```

## Skills and agents

**`/documentation`** — writes or improves a document. Tell it what you want to document and it'll ask the right questions, propose a structure, and write it.

**`tech-writer-reviewer` agent** — reviews existing documentation for clarity, accuracy, and consistency. Returns a verdict: ready to publish, needs minor edits, or needs significant rework.

## Usage

Run the skill from a Claude Code session:

```text
/documentation
```

Or with a hint:

```text
/documentation ADR for switching from REST to GraphQL
```

To invoke the reviewer agent, ask Claude directly — it doesn't use a slash command:

```text
Review docs/api-reference.md
```

## Templates

Eight templates are included:

| Template | Diátaxis type |
| --- | --- |
| README | Orientation + quickstart |
| Quickstart | Tutorial |
| How-to guide | How-to |
| API reference | Reference |
| ADR | — |
| RFC / Design doc | — |
| Migration guide | — |
| Runbook | — |

ADRs, RFCs, migration guides, and runbooks fall outside the Diátaxis taxonomy but follow their own established conventions.

## Optional dependencies

If these aren't installed, the skill and reviewer skip linting and note it in their output.

- **[markdownlint](https://github.com/DavidAnson/markdownlint-cli2)** — structural markdown quality (runs via `npx`, auto-downloaded on first use; requires Node.js)
- **[Vale](https://vale.sh/)** — prose quality: passive voice, weasel words, tone

Install Vale:

```bash
brew install vale        # macOS
snap install vale --edge # Linux
```

Or download directly from [vale.sh](https://vale.sh/docs/vale-cli/installation/).
