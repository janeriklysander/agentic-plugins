# Jan-Erik Lysander's Claude Code Plugins

My personal collection of [Claude Code](https://claude.com/claude-code) plugins for documentation, domain-driven design, hook authoring, and code review.

## Installation

From a Claude Code session, add the marketplace first:

```text
/plugin marketplace add janeriklysander/jel-claude-plugins
```

Then install any plugin below. If you prefer to browse interactively, run `/plugin`.

## Plugins

### [docs-system](plugins/docs-system/README.md)

Documentation skill with the Diátaxis framework, 8 templates, and a tech-writer-reviewer agent.

```text
/plugin install docs-system@jel-claude-plugins
```

### [ddd-design](plugins/ddd-design/README.md)

Structured Domain-Driven Design discovery — interview-driven, one question at a time, produces a domain model document.

```text
/plugin install ddd-design@jel-claude-plugins
```

### [create-hook](plugins/create-hook/README.md)

Step-by-step guide for writing Claude Code hooks as shell scripts and wiring them into settings.json.

```text
/plugin install create-hook@jel-claude-plugins
```

### [code-review](plugins/code-review/README.md)

Multi-agent code review — dispatches specialist agents (correctness, security, architecture, performance, maintainability, observability), normalizes findings by severity, and creates follow-up issues for descoped items.

```text
/plugin install code-review@jel-claude-plugins
```

## Reference guides

Standalone reference documentation for Claude Code users, hosted at [janeriklysander.github.io/jel-claude-plugins](https://janeriklysander.github.io/jel-claude-plugins/):

- [1Password SSH agent setup](https://janeriklysander.github.io/jel-claude-plugins/1password-ssh-agent-setup) — configure 1Password's SSH agent to work inside Claude Code's sandbox (macOS and WSL2)
- [Claude Code security hardening](https://janeriklysander.github.io/jel-claude-plugins/claude-code-security-hardening) — three-layer permission and sandbox configuration to reduce prompt fatigue and protect against prompt injection

## Optional dependencies

The **docs-system** plugin's lint script uses [Vale](https://vale.sh/) and [markdownlint](https://github.com/DavidAnson/markdownlint-cli2). Both are optional — if you don't have them installed, the skill and reviewer agent skip linting and note it in their output. See the [docs-system README](plugins/docs-system/README.md) for install instructions.

## Development setup

After cloning, configure git to use the tracked hooks:

```bash
git config core.hooksPath .githooks
```

A pre-commit hook enforces that any change to a plugin includes a version bump in that plugin's `plugin.json`.

## Contributing

Feedback and bug reports are welcome via [GitHub Issues](https://github.com/janeriklysander/jel-claude-plugins/issues). There is no roadmap — this is a personal collection shared with the community.

## License

[MIT](LICENSE)
