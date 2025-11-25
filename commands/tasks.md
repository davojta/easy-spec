Generate a comprehensive tasks.md file from an implementation plan.md using the tasks_template.md structure.

## Process

1. **FIRST**: Check if the user already provided folder path in their message
   - Look for absolute paths like /Users/... or relative paths like ai_tasks/...
   - Only prompt if folder path is missing
2. Read the plan.md from the specified feature folder
3. Read spec.md, research.md, data-model.md if available
4. Find and read the tasks template:
   - Use Glob to find: `easy-spec/templates/tasks_template.md`
   - This will locate the template in the installed plugin directory
   - Read it to understand the structure you should follow
5. Extract tech stack, project structure, and implementation details
6. Generate numbered tasks (T001-T0XX) organized by phase:
   - Phase 3.1: Setup (project init, dependencies, config)
   - Phase 3.2: Configuration & Templates (if applicable)
   - Phase 3.3: Core Implementation (services, models, business logic)
   - Phase 3.4: Integration (external systems, middleware, error handling)
   - Phase 3.5: Polish (tests, docs, performance)
7. Mark [P] for parallel-safe tasks (different files, no dependencies)
8. Create dependency graph showing task relationships
9. Write detailed task specifications with objectives, requirements, code examples
10. Output tasks.md to the feature folder

## Task Format Requirements

Each task must include:
- Numbered ID (T001, T002, etc.)
- [P] marker if can run in parallel
- Clear description with exact file paths
- Detailed specification section:
  - Objective
  - Requirements
  - Key Components (with code examples)
  - Implementation Details
  - Files to Create/Modify

## Arguments Expected

When using this command, provide:
1. Feature folder path (e.g., `ai_tasks/another-big-project/ml-automation/feature-snapshotter/`)
2. (Optional) Custom instructions or focus areas

Example usage in chat:
```
Use the /tasks command to generate tasks for ai_tasks/another-big-project/ml-automation/feature-snapshotter/
```

Or with custom prompt:
```
Use the /tasks command for ai_tasks/another-big-project/ml-automation/feature-name/ with focus on TypeScript best practices
```
