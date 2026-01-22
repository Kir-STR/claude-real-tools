# Real Supervisor Plugin Marketplace

This repository includes a plugin marketplace configuration that allows easy distribution and installation of the Real Supervisor plugin.

## What is a Marketplace?

A Claude Code plugin marketplace is a catalog that distributes extensions to teams and communities. It provides:
- ✅ Centralized plugin discovery
- ✅ Version tracking and updates
- ✅ Support for multiple source types (Git, local paths, URLs)
- ✅ Automatic installation and configuration

## Marketplace Structure

### File Location
`.claude-plugin/marketplace.json`

### Schema
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
      "description": "Orchestrates complex, multi-step projects...",
      "version": "1.0.0",
      ...
    }
  ]
}
```

## Installation Methods

### Method 1: Install via GitHub (Recommended)

Users can install the plugin directly from the GitHub marketplace:

```bash
# Add the marketplace
/plugin marketplace add Kir-STR/claude-real-tools

# Install the plugin
/plugin install real-supervisor@real-tools
```

This method:
- ✅ Automatically pulls updates
- ✅ Handles version management
- ✅ Works for public repositories
- ✅ Simplest for end users

### Method 2: Clone and Symlink (Development)

For local development or testing:

```bash
# Clone the repository
git clone https://github.com/Kir-STR/claude-real-tools.git

# Create symlink to plugins directory
# Linux/Mac
ln -s $(pwd)/claude-real-tools ~/.config/claude-code/plugins/real-supervisor

# Windows (PowerShell, as Administrator)
New-Item -ItemType SymbolicLink -Path "$env:APPDATA\claude-code\plugins\real-supervisor" -Target "$(Get-Location)\claude-real-tools"

# Reload plugins
/reload-plugins
```

This method:
- ✅ Best for development and testing
- ✅ Changes reflect immediately
- ✅ No marketplace needed

### Method 3: Install from Local Marketplace

For testing the marketplace locally:

```bash
# Add local marketplace
/plugin marketplace add ./claude-real-tools

# Install from local marketplace
/plugin install real-supervisor@real-tools
```

## Team Distribution

### Auto-Install for Teams

Teams can configure automatic installation by adding to `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "real-tools": {
      "source": {
        "source": "github",
        "repo": "Kir-STR/claude-real-tools"
      }
    }
  },
  "enabledPlugins": {
    "real-supervisor@real-tools": true
  }
}
```

When team members clone the project:
1. Claude Code detects the marketplace configuration
2. Prompts to install the Real Supervisor plugin
3. Automatically enables it for the project

### Managed Marketplaces (Enterprise)

Organizations can restrict plugin sources via `strictKnownMarketplaces`:

```json
{
  "strictKnownMarketplaces": [
    {
      "source": "github",
      "repo": "Kir-STR/claude-real-supervisor-plugin"
    }
  ]
}
```

This ensures only approved plugins can be installed.

## Private Repository Access

For private repositories, set authentication tokens:

```bash
# GitHub
export GITHUB_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxx

# GitLab
export GITLAB_TOKEN=glpat_xxxxxxxxxxxxxxxxxxxx

# Bitbucket
export BITBUCKET_TOKEN=xxxxxxxxxxxxxxxxxxxxxxxx
```

## Plugin Source Configuration

For GitHub-based marketplaces, the source field should explicitly reference the repository:

```json
"source": {
  "source": "github",
  "repo": "Kir-STR/claude-real-tools"
}
```

This configuration:
- Works when marketplace is added from GitHub
- Plugin files are in the **same repository** as the marketplace
- Entire repository root is copied as the plugin
- Validates correctly with Claude Code's schema

### Alternative Source Types

If you want to distribute from a different repository or use local paths:

**Local Path (for local testing only):**
```json
{
  "name": "real-supervisor",
  "source": "."
}
```
Note: This only works when using `/plugin marketplace add ./path/to/repo`

**GitHub Source (for remote distribution):**
```json
{
  "name": "real-supervisor",
  "source": {
    "source": "github",
    "repo": "Kir-STR/claude-real-tools"
  }
}
```

**Git URL Source:**
```json
{
  "name": "real-supervisor",
  "source": {
    "source": "url",
    "url": "https://github.com/Kir-STR/claude-real-tools.git"
  }
}
```

## Validation & Testing

### Validate Marketplace Structure

```bash
# Validate the marketplace JSON
claude plugin validate .
# or
/plugin validate .
```

### Test Installation Flow

```bash
# 1. Add marketplace locally
/plugin marketplace add ./claude-real-tools

