# CLAUDE.md

## Project Type
Claude Code plugin marketplace **Real Tools**.

## Version Information

- **Marketplace Version**: 1.0.0
- **Repository**: https://github.com/Kir-STR/claude-real-tools

## Repository Structure

```
claude-real-tools/                               # Marketplace root
├── .claude-plugin/
│   └── marketplace.json                         # Marketplace configuration
├── plugins/
├── README.md                                    # Marketplace documentation (for users)
├── CLAUDE.md                                    # This file (for Claude Code)
└── LICENSE

```

### Notes

1. Plugins manifests goes in `plugins/{plugin-name}/.claude-plugin/plugin.json`
2. Plugin components (skills, commands, hooks) go under `plugins/{plugin-name}/`

## Do Not Create
- Additional documentation files unless explicitly requested
- README files in every directory
- Extensive comments in configuration files

## Important Notes

- This is a marketplace repository
- Users install via: `/plugin marketplace add Kir-STR/claude-real-tools`
- Keep all documentation concise and structured
