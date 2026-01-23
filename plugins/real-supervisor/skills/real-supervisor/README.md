# Real Supervisor Skill

The Real Supervisor orchestrates complex, multi-step projects through a structured, resumable workflow with specialized worker agents.

## Overview

The supervisor implements a **supervisor-worker pattern** where:
- **Supervisor** manages the workflow, coordinates agents, and handles state
- **Workers** are specialized agents that perform specific tasks

## Workflow

The supervisor executes a **14-step workflow** across **4 phases**:

### Phase 1: Initialization (Steps 1-2)
**Purpose**: Check for existing session and initialize or resume

- Checks for existing session state
- Initializes new session or resumes interrupted session

### Phase 2: Planning & Specification (Steps 3-9)
**Purpose**: Analyze requirements, create execution plan, and generate detailed specification

- Step 3: Explore PRD and extract requirements
- Steps 4-5: Create execution plan and specification schema
- Steps 6-7: Critique plan and get user approval (**HIL Gate 1**)
- Step 8: Generate detailed specification conforming to schema
- Step 9: Create initial checkpoint (use `/rewind` to return here)

### Phase 3: Draft Creation & Review (Steps 10-12)
**Purpose**: Create draft deliverable, collect user feedback, and checkpoint

- Step 10: Create draft deliverable using specialized worker agent (Designer/Implementer/Writer)
- Step 11: Present draft for user review and feedback (**HIL Gate 2**)
- Step 12: Create draft checkpoint before final execution

### Phase 4: Final Delivery (Steps 13-14)
**Purpose**: Create final deliverable and complete workflow

- Step 13: Create final deliverable incorporating user feedback
- Step 14: Archive session and present all deliverables

## Worker Agents

The supervisor delegates to **7 specialized agents**:

1. **Explore** - PRD analysis and requirements extraction (built-in Claude Code agent)
2. **Analyst** - Requirements analysis and specification generation
3. **Designer** - UI/UX designs, architecture diagrams, system design
4. **Implementer** - Code implementation and technical development
5. **Writer** - Documentation, technical writing, content creation
6. **Critique** - Critical review and constructive feedback
7. **Tester** - Testing strategies, test cases, quality assurance

The supervisor selects the appropriate agent based on deliverable type and task description.

## Workflow Diagram

```
┌──────────────────────────────────────────────┐
│  USER: /rs path/to/prd.md                    │
└────────────────────┬─────────────────────────┘
                     ▼
┌──────────────────────────────────────────────┐
│  PHASE 1: Initialization (Steps 1-2)        │
│  Check state → Resume or Start Fresh         │
└────────────────────┬─────────────────────────┘
                     ▼
┌──────────────────────────────────────────────┐
│  PHASE 2: Planning & Specification (3-9)    │
│                                              │
│  Step 3:  Explore PRD → exploration_report  │
│  Step 4:  Create plan.md                    │
│  Step 5:  Define spec_schema.md             │
│  Step 6:  Critique → plan_critique.md       │
│  Step 7:  HIL Gate 1: User approves plan ◄┐ │
│                └─ Changes? ─────────────────┘ │
│  Step 8:  Generate task_spec.md            │
│  Step 9:  Checkpoint (can /rewind here)    │
└────────────────────┬─────────────────────────┘
                     ▼
┌──────────────────────────────────────────────┐
│  PHASE 3: Draft Creation & Review (10-12)  │
│                                              │
│  Step 10: Create draft_[type].[ext]        │
│  Step 11: HIL Gate 2: User reviews ◄──┐    │
│                └─ Feedback? → feedback.md   │
│  Step 12: Checkpoint (can /rewind here)    │
└────────────────────┬─────────────────────────┘
                     ▼
┌──────────────────────────────────────────────┐
│  PHASE 4: Final Delivery (13-14)            │
│                                              │
│  Step 13: Create final_[type].[ext]        │
│           (incorporates feedback)            │
│  Step 14: Archive & present deliverables   │
└────────────────────┬─────────────────────────┘
                     ▼
                ┌─────────┐
                │ SUCCESS │
                └─────────┘
```

## State Management

All session state is stored in `.supervisor/`:

```
.supervisor/
├── state.json                    # Current workflow state
├── session_log.json              # Complete agent execution log
└── output/
    ├── exploration_report.md     # Step 3
    ├── plan.md                   # Step 4
    ├── spec_schema.md            # Step 5
    ├── plan_critique.md          # Step 6
    ├── task_spec.md              # Step 8
    ├── draft_[type].[ext]        # Step 10
    ├── feedback.md               # Step 11 (if provided)
    └── final_[type].[ext]        # Step 13
```

### state.json Structure

```json
{
  "session_id": "timestamp-based-id",
  "current_phase": "phase_name",
  "current_step": 1,
  "prd_path": "path/to/prd.md",
  "artifacts": {
    "prd": "path/to/prd.md",
    "exploration_report": ".supervisor/output/exploration_report.md",
    "plan": ".supervisor/output/plan.md",
    "spec_schema": ".supervisor/output/spec_schema.md",
    "task_spec": ".supervisor/output/task_spec.md",
    "draft": ".supervisor/output/draft_*.ext",
    "feedback": ".supervisor/output/feedback.md",
    "final": ".supervisor/output/final_*.ext"
  },
  "checkpoints": {
    "initial": 9,
    "draft": 12
  },
  "completed_steps": [1, 2, 3, ...]
}
```

