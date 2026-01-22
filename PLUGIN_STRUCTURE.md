# Real Supervisor Plugin Structure

This document describes the complete structure of the Real Supervisor plugin and how it conforms to the official Claude Code plugin specification.

## Official Plugin Files

### `.claude-plugin/plugin.json` ✅
**Status**: Compliant with official schema

The plugin manifest defines the plugin's structure and components. It follows the official Claude Code plugin specification:

```json
{
  "name": "real-supervisor",
  "version": "1.0.0",
  "description": "...",
  "author": {
    "name": "Kirill Bogdanov",
    "email": "",
    "url": "https://github.com/Kir-STR"
  },
  "homepage": "https://github.com/Kir-STR/claude-real-tools",
  "repository": "https://github.com/Kir-STR/claude-real-tools.git",
  "license": "MIT",
  "keywords": [...],
  "skills": "./.claude/skills/",
  "hooks": "./hooks/hooks.json"
}
```

**Key Fields:**
- `name` (required): Unique identifier in kebab-case
- `version`: Semantic versioning (MAJOR.MINOR.PATCH)
- `author`: Object with name, email, url
- `repository`: String URL (not object)
- `skills`: Path to skills directory
- `hooks`: Path to hooks configuration

**Changes from original:**
- ❌ Removed: `displayName`, `main`, `settings`, `dependencies`, `engines`, `categories`
- ✅ Fixed: `author` format (string → object)
- ✅ Fixed: `repository` format (object → string)
- ✅ Fixed: `skills` format (array → path string)
- ✅ Added: `hooks` field
- ✅ Added: `homepage` field

### `.claude-plugin/marketplace.json` ✅
**Status**: Optional, compliant with marketplace specification

The marketplace configuration enables distribution through the Claude Code marketplace system:

```json
{
  "name": "real-tools",
  "owner": {
    "name": "Kirill Bogdanov",
    "email": ""
  },
  "metadata": {
    "description": "Official marketplace for the Real Supervisor plugin",
    "version": "1.0.0"
  },
  "plugins": [
    {
      "name": "real-supervisor",
      "source": {
        "source": "github",
        "repo": "Kir-STR/claude-real-tools"
      },
      "description": "...",
      "version": "1.0.0",
      ...
    }
  ]
}
```

**Key Fields:**
- `name` (required): Marketplace identifier in kebab-case
- `owner` (required): Maintainer information (name required, email optional)
- `plugins` (required): Array of plugin entries
- `metadata` (optional): Marketplace description and version

**Purpose:**
- Enables marketplace-based installation: `/plugin marketplace add Kir-STR/claude-real-tools`
- Provides centralized distribution and version management
- Allows team/enterprise configuration
- See [MARKETPLACE.md](MARKETPLACE.md) for complete documentation

## Directory Structure

```
real-supervisor/
├── .claude/                      # Skill configuration
│   ├── settings.json            # Project settings (minimal)
│   └── skills/
│       └── real-supervisor/     # The main skill
│           ├── SKILL.md         # Core supervisor instructions
│           ├── agents.md        # Worker agent definitions
│           └── examples/        # Templates and examples
│
├── .claude-plugin/              # Plugin metadata
│   ├── plugin.json             # REQUIRED: Plugin manifest
│   └── marketplace.json        # OPTIONAL: Marketplace distribution config
│
├── commands/                    # Command shortcuts (auto-discovered)
│   ├── supervisor.md           # /supervisor command
│   └── rs.md                   # /rs command (short alias)
│
└── hooks/                       # Plugin hooks (auto-loaded)
    ├── hooks.json              # Hook definitions
    └── README.md               # Hooks documentation
```

## Component Breakdown

### 1. Skills (`skills: "./.claude/skills/"`)

**Location**: `.claude/skills/real-supervisor/`
**Entry Point**: `SKILL.md`
**Auto-discovery**: Yes
**Creates Command**: `/real-supervisor`

The skill is the core of the plugin, containing:
- Main orchestration logic (`SKILL.md`)
- Worker agent definitions (`agents.md`)
- Example files and templates (`examples/`)

**How it works:**
- Claude Code automatically discovers skills in the specified path
- Skills with `<name>/SKILL.md` structure create `/name` commands
- The skill can be invoked manually or auto-invoked by Claude when relevant

