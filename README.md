# Real Tools Marketplace

Claude Code plugin marketplace **Real Tools**.

## Version Information

- **Marketplace Version**: 1.0.0
- **Repository**: https://github.com/Kir-STR/claude-real-tools

## What's Included

Plugins with components (skills, commands, hooks) under `plugins/{plugin-name}/`.

## Quick Install (Recommended)

```bash
# Add the marketplace
/plugin marketplace add Kir-STR/claude-real-tools
```

## Repository Structure

```
claude-real-tools/                    # Marketplace root
├── .claude-plugin/
│   └── marketplace.json              # Marketplace configuration
├── plugins/
├── CLAUDE.md                         # Instructions for Claude Code
├── LICENSE                           # MIT License
└── README.md                         # This file
```

## Team Configuration

Add to your project's `.claude/settings.json` for automatic installation:

```json
{
  "extraKnownMarketplaces": {
    "real-tools": {
      "source": {
        "source": "github",
        "repo": "Kir-STR/claude-real-tools"
      }
    }
  }
}
```

## Documentation

- **Plugins Guides**: `plugins/{plugin-name}/README.md`
- **Skills Guides**:  `plugins/{plugin-name}/skills/{skill-name}/README.md`
- **Usage Examples**: `plugins/{plugin-name}/skills/{skill-name}/examples.md`

---

**Author**: Kirill Bogdanov
**Marketplace**: real-tools
