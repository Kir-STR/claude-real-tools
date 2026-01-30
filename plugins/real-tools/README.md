# Real Tools Plugin

> **Let's `/real-tools:dev` this!**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](https://github.com/Kir-STR/real-tools-marketplace/releases)

Build websites, components, and applications through structured, resumable workflows with specialized worker agents.

## Overview

The **Real Tools Plugin** provides a complete orchestration system for managing complex project workflows. The plugin consists of multiple components that work together: a skill that defines the orchestration workflow, specialized worker agents, commands for invocation, and hooks for workflow integration.

## Plugin Components

### Skills (1)

- **dev** - Orchestrates a 14-step workflow across 4 phases with state management, checkpointing, and human-in-the-loop approval gates

See [skills/dev/README.md](./skills/dev/README.md) for detailed skill documentation and usage guide.

### Agents (6)

Specialized worker agents automatically discovered from the `agents/` directory:

- **Analyst** (`agents/analyst.md`) - Requirements analysis and specification generation
- **Designer** (`agents/designer.md`) - UI/UX designs, architecture diagrams, system designs
- **Implementer** (`agents/implementer.md`) - Code implementation and technical development
- **Writer** (`agents/writer.md`) - Documentation and technical writing
- **Critique** (`agents/critique.md`) - Critical review and feedback
- **Tester** (`agents/tester.md`) - Test plan and test case generation

Claude Code automatically selects the appropriate agent based on task characteristics.

**Note:** The skill also utilizes Claude Code's built-in `Explore` and `Plan` agents for codebase exploration and planning tasks.

### Commands (1)

- **`/real-tools:dev <prd_path>`** - Invoke the dev skill to build your project

### Hooks (3)

- **SubagentStop** - Logs agent outputs to session log for audit trail
- **SessionStart** - Checks for interrupted dev sessions on startup and offers resumption
- **PostToolUse** - Validates `.supervisor/` directory and file integrity

Hook configuration: `hooks/hooks.json`

## Installation

### Via Marketplace (Recommended)