### 2. Commands (`commands/` - default location)

**Location**: `commands/`
**Auto-discovery**: Yes
**Files**:
- `supervisor.md` - Creates `/supervisor` command
- `rs.md` - Creates `/rs` command (alias)

**How it works:**
- Claude Code automatically scans the `commands/` directory
- Each `.md` file creates a corresponding `/filename` command
- Commands are simpler than skills - just markdown instructions
- Both `/supervisor` and `/rs` invoke the Real Supervisor skill

### 3. Hooks (`hooks: "./hooks/hooks.json"`)

**Location**: `hooks/hooks.json`
**Auto-loading**: Yes when plugin is enabled
**Hooks defined**:
- `SubagentStop` - Logs agent outputs to session log
- `SessionStart` - Checks for interrupted sessions
- `PostToolUse` (Write|Edit) - Validates `.supervisor/` files

**How it works:**
- Plugin hooks are automatically merged with user/project hooks
- Uses `${CLAUDE_PLUGIN_ROOT}` environment variable for paths
- Hooks run whenever the plugin is enabled
- Multiple hooks from different sources run in parallel

## Usage

### Installation

**Recommended: Via Marketplace**
```bash
# Add marketplace
/plugin marketplace add Kir-STR/claude-real-tools

# Install plugin
/plugin install real-supervisor@real-tools
```

**Alternative: Clone and Symlink (for development)**
```bash
# Clone and symlink
git clone https://github.com/Kir-STR/claude-real-tools.git
ln -s $(pwd)/claude-real-tools ~/.config/claude-code/plugins/real-supervisor

# Reload in Claude Code
/reload-plugins
```

See [MARKETPLACE.md](MARKETPLACE.md) for complete installation options and team distribution.

### Invocation Options

Users can invoke the supervisor in multiple ways:

1. **Skill command**: `/real-supervisor path/to/prd.md`
2. **Full command**: `/supervisor path/to/prd.md`
3. **Short alias**: `/rs path/to/prd.md`

All three are equivalent and activate the same skill.

### Auto-discovery

When the plugin is installed:
- ✅ Skills are discovered from `./.claude/skills/`
- ✅ Commands are discovered from `commands/`
- ✅ Hooks are loaded from `./hooks/hooks.json`
- ✅ All create appropriate shortcuts and integrations

## File Paths & Environment Variables

### In `plugin.json`
- All paths must be relative to plugin root
- Must start with `./`
- Example: `"skills": "./.claude/skills/"`

### In `hooks/hooks.json`
- Use `${CLAUDE_PLUGIN_ROOT}` for plugin files
- Use `${CLAUDE_PROJECT_DIR}` for project files
- Example: `"command": "${CLAUDE_PLUGIN_ROOT}/scripts/process.sh"`

## Compliance Checklist

✅ Required manifest at `.claude-plugin/plugin.json`
✅ Valid `name` field (kebab-case)
✅ Proper `author` object format
✅ Repository as string URL
✅ Skills path points to directory with `<name>/SKILL.md` structure
✅ Hooks path points to valid `hooks.json` file
✅ Commands in default `commands/` directory
✅ No invalid/custom fields in plugin.json
✅ All component paths are relative and start with `./`
✅ Directory structure follows official conventions

## References

- **Official Plugin Docs**: https://code.claude.com/docs/en/plugins-reference
- **Marketplace Documentation**: https://code.claude.com/docs/en/plugin-marketplaces
- **Hooks Documentation**: https://code.claude.com/docs/en/hooks#plugin-hooks
- **Plugin Manifest**: `.claude-plugin/plugin.json`
- **Marketplace Config**: `.claude-plugin/marketplace.json`
- **Main Skill**: `.claude/skills/real-supervisor/SKILL.md`
- **Hooks Config**: `hooks/hooks.json`
- **Distribution Guide**: [MARKETPLACE.md](MARKETPLACE.md)

## Version History

### 1.0.0 (2026-01-21)
- Restructured plugin to match official specification
- Fixed plugin.json schema compliance
- Added proper commands for /supervisor and /rs
- Moved hooks to hooks/hooks.json
- Updated all documentation
