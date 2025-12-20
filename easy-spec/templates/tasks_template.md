# Tasks: [FEATURE NAME]

**Created**: [DATE]
**Status**: Draft
**Input**: Design documents from `/specs/[###-feature-name]/`
**Prerequisites**: spec.md, plan.md (required), research.md, data-model.md

> Template for creating technical implementation details for the functional requirements for use with Claude Code. Based on analysis of existing plans and Claude Code best practices.


## Execution Flow (main)
```
1. Load plan.md from feature directory
   → If not found: ERROR "No implementation plan found"
   → Extract: tech stack, libraries, structure
2. Load optional design documents:
   → data-model.md: Extract entities → model tasks
   → research_for_plan.md: Extract decisions → setup tasks
3. Generate tasks by category:
   → Setup: project init, dependencies, linting
   → Core: input and output models with schema, services, CLI commands
   → Integration: DB, middleware, logging
   → Polish: unit tests, performance, solution simplification, docs
4. Apply task rules:
   → Different files = mark [P] for parallel
   → Same file = sequential (no [P])
5. Number tasks sequentially (T001, T002...)
6. Generate dependency graph
7. Create parallel execution examples
8. Return: SUCCESS (tasks ready for execution)
```

---

## Format: `[ID] [P?] Description`
- **[P]**: Can run in parallel (different files, no dependencies)
- Include exact file paths in descriptions


## Tasks Breakdown

## Phase 3.1: Setup
- [ ] T001 Create project structure per implementation plan
- [ ] T002 Initialize [language] project with [framework] dependencies
- [ ] T003 [P] Configure linting and formatting tools

## Phase 3.3: Core Implementation
- [ ] T008 [P] User model in src/models/user.py
- [ ] T009 [P] UserService CRUD in src/services/user_service.py
- [ ] T010 [P] CLI --create-user in src/cli/user_commands.py
- [ ] T011 POST /api/users endpoint
- [ ] T012 GET /api/users/{id} endpoint
- [ ] T013 Input validation
- [ ] T014 Error handling and logging

## Phase 3.4: Integration
- [ ] T015 Connect UserService to DB
- [ ] T016 Auth middleware
- [ ] T017 Request/response logging
- [ ] T018 CORS and security headers

## Phase 3.5: Polish
- [ ] T019 [P] Unit tests for validation in tests/unit/test_validation.py
- [ ] T020 Performance tests (<200ms)
- [ ] T021 [P] Update docs/api.md
- [ ] T022 Remove duplication
- [ ] T023 Run manual-testing.md

## Dependencies
- Tests (T004-T007) before implementation (T008-T014)
- T008 blocks T009, T015
- T016 blocks T018
- Implementation before polish (T019-T023)

## Task Generation Rules
*Applied during main() execution*


1**From Data Model**:
    - Each entity → model creation task [P]
    - Relationships → service layer tasks

2. **From User Stories**:
    - Each story → integration test [P]
    - Test scenarios → validation tasks

3**Ordering**:
    - Setup → Models → Services → Integration → Polish
    - Dependencies block parallel execution


### Task N: [Task Name]
**Objective:** `[What this task accomplishes]`

**Requirements:**
- `[Specific requirement 1]`
- `[Specific requirement 2]`
- `[Integration points with existing systems]`

**Key Components:**
```language
# Laconic code examples or configuration snippets
# Show expected structure and patterns in very laconic form
# Use plain English descriptions that Claude can understand and act on
```

**Implementation Details:**
- `[Technical approach and methodology]`
- `[Dependencies and prerequisites]`
- `[Error handling considerations]`
- `[Testing approach]`

**Files to Create/Modify:**
- ✅ `path/to/file.ext` - Description of file purpose
- ⏳ `path/to/other.ext` - Description of what needs implementation

---

## Implementation Status

### Task N: [Task Name] `[STATUS: ✅ COMPLETED | ⏳ PENDING | 🔄 IN PROGRESS]`
**Progress Summary:**
- ✅ `[Completed milestone 1]`
- ✅ `[Completed milestone 2]`  
- ⏳ `[Pending work item]`

**Implementation Notes:**
- `[Key decisions made during implementation]`
- `[Challenges encountered and solutions]`
- `[Performance metrics or test results]`

---

## Configuration & Setup

**Project Structure:**
```
project-root/
├── config-files
├── source-directories/
│   ├── module1/
│   └── module2/
└── documentation/
```

**Dependencies:**
```toml
# Package configuration examples
[dependencies]
key-package = "version"
```

**Environment Setup:**
```bash
# Commands to set up the project
command-to-install
command-to-configure
```

---

## Implementation Notes

### Current Status
**Working Features:**
- `[List of completed and tested features]`

**Known Issues:**
- `[Any current limitations or bugs]`

**Next Steps:**
- `[Immediate follow-up work needed]`

---


## Template Usage Notes

**For Claude Code Users:**
1. **Clear Requirements**: Specify exactly what needs to be built with concrete examples
2. **Modular Tasks**: Break complex work into discrete, testable components  
3. **Status Tracking**: Use checkboxes and status indicators to track progress
4. **Code Examples**: Include expected laconic patterns and structures to guide implementation


**Best Practices:**
- Use plain laconic English descriptions that Claude can understand and act on
- Include specific file paths, commands, and configuration examples
- Track implementation status with visual indicators (✅⏳🔄)
- Reference existing patterns from similar projects when available
