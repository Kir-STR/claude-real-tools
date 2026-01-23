---
description: Orchestrates complex, multi-step projects through structured, resumable workflow
---

The Real Supervisor skill implements a supervisor-worker pattern to orchestrate complex projects from requirements to final deliverables.

## Usage

```
/supervisor <prd_file_path>
```

Or use the shorter alias:
```
/rs <prd_file_path>
```

## Arguments

- `prd_file_path` (required): Path to Product Requirements Document (markdown file)

If not provided as a command argument, the supervisor will prompt you to provide the path.
