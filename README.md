# standards.ai

A collection of AI rules templates and configuration files for AI-powered coding assistants.

## Overview

This repository provides ready-to-use configuration templates that define coding standards, project conventions, and behavioral guidelines for AI tools. Drop them into your projects to get consistent, high-quality AI assistance out of the box.

## Supported Tools

| Tool | Config File / Directory |
|------|------------------------|
| [Claude Code](https://docs.anthropic.com/en/docs/claude-code) | `CLAUDE.md` |
| [Cursor](https://www.cursor.com/) | `.cursor/` |

## Usage

Copy the relevant configuration files into the root of your project:

```sh
# Claude Code
cp templates/CLAUDE.md /path/to/your/project/CLAUDE.md

# Cursor
cp -r templates/.cursor /path/to/your/project/.cursor
```

Customize the templates to match your project's stack, conventions, and preferences.

## Contributing

Contributions are welcome. If you have templates for additional AI tools or improvements to existing ones, feel free to open a PR.

## License

MIT with No-Resale Restriction — free to use, modify, and distribute, but not to sell as a standalone product. See [LICENSE](LICENSE) for details.
