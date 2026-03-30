# create-hook

A skill that guides you through adding a Claude Code hook — a shell script that runs automatically before or after tool calls.

## Prerequisites

Install the plugin from a Claude Code session:

```text
/plugin install create-hook@jel-claude-plugins
```

The skill generates shell scripts that use `jq` to parse tool input, so `jq` must be installed:

```bash
brew install jq   # macOS
apt install jq    # Linux
```

## Usage

```text
/create-hook
```

Or describe what you want:

```text
/create-hook run shellcheck before every file write
```

The skill walks you through five steps:

1. Pick the hook type — `PreToolUse` runs before the tool and can block it by exiting non-zero; `PostToolUse` runs after
2. Choose the tool matcher (`Bash`, `Write`, `Edit`, etc.)
3. Write a shell script in `.claude/hooks/`
4. Wire it into `.claude/settings.json` or `.claude/settings.local.json`
5. Test it by piping a sample payload and checking the exit code

Scripts read tool input from stdin as JSON via `jq`.
