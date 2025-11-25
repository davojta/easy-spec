Execute tasks from tasks.md in a feature folder.

## Process

1. Read tasks.md from the specified feature folder (REQUIRED)
2. Parse task list and identify:
   - Task IDs (T001, T002, etc.)
   - Task descriptions and file paths
   - Task status (pending, in-progress, completed)
   - Parallel execution markers [P]
   - Dependencies between tasks
3. Read supporting documents if available:
   - spec.md (functional requirements)
   - plan.md (technical approach)
   - research.md (technical decisions)
   - data-model.md (data structures)
4. Read detailed task specifications from tasks.md:
   - Objective and requirements
   - Key components and code examples
   - Implementation details
   - Files to create/modify
5. Execute tasks following the workflow:
   - Start with pending tasks that have no blockers
   - Follow dependency order (setup → core → integration → polish)
   - Run tasks marked [P] in parallel when possible
   - Update task status in tasks.md as you progress:
     - Mark task as in-progress when starting
     - Mark task as completed when done
     - Add implementation notes and progress summary
6. After each task completion:
   - Run code quality checks (lint, typecheck, format)
   - Run tests if available
   - Update Implementation Status section in tasks.md
7. Continue until all pending tasks are completed or blocked

## Task Execution Rules

**Status Updates:**
- Mark ONE task as in-progress before starting work
- Mark task as completed IMMEDIATELY after finishing
- Add implementation notes with key decisions and challenges
- Update progress summary with completed milestones

**Quality Checks:**
- Run `make lint` after each task
- Run `make typecheck` after each task
- Run `make format` to auto-fix issues
- Run tests: `make test` or `uv run pytest`
- All checks must pass before marking task as completed

**Dependency Management:**
- Respect task dependencies (don't start T005 if T004 blocks it)
- Tasks marked [P] can run in parallel (different files)
- Sequential tasks must complete in order

**Error Handling:**
- If task fails or is blocked, keep status as in-progress
- Create new task describing what needs resolution
- Document blockers in implementation notes
- Ask user for clarification if needed

## Arguments Expected

When using this command, provide:
1. Feature folder path (e.g., `ai_tasks/another-big-project/ml-automation/features/download-geojsons/`)
2. (Optional) Custom instructions or focus areas
3. (Optional) Specific task ID to execute (e.g., `T005`)

Example usage in chat:
```
/execute ai_tasks/another-big-project/ml-automation/features/download-geojsons/
```

With custom prompt:
```
/execute ai_tasks/another-big-project/ml-automation/features/download-geojsons/ Focus on error handling and edge cases
```

Execute specific task:
```
/execute ai_tasks/another-big-project/ml-automation/features/download-geojsons/ T005
```

## Success Criteria

Successful execution should:
- Load tasks.md and parse all tasks correctly
- Identify task dependencies and execution order
- Execute tasks following dependency order
- Update task status in real-time (in-progress → completed)
- Run quality checks after each task (lint, typecheck, format)
- Run tests to verify implementation
- Update Implementation Status section with progress
- Add implementation notes with key decisions
- Complete all pending tasks or identify blockers
- Provide clear summary of completed work

## Common Pitfalls to Avoid

❌ **Don't**:
- Start multiple tasks simultaneously (only ONE in-progress at a time)
- Mark tasks as completed before quality checks pass
- Skip dependencies (respect task ordering)
- Forget to update Implementation Status section
- Ignore test failures or lint errors
- Batch multiple task completions

✅ **Do**:
- Mark task in-progress BEFORE starting work
- Run quality checks after each task
- Update status IMMEDIATELY after completion
- Document implementation decisions
- Respect dependency order
- Ask for clarification when blocked
- Follow code quality standards

## Task Status Format in tasks.md

When updating tasks in tasks.md:

**Checklist format:**
```markdown
- [x] T001 [P] Create project structure ✅
- [ ] T002 Initialize dependencies
```

**Detailed status format:**
```markdown
### Task T001: Create Project Structure `✅ COMPLETED`
**Progress Summary:**
- ✅ Created directory structure
- ✅ Added package.json with dependencies
- ✅ Configured TypeScript

**Implementation Notes:**
- Used recommended tsconfig.json settings
- Added strict type checking
- Configured ESLint for code quality
```

**Now read the tasks.md file from the specified feature folder and begin executing tasks following the workflow above.**