# 2. List available plugins
/plugin marketplace list

# 3. Install the plugin
/plugin install real-supervisor@real-tools

# 4. Verify installation
/plugins

# 5. Test the plugin
/rs .claude/skills/real-supervisor/examples/example_prd.md
```

## Version Management

### Updating the Plugin

When you release a new version:

1. Update version in `.claude-plugin/plugin.json`:
   ```json
   {
     "version": "1.1.0"
   }
   ```

2. Update version in `.claude-plugin/marketplace.json`:
   ```json
   {
     "metadata": {
       "version": "1.1.0"
     },
     "plugins": [
       {
         "version": "1.1.0"
       }
     ]
   }
   ```

3. Commit and push changes
4. Tag the release:
   ```bash
   git tag v1.1.0
   git push origin v1.1.0
   ```

5. Users update with:
   ```bash
   /plugin update real-supervisor@real-tools
   ```

### Semantic Versioning

Follow semver conventions:
- **MAJOR** (1.0.0 → 2.0.0): Breaking changes
- **MINOR** (1.0.0 → 1.1.0): New features (backward-compatible)
- **PATCH** (1.0.0 → 1.0.1): Bug fixes (backward-compatible)

## Marketplace vs Plugin.json

### `.claude-plugin/plugin.json` (Required)
The plugin manifest defines the plugin's structure and components:
- Required for all plugins
- Contains component paths (skills, commands, hooks)
- Specifies metadata (name, version, author)

### `.claude-plugin/marketplace.json` (Optional)
The marketplace catalog distributes plugins:
- Optional - only needed for marketplace distribution
- Can distribute multiple plugins from one repository
- Allows overriding plugin.json fields for marketplace listings
- Enables team/enterprise distribution

**Both files work together:**
1. Plugin is installed via marketplace entry
2. Claude Code reads `plugin.json` from the installed plugin
3. Marketplace metadata merges with plugin.json metadata

## Key Features

### Strict Mode (Default)
`"strict": true` (default behavior):
- Plugin source **must** contain `.claude-plugin/plugin.json`
- Marketplace fields **merge** with plugin.json
- Best for well-structured plugins

### Non-Strict Mode
`"strict": false`:
- Plugin source doesn't need plugin.json
- Marketplace entry defines **everything**
- Useful for wrapping external tools

Example:
```json
{
  "name": "external-tool",
  "source": "https://github.com/someone/tool.git",
  "strict": false,
  "commands": ["./scripts/tool.md"],
  "hooks": {...}
}
```

## Publishing Checklist

Before publishing your marketplace:

- ✅ Validate `.claude-plugin/marketplace.json` structure
- ✅ Ensure `.claude-plugin/plugin.json` exists and is valid
- ✅ Test local installation: `/plugin marketplace add .`
- ✅ Test plugin functionality after installation
- ✅ Document installation instructions in README
- ✅ Add GitHub repository URL to marketplace metadata
- ✅ Tag releases with semantic versions
- ✅ Update CHANGELOG.md with version history

## Troubleshooting

### Issue: "Marketplace not found"
**Solution**: Ensure the repository is public or authentication token is set.

### Issue: "Plugin has no plugin.json"
**Solution**: Verify `.claude-plugin/plugin.json` exists or set `"strict": false` in marketplace entry.

### Issue: "Invalid source path"
**Solution**: Use `"source": "."` for same-repo plugins, or full Git URL for external sources.

### Issue: "Plugin not appearing after install"
**Solution**: Run `/reload-plugins` or restart Claude Code.

## Resources

- **Official Docs**: https://code.claude.com/docs/en/plugin-marketplaces
- **Plugin Reference**: https://code.claude.com/docs/en/plugins-reference
- **Repository**: https://github.com/Kir-STR/claude-real-tools
- **Issues**: https://github.com/Kir-STR/claude-real-tools/issues

## Support

For marketplace-related questions:
1. Check the [official documentation](https://code.claude.com/docs/en/plugin-marketplaces)
2. Review example marketplace configurations
3. Open an issue on GitHub
4. Contact the marketplace owner

---

**Marketplace Owner**: Kirill Bogdanov
**Repository**: https://github.com/Kir-STR/claude-real-tools
**Version**: 1.0.0