See the [marketplace README](../../README.md#installation) for installation via the Real Tools marketplace.

### Manual Installation

#### Clone and Symlink (Development)

```bash
# Clone the marketplace repository
git clone https://github.com/Kir-STR/real-tools-marketplace.git

# Create symlink to plugins directory
# Linux/Mac
ln -s $(pwd)/real-tools-marketplace/plugins/real-tools ~/.config/claude-code/plugins/real-tools

# Windows (PowerShell, as Administrator)
New-Item -ItemType SymbolicLink -Path "$env:APPDATA\claude-code\plugins\real-tools" -Target "$(Get-Location)\real-tools-marketplace\plugins\real-tools"
```

#### Manual Copy

```bash
# Clone and copy
git clone https://github.com/Kir-STR/real-tools-marketplace.git

# Linux/Mac
cp -r real-tools-marketplace/plugins/real-tools ~/.config/claude-code/plugins/real-tools

# Windows (PowerShell)
Copy-Item -Recurse real-tools-marketplace\plugins\real-tools "$env:APPDATA\claude-code\plugins\real-tools"
```

### Post-Installation

```bash
# Reload plugins (if not using marketplace)
/reload-plugins

# Verify the plugin is loaded
/plugins
```

## Quick Start

The Dev skill orchestrates project execution from PRD to final deliverable. For detailed usage, workflow phases, and examples, see the [skill documentation](./skills/dev/README.md).

### Basic Usage

```bash
# Create a PRD (Product Requirements Document)
# See skills/dev/README.md for PRD structure

# Let's dev this!
/real-tools:dev path/to/your_prd.md
```

The skill will guide you through the complete workflow with approval gates and checkpoints.

## Plugin Structure

For the full repository structure, see the [marketplace README](../../README.md#repository-structure).

This plugin is organized as:

```
real-tools/
├── .claude-plugin/plugin.json    # Plugin manifest
├── agents/                        # 6 worker agent definitions
├── commands/                      # /real-tools:dev command
├── hooks/                         # Workflow integration hooks
├── skills/dev/                    # Orchestration skill
└── README.md                      # This file
```

See individual directories for detailed component documentation.

## Configuration

### Customizing Agents

Agent definitions can be modified in the `agents/` directory. Each agent is defined in a separate markdown file with front matter:

```markdown
---
name: agent-name
description: Agent purpose
---

Agent instructions...
```

You can add custom agents by creating new files in the `agents/` directory following the same format.

### Hook Behavior

Hooks are configured in `hooks/hooks.json`. The plugin uses three hooks:

- **SubagentStop**: Triggered when any subagent completes, logs output to `.supervisor/session_log.json`
- **SessionStart**: Triggered on Claude Code startup, checks for interrupted dev sessions
- **PostToolUse**: Triggered after tool usage, validates state file integrity

Hook configuration: `hooks/hooks.json`

## Output Files

The skill creates all outputs in `.supervisor/`:

```
.supervisor/
├── state.json                    # Current workflow state
├── session_log.json              # Agent execution history
└── output/
    ├── exploration_report.md     # PRD analysis
    ├── plan.md                   # Execution plan
    ├── spec_schema.md            # Specification schema
    ├── plan_critique.md          # Plan review
    ├── task_spec.md              # Detailed specification
    ├── draft_*                   # Draft deliverable
    ├── feedback.md               # Your review feedback
    └── final_*                   # Final deliverable
```

## Documentation

- **Plugin README** (this file) - Plugin components and installation
- **Skill Guide** - [skills/dev/README.md](./skills/dev/README.md)
- **Usage Examples** - [skills/dev/examples.md](./skills/dev/examples.md)
- **Hook Configuration** - [hooks/hooks.json](./hooks/hooks.json)

## Edge Cases

### PRD Modified Between Sessions

If you modify the PRD file after starting a session:
- The dev orchestrator will use the original PRD path from state.json
- To use the modified PRD, either:
  - Start a fresh session (decline resumption)
  - Or manually update `prd_path` in state.json before resuming

### State File Corrupted

If `.supervisor/state.json` is corrupted or invalid:
- Delete `.supervisor/state.json`
- Re-run the command to start fresh
- Previous outputs in `.supervisor/output/` will be preserved

### Output Files Manually Deleted

If you delete files from `.supervisor/output/`:
- The dev orchestrator will detect missing files during resumption
- You will be prompted to either:
  - Restart from the step that produces the missing file
  - Or start a fresh session

### Multiple PRDs in Same Directory

To work on multiple projects in the same directory:
- Complete or cancel the current session first
- Archive `.supervisor/` directory:
  ```bash
  mv .supervisor .supervisor_project1
  ```
- Start new session for the second PRD

### Session Interrupted During HIL Gate

If interrupted while waiting for user input:
- Re-run the command
- The dev orchestrator will resume at the same HIL gate
- You will be prompted again for approval/feedback

## Troubleshooting

### Plugin Not Appearing

1. Verify the plugin is in the correct plugins directory
2. Try `/reload-plugins` or restart Claude Code
3. Check Claude Code logs for errors

### Commands Not Working

Ensure the plugin is properly installed and loaded (`/plugins` should list `real-tools`).

### Hook Issues

If hooks aren't working:
1. Check `hooks/hooks.json` is present
2. Verify hook paths are correct relative to plugin directory
3. Restart Claude Code after modifying hooks

For skill-specific issues (workflow, state, agents), see [skills/dev/README.md](./skills/dev/README.md).

## Version

Current version: 1.0.0

## License

MIT License - see [LICENSE](../../LICENSE) file for details.

## Support

For issues, questions, or feature requests:
- Open an issue on [GitHub](https://github.com/Kir-STR/real-tools-marketplace/issues)
- Check existing issues for solutions
- Review skill documentation: [skills/dev/README.md](./skills/dev/README.md)

## Author

**Kirill Bogdanov**

Plugin: `real-tools`
Marketplace: `real-tools-marketplace`

---

**Quick Reference:**
- Install: `/plugin marketplace add Kir-STR/real-tools-marketplace` → `/plugin install real-tools`
- Usage: `/real-tools:dev path/to/prd.md`
- Skill docs: [skills/dev/README.md](./skills/dev/README.md)
- Examples: [skills/dev/examples.md](./skills/dev/examples.md)