## Resumption

If a session is interrupted, simply re-run the command:

```bash
/rs path/to/prd.md
```

The supervisor:
1. Detects existing `state.json`
2. Reads the last completed step
3. Asks if you want to resume or start fresh
4. If resuming, continues from the next step

## Human-in-the-Loop (HIL) Gates

Two mandatory approval points:

### HIL Gate 1: Plan Approval (Step 7)
Review and approve the execution plan before work begins.

**Options:**
- **Approve** → Continue to specification
- **Request Changes** → Provide feedback, plan is revised, re-presented
- **Cancel** → Abort the task

### HIL Gate 2: Draft Review (Step 11)
Review the draft deliverable and provide feedback.

**Options:**
- **Approve** → Continue to final version (feedback: "approved")
- **Provide Feedback** → Save feedback, incorporated in final version
- **Reject** → Return to draft creation with rejection reasons

## Checkpoints

Two checkpoints enable safe rollback via `/rewind`:

1. **Initial Checkpoint (Step 9)** - Before draft creation
   - Use to change the specification or approach

2. **Draft Checkpoint (Step 12)** - Before final execution
   - Use to revise based on different feedback

## PRD Requirements

Your PRD should include:

```markdown
# Project Title

## Overview
Brief project description

## Objectives
What you want to achieve

## Functional Requirements
- Requirement 1
- Requirement 2
- ...

## Non-Functional Requirements (Optional)
- Performance requirements
- Security requirements
- ...

## Deliverable
What the final output should be (code, design, documentation, etc.)
```

### Complete PRD Template

```markdown
# Product Requirements Document: [Project Name]

## Overview
We need to build [description]. The [system/application/deliverable] should [main purpose].

## Objectives
1. [Primary objective]
2. [Secondary objective]
3. [Additional objectives]

## Functional Requirements

### FR-1: [Feature Category]
- [Specific requirement 1]
- [Specific requirement 2]
- [Specific requirement 3]

### FR-2: [Feature Category]
- [Specific requirement 1]
- [Specific requirement 2]

## Non-Functional Requirements

### NFR-1: Performance
- [Performance requirement with metrics]
- [Response time requirements]
- [Throughput requirements]

### NFR-2: Security
- [Security requirement 1]
- [Security requirement 2]
- [Authentication/authorization requirements]

### NFR-3: Scalability
- [Scalability requirements]
- [Concurrent user support]

## Technical Constraints (Optional)
- Technology stack preferences
- Platform requirements
- Integration requirements

## Deliverable
A complete [type] with:
- [Component 1]
- [Component 2]
- [Documentation requirements]
- [Setup/deployment instructions]
```

See `examples.md` for complete PRD examples.

## Usage Examples

### Example 1: Building a REST API

```bash
# Create your PRD
cat > api_prd.md << 'EOF'
# Task Management REST API

## Overview
Build a RESTful API for task management with CRUD operations.

## Requirements
- User authentication with JWT
- Task CRUD endpoints
- Task filtering and search
- PostgreSQL database

## Deliverable
Complete Node.js/Express application with API endpoints
EOF

# Run the supervisor
/rs api_prd.md
```

The supervisor will:
1. Analyze API requirements
2. Create implementation plan
3. Present plan for your approval
4. Generate detailed specification
5. Create initial checkpoint
6. Implement draft API code (using Implementer agent)
7. Present draft for your review
8. Create draft checkpoint
9. Incorporate your feedback
10. Deliver final API implementation

### Example 2: Creating a System Design

```bash
# Create design PRD
cat > design_prd.md << 'EOF'
# E-Commerce Platform Architecture

## Overview
Design the system architecture for a scalable e-commerce platform.

## Requirements
- Microservices architecture
- Handle 10,000 concurrent users
- Support multiple payment gateways
- Real-time inventory management

## Deliverable
Comprehensive architecture document with diagrams and component descriptions
EOF

# Run the supervisor
/supervisor design_prd.md
```

The supervisor selects the **Designer** agent to create comprehensive architecture documentation with diagrams.

### Example 3: Generating Documentation

```bash
# Create documentation PRD
cat > docs_prd.md << 'EOF'
# API Documentation for Payment Service

## Overview
Create comprehensive API documentation for our payment processing service.

## Requirements
- Document all REST endpoints
- Include request/response examples
- Authentication and authorization guide
- Error codes and handling
- Rate limiting information

## Deliverable
Markdown documentation with code examples
EOF

/rs docs_prd.md
```

The supervisor selects the **Writer** agent to create detailed technical documentation.

### Example 4: Resuming After Interruption

```bash
# Original invocation (interrupted during draft creation)
/rs project_prd.md
# ... session interrupted ...

# Later, resume the session
/rs project_prd.md
# Supervisor detects previous session and asks to resume
# Select "Yes" to continue from where you left off
```

