# Dev Skill Usage Examples

> **Let's `/real-tools:dev` this!**

This document provides practical examples of using the Dev skill for various project types.

## Example 1: Build a REST API

### Scenario
You need to implement a RESTful API for a task management system.

### PRD
Create `api_requirements.md`:

```markdown
# Task Management API

## Overview
Build a RESTful API for managing tasks with user authentication.

## Objectives
- Provide CRUD operations for tasks
- Implement user authentication and authorization
- Support task filtering and sorting

## Functional Requirements
- User registration and login with JWT tokens
- Create, read, update, delete tasks
- Assign tasks to users
- Filter tasks by status, priority, assignee
- Sort tasks by due date, creation date

## Non-Functional Requirements
- RESTful architecture
- JSON responses
- Error handling with appropriate HTTP status codes
- Input validation

## Deliverable
Complete API implementation with Express.js, including route handlers, middleware, and database models.
```

### Invocation

```bash
/real-tools:dev api_requirements.md
```

### Workflow

1. **Explore** - Analyzes API requirements, identifies endpoints, authentication needs
2. **Plan** - Creates implementation plan:
   - Express.js setup
   - Authentication middleware
   - Task CRUD routes
   - Database schema
3. **Approve** - You review the plan
4. **Specification** - Analyst generates detailed spec with route definitions, schemas, validation rules
5. **Checkpoint** - Initial checkpoint created
6. **Draft** - Implementer creates draft API code
7. **Review** - You test the draft, provide feedback
8. **Checkpoint** - Draft checkpoint created
9. **Final** - Final API implementation with your feedback incorporated

### Expected Output

```
.supervisor/output/
├── exploration_report.md
├── plan.md
├── spec_schema.md
├── task_spec.md
├── draft_api.js
├── feedback.md
└── final_api.js
```

---

## Example 2: Design System Architecture

### Scenario
You need architecture documentation for a microservices-based e-commerce platform.

### PRD
Create `architecture_prd.md`:

```markdown
# E-Commerce Platform Architecture

## Overview
Design a scalable microservices architecture for an e-commerce platform handling 100k+ daily users.

## Objectives
- Design microservices decomposition
- Define service boundaries and responsibilities
- Plan data flow and communication patterns
- Ensure scalability and fault tolerance

## Functional Requirements
- User service (authentication, profiles)
- Product catalog service
- Shopping cart service
- Order processing service
- Payment service
- Notification service

## Non-Functional Requirements
- Horizontal scalability
- Service isolation
- API gateway pattern
- Async communication where appropriate
- Database per service pattern

## Deliverable
Architecture documentation with:
- System overview diagram
- Service interaction diagrams
- Data flow diagrams
- Technology stack recommendations
- Deployment architecture
```

### Invocation

```bash
/real-tools:dev architecture_prd.md
```

### Key Difference
The dev orchestrator automatically selects the **Designer** agent (instead of Implementer) because the deliverable is architecture documentation with diagrams.

### Expected Output

```
.supervisor/output/
├── exploration_report.md
├── plan.md
├── spec_schema.md
├── task_spec.md
├── draft_architecture.md (with mermaid diagrams)
├── feedback.md
└── final_architecture.md
```

---

## Example 3: Write Technical Documentation

### Scenario
Create comprehensive documentation for an existing CLI tool.

### PRD
Create `docs_requirements.md`:

```markdown
# CLI Tool Documentation

## Overview
Create user-facing documentation for the 'deploy-kit' CLI tool.

## Objectives
- Document all commands and options
- Provide usage examples
- Include troubleshooting guide

## Functional Requirements
- Getting started guide
- Command reference for 10 commands
- Configuration file documentation
- Example workflows (dev, staging, prod deployments)
- Common error messages and solutions

## Non-Functional Requirements
- Clear, beginner-friendly language
- Code examples that can be copy-pasted
- Consistent formatting
- Table of contents

## Deliverable
Complete markdown documentation ready for GitHub wiki.
```

### Invocation

```bash
/real-tools:dev docs_requirements.md
```

### Key Difference
The dev orchestrator selects the **Writer** agent for documentation deliverables.

### Expected Output

```
.supervisor/output/
├── exploration_report.md
├── plan.md
├── spec_schema.md
├── task_spec.md
├── draft_documentation.md
├── feedback.md
└── final_documentation.md
```

---

## Example 4: Resume Interrupted Session

### Scenario
Your session was interrupted during draft creation. You want to resume.

### Steps

1. **Re-run the same command:**
   ```bash
   /real-tools:dev api_requirements.md
   ```

