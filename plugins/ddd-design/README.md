# ddd-design

A Domain-Driven Design facilitation skill for Claude Code. It runs a structured discovery session — one question at a time — and produces a domain model document covering ubiquitous language, bounded contexts, aggregates, and domain events.

## Prerequisites

Install the plugin from a Claude Code session:

```text
/plugin install ddd-design@jel-claude-plugins
```

## Usage

Start a session from a Claude Code session:

```text
/ddd-design
```

Or with a hint:

```text
/ddd-design order management system
```

The skill explores your codebase first to surface implicit domain concepts, then interviews you through two layers:

- **Strategic:** domain name, actors, subdomains, bounded contexts, context map
- **Tactical:** aggregates, entities vs value objects, domain events

The skill challenges decisions that look off — an aggregate that should be a value object, a context boundary that creates tight coupling. You can always override its suggestions.

When all checklist items are resolved or deferred, it writes the domain model to `docs/domain/<domain-name>.md`.

## Output

The generated document includes:

- Ubiquitous language glossary
- Subdomain classification (core / supporting / generic)
- Bounded context responsibilities
- Context map with relationship types (upstream/downstream, shared kernel, Anti-Corruption Layer, open host service)
- Aggregates with entities, value objects, and invariants
- Domain events with producers and consumers
- Open questions