### Example 5: Using Checkpoints

```bash
# Run the workflow
/rs implementation_prd.md
# ... workflow completes through draft review ...
# You approve the draft
# ... final version is created ...

# Later, you want to provide different feedback
/rewind
# Select the "Draft Checkpoint" to return
# Provide new feedback
# Get a different final version
```

## Error Handling

If any step fails:
1. Error is logged in `session_log.json` with `status: "failed"`
2. Workflow does NOT advance to next step
3. You are notified of the failure
4. Options: Retry step, Modify inputs, or Cancel

## Advanced Usage

### Modifying the Specification Schema

If the default schema doesn't fit your needs, you can provide a custom schema:

1. Create your own `custom_schema.md`
2. Reference it in your PRD under a "Specification Schema" section
3. The supervisor will use your custom schema instead

### Providing Early Feedback

At the plan approval stage (Step 7), you can:
- **Approve** the plan as-is to proceed
- **Request specific changes** to the plan
- **Cancel** if the approach isn't suitable

This ensures you're aligned with the approach before implementation begins.

### Iterating on Drafts

At the draft review stage (Step 11), you can:
- **Approve** the draft and proceed to final version
- **Provide detailed feedback** for improvements
- **Reject** the draft and request a complete redo

Your feedback is saved to `feedback.md` and incorporated into the final version.

### Working with Large Projects

For complex projects:
1. Break the PRD into phases if needed
2. Use checkpoints to iterate on specific parts
3. Review `session_log.json` to understand agent decisions
4. Use `/rewind` to try different approaches

## Troubleshooting

### Issue: "No PRD file found at path"

**Solution**: Ensure the PRD file path is correct and the file exists.

```bash
# Check if file exists
ls path/to/prd.md

# Use absolute path if needed
/rs /full/path/to/prd.md
```

### Issue: Agent fails during execution

**Solution**: The supervisor will log the error and ask if you want to retry, modify inputs, or cancel. Check the session log for details:

```bash
# View session log
cat .supervisor/session_log.json

# Look for entries with "status": "failed"
```

### Issue: Can't resume previous session

**Solution**: Ensure `.supervisor/state.json` exists. If corrupted:

```bash
# Check if state file exists
ls .supervisor/state.json

# If corrupted, delete and start fresh
rm -rf .supervisor/
/rs path/to/prd.md
```

### Issue: Final deliverable doesn't match expectations

**Solution**: Use `/rewind` to return to the draft checkpoint, provide more detailed feedback, and regenerate the final version.

```bash
# Rewind to draft checkpoint
/rewind
# Select "Draft Checkpoint"
# Workflow resumes at draft review (Step 11)
# Provide detailed, specific feedback
# Final version regenerated with new feedback
```

### Issue: Workflow stuck at approval gate

**Solution**: The supervisor is waiting for your input. Respond to the approval prompt to continue.

### Issue: State file shows wrong step

**Solution**: The state file may be corrupted. You can either:
1. Delete `.supervisor/` and start fresh
2. Manually edit `.supervisor/state.json` to correct the step (advanced)

## Best Practices

1. **Write detailed PRDs** - Better requirements lead to better results
2. **Review plans carefully** at HIL Gate 1 - This is your chance to shape the approach
3. **Provide specific feedback** during draft review - Be explicit about what needs to change
4. **Use `/rewind`** liberally - Don't hesitate to go back to checkpoints
5. **Check `session_log.json`** for complete audit trail of all agent activity
6. **Organize PRDs** in a dedicated directory for easy management
7. **Be specific in deliverable requirements** - Clearly state what format and content you expect

## Limitations

The supervisor explicitly avoids work not required by the PRD:
- Won't create extensive documentation beyond specifications unless requested
- Won't perform complex analytics unless specified in PRD
- Won't create deployment pipelines or CI/CD unless included in deliverable requirements
- Won't create automated test suites unless explicitly required in PRD

**Focus**: Core task execution from PRD to deliverable as specified.

## Components

The Real Supervisor skill consists of:

- **SKILL.md** - Core supervisor instructions for Claude Code
- **examples.md** - Usage examples, common patterns, and structure examples
- **README.md** - This file (skill documentation for users)

Worker agents are defined in `../../agents/` directory (7 specialized agents).

## Architecture

The supervisor implements these principles:

1. **File-Based Operations** - All inputs/outputs are files for transparency and version control
2. **Supervisor Pattern** - Decompose complex tasks, delegate to specialists, aggregate results
3. **Spec-Driven Communication** - Structured schemas ensure reliable agent handoffs
4. **State Persistence** - Robust state management enables resumption after interruptions
5. **Human-in-the-Loop** - User approval at critical decision points (plan approval, draft review)
6. **Checkpointing** - Integration with `/rewind` for safe rollback to known states

## Related Documentation

- **Plugin README** - [../../README.md](../../README.md) - Plugin components and installation
- **Examples** - [examples.md](./examples.md) - Detailed usage patterns and structure examples
- **Skill Instructions** - SKILL.md - Instructions for Claude Code (internal)

## Version

Skill version: 1.0.0

Part of Real Supervisor Plugin v1.0.0