2. **Dev orchestrator detects existing state:**
   - Reads `.supervisor/state.json`
   - Finds `completed_steps: [1, 2, 3, 4, 5, 6, 7, 8, 9]`
   - Last completed: Step 9 (checkpoint)

3. **Dev orchestrator prompts:**
   ```
   Previous session detected for 'api_requirements.md'
   Last completed: Step 9 (Checkpoint Initial)

   Resume from Step 10 (Execute Draft)?
   [ ] Yes, resume from Step 10
   [ ] No, start fresh
   ```

4. **You select "Yes":**
   - Dev orchestrator loads all artifact paths from state
   - Continues from Step 10
   - Invokes Implementer agent with existing task_spec.md

---

## Example 5: Using Checkpoints with /rewind

### Scenario
You've completed the draft but want to go back and modify the specification before creating the final version.

### Steps

1. **Complete workflow through Step 11** (draft review)

2. **Realize specification needs changes**

3. **Use /rewind to go back to initial checkpoint:**
   ```bash
   /rewind
   ```
   (Select the checkpoint from Step 9)

4. **State reverts to Step 9:**
   - All files from Step 10+ are archived
   - Dev orchestrator resumes from Step 9
   - You can now modify task_spec.md or even go back further

5. **Re-run dev command:**
   ```bash
   /real-tools:dev api_requirements.md
   ```
   Dev orchestrator continues from Step 10 with modified context.

---

## Example 6: Providing Detailed Feedback

### Scenario
The draft implementation is close but needs specific changes.

### HIL Gate 2: Draft Review (Step 11)

When presented with the draft, provide structured feedback:

```markdown
# Feedback on Draft API

## What Works Well
- Authentication middleware is clean
- Error handling is comprehensive
- Route structure is logical

## Required Changes
1. **Task validation**: Add validation for task priority field (must be 'low', 'medium', 'high')
2. **Sorting**: The sort by due date doesn't handle null due dates correctly
3. **Response format**: Wrap all responses in { success, data, error } envelope

## Nice to Have
- Add request logging middleware
- Consider adding rate limiting

## Specific Code Changes
In `routes/tasks.js`, line 45:
```javascript
// Current
const tasks = await Task.find({ assignee: userId }).sort({ dueDate: 1 });

// Change to
const tasks = await Task.find({ assignee: userId })
  .sort({ dueDate: 1, _id: 1 })
  .where('dueDate').ne(null);
```
```

This feedback is saved to `feedback.md` and used by the Implementer agent in Step 13.

---

## Example 7: Canceling and Starting Fresh

### Scenario
During plan approval (Step 7), you realize the approach is wrong.

### Steps

1. **HIL Gate 1 presents the plan**

2. **You select "Cancel":**
   - Dev orchestrator updates state with cancellation status
   - Session stops
   - All artifacts remain in `.supervisor/output/` for reference

3. **Modify your PRD** based on what you learned

4. **Start fresh:**
   ```bash
   /real-tools:dev updated_requirements.md
   ```

---

## Example 8: Complex Multi-Component Project

### Scenario
Build a full-stack feature: backend API + React frontend.

### PRD
Create `feature_requirements.md`:

```markdown
# User Profile Feature

## Overview
Implement user profile management with backend API and React frontend.

## Functional Requirements

### Backend
- GET /api/profile - Fetch user profile
- PUT /api/profile - Update user profile
- POST /api/profile/avatar - Upload avatar image
- Validation and error handling

### Frontend
- Profile view component
- Profile edit form component
- Avatar upload with preview
- Form validation
- API integration

## Deliverable
Complete feature with backend routes and React components, ready to integrate.
```

### Invocation

```bash
/real-tools:dev feature_requirements.md
```

### Workflow Notes

- **Specification** - Analyst creates comprehensive spec covering both backend and frontend
- **Draft** - Implementer may create both backend and frontend code
- **Feedback** - You can request changes to either part
- **Final** - Integrated feature with both components

---

## Tips for Better Results

### Write Detailed PRDs
```markdown
# Good PRD
- Clear objectives
- Specific requirements
- Acceptance criteria
- Examples of expected behavior

# Poor PRD
- Vague goals
- Missing details
- No examples
```

### Use Structured Feedback
- Separate "must fix" from "nice to have"
- Reference specific parts of the draft
- Provide concrete examples
- Explain the "why" behind changes

### Leverage Checkpoints
- Initial checkpoint (Step 9) - Go back to change specification
- Draft checkpoint (Step 12) - Go back to revise draft approach

### Check Session Log
```bash
cat .supervisor/session_log.json
```
Complete audit trail of all agent invocations, useful for debugging.

### Archive Old Sessions
Before starting a new project in the same directory:
```bash
mv .supervisor .supervisor_old_$(date +%Y%m%d_%H%M%S)
```

