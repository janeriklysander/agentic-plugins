# Agentic Plugins

Curated plugins for [Claude Code](https://claude.com/claude-code) — a documentation system, a DDD design facilitator, and a hook creation guide.

## Plugins

| Plugin | Description |
| --- | --- |
| **docs-system** | Documentation skill with Diataxis framework, 8 templates, linting, and a tech-writer-reviewer agent |
| **ddd-design** | Structured Domain-Driven Design discovery sessions with strategic and tactical modeling |
| **create-hook** | Step-by-step guide for creating Claude Code hooks with shell scripts |

## Installation

From inside a Claude Code session, first add the marketplace:

```
/plugin marketplace add janeriklysander/jel-claude-plugins
```

Then install a plugin:

```
/plugin install docs-system@jel-claude-plugins
```

Replace `docs-system` with the plugin you want. Or run `/plugin` to browse and install interactively.

## Reference guides

Standalone reference documentation for Claude Code users, hosted at [janeriklysander.github.io/jel-claude-plugins](https://janeriklysander.github.io/jel-claude-plugins/):

- [1Password SSH agent setup](https://janeriklysander.github.io/jel-claude-plugins/1password-ssh-agent-setup) — configure 1Password's SSH agent to work inside Claude Code's sandbox (macOS and WSL2)
- [Claude Code security hardening](https://janeriklysander.github.io/jel-claude-plugins/claude-code-security-hardening) — three-layer permission and sandbox configuration to reduce prompt fatigue and protect against prompt injection

## Optional dependencies

The **docs-system** plugin includes a lint script that uses [Vale](https://vale.sh/) and [markdownlint](https://github.com/DavidAnson/markdownlint-cli2). These are optional — if not installed, the documentation skill and tech-writer-reviewer agent will skip linting and note it in their output.

## Contributing

Feedback and bug reports are welcome via [GitHub Issues](https://github.com/janeriklysander/jel-claude-plugins/issues). There is no roadmap — this is a personal collection shared with the community.

## License

[MIT](LICENSE)
