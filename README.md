# Ray Skills

A skill for AI coding agents to interact with the [Ray app](https://myray.app/).

Ray is a desktop application for debugging that displays information sent to it via HTTP. This skill teaches AI coding agents how to send payloads to Ray's local HTTP server.

## Installation

```bash
npx skills add spatie/ray-skills
```

This will install the skill into your AI coding agent. The agent will automatically learn how to send data to Ray when needed.

Works with Claude Code, Cursor, Cline, GitHub Copilot, and other AI coding agents.

## Requirements

- [Ray desktop app](https://myray.app/) running locally
- Ray listens on `http://localhost:23517/` by default

## Skill Structure

```
skills/ray/
├── SKILL.md              # Main skill definition
└── rules/
    ├── ray-local-http.md # HTTP API documentation
    ├── log.md            # Log payload documentation
    ├── table.md          # Table payload documentation
    └── json.md           # JSON payload documentation
```

## Documentation

- [Ray Payload Documentation](https://myray.app/docs/developing-ray-libraries/payload) - Official Ray payload structure documentation
- [Spatie Ray Ecosystem](skills/ray/rules/spatie-ray-ecosystem.md) - Overview of all Spatie Ray packages for different languages and frameworks

## License

The MIT License (MIT). Please see [License File](./LICENSE.md) for more information.