---

## Common Patterns

### Pattern 1: Iterative Refinement
1. Start with minimal PRD
2. Review draft
3. Provide feedback for improvements
4. Get final version
5. If needed, use `/rewind` and modify specification for next iteration

### Pattern 2: Phased Delivery
1. PRD for Phase 1 only
2. Complete workflow
3. New PRD for Phase 2 (references Phase 1 output)
4. Complete workflow
5. Continue for each phase

### Pattern 3: Exploration First
1. Create exploratory PRD asking for analysis and recommendations
2. Review exploration report and plan
3. Create detailed PRD based on insights
4. Run full workflow with detailed PRD

---

## Appendix: File Structure Examples

### Example PRD Structure

For a complete PRD example, see **Example 1: Build a REST API** above.

A well-structured PRD should include:

**Required Sections:**
1. **Title** - Clear, descriptive name for the project
2. **Overview** - Brief description of what needs to be built (2-3 sentences)
3. **Objectives** - Bullet list of high-level goals
4. **Functional Requirements** - Specific features and behaviors
5. **Deliverable** - What the dev skill should produce

**Optional Sections:**
6. **Non-Functional Requirements** - Performance, security, scalability constraints
7. **Technical Constraints** - Technology stack, frameworks, dependencies
8. **Success Criteria** - How to measure completion

**PRD Writing Tips:**
- Be specific: "Support 1000 concurrent users" instead of "Handle many users"
- Be complete: Include all requirements upfront to avoid mid-workflow clarifications
- Provide context: Explain *why* something is needed, not just *what*
- Include examples: Show sample inputs/outputs or API responses if helpful

### Example state.json Structure

```json
{
  "session_id": "20240122_143052",
  "current_phase": "execute_draft",
  "current_step": 10,
  "prd_path": "api_requirements.md",
  "artifacts": {
    "prd": "api_requirements.md",
    "exploration_report": ".supervisor/output/exploration_report.md",
    "plan": ".supervisor/output/plan.md",
    "spec_schema": ".supervisor/output/spec_schema.md",
    "plan_critique": ".supervisor/output/plan_critique.md",
    "task_spec": ".supervisor/output/task_spec.md",
    "draft": ".supervisor/output/draft_api.js",
    "feedback": null,
    "final": null
  },
  "checkpoints": {
    "initial": 9,
    "draft": null
  },
  "completed_steps": [1, 2, 3, 4, 5, 6, 7, 8, 9],
  "status": "in_progress"
}
```

### Example session_log.json Structure

```json
{
  "sessions": [
    {
      "step": 3,
      "phase": "explore",
      "agent": "Explore",
      "output_path": ".supervisor/output/exploration_report.md",
      "status": "completed",
      "timestamp": "2024-01-22T14:31:15Z",
      "notes": "PRD analysis completed"
    },
    {
      "step": 6,
      "phase": "critique",
      "agent": "general-purpose",
      "output_path": ".supervisor/output/plan_critique.md",
      "status": "completed",
      "timestamp": "2024-01-22T14:35:42Z",
      "notes": "Plan critique completed, minor refinements suggested"
    },
    {
      "step": 8,
      "phase": "execute_spec",
      "agent": "analyst",
      "output_path": ".supervisor/output/task_spec.md",
      "status": "completed",
      "timestamp": "2024-01-22T14:38:20Z",
      "notes": "Specification conforms to schema"
    }
  ]
}
```

### Example spec_schema.md Structure

```markdown
# Task Specification Schema

This schema defines the required structure for task_spec.md.

## Required Sections

### 1. Requirements
**Functional Requirements**: List all functional requirements with acceptance criteria
**Non-Functional Requirements**: Performance, security, scalability requirements

### 2. Architecture
**System Overview**: High-level architecture description
**Components**: Breakdown of major components and their responsibilities
**Data Flow**: How data moves through the system

### 3. Data Models
**Entities**: Define all data entities with fields and types
**Relationships**: Entity relationships and cardinality
**Constraints**: Validation rules, unique constraints, required fields

### 4. API Contracts
**Endpoints**: List all API endpoints with methods
**Request Format**: Request body/query parameters for each endpoint
**Response Format**: Response structure and status codes
**Error Handling**: Error response format and codes

### 5. Implementation Guidelines
**Technology Stack**: Specific technologies to use
**Code Organization**: File structure and module organization
**Dependencies**: External libraries and versions

### 6. Acceptance Criteria
**Functional Tests**: What must work
**Performance Targets**: Response time, throughput requirements
**Security Requirements**: Authentication, authorization, data validation
```

---

This examples document is regularly updated. For the latest version, see the plugin repository.
