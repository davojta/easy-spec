# Implementation Plan Template for Claude Code

**Created**: [DATE]
**Status**: Draft
**Input**: specification from *spec.md

> Template for creating technical implementation details and plan for the functional requirements for use with Claude Code. Based on analysis of existing plans and Claude Code best practices.

## Project Overview

### Project Name
`[Brief, descriptive name of the project/feature]`

### Problem Statement
`[Clear laconic description of the problem to be solved or feature to be implemented]`

### Solution Summary
`[High-level laconic approach and key outcomes expected]`

---
## Template Requirements
### Execution Flow (main)
```
1. Parse user description from Input
   → If empty: ERROR "No feature description provided"
2. Extract key concepts from description
   → Identify: actors, actions, data, constraints
3. For each unclear aspect:
   → Mark with [NEEDS CLARIFICATION: specific question]
4. Fill User Scenarios & Testing section
   → If no clear user flow: ERROR "Cannot determine user scenarios"
5. Generate Functional Requirements
   → Each requirement must be testable
   → Mark ambiguous requirements
6. Identify Key Entities (if data involved)
7. Run Review Checklist
   → If any [NEEDS CLARIFICATION]: WARN "Spec has uncertainties"
   → If implementation details found: ERROR "Remove tech details"
8. Return: SUCCESS (spec ready for planning)
```
## Summary
[Extract from feature spec: primary requirement + technical approach from research]

## Technical Implementation 
### Technical Context
**Language/Version**: [e.g., Python 3.11, Swift 5.9, Rust 1.75 or NEEDS CLARIFICATION]  
**Primary Dependencies**: [e.g., FastAPI, UIKit, LLVM or NEEDS CLARIFICATION]  
**Storage**: [if applicable, e.g., PostgreSQL, CoreData, files or N/A]  
**Testing**: [e.g., pytest, XCTest, cargo test or NEEDS CLARIFICATION]  
**Target Platform**: [e.g., Linux server, iOS 15+, WASM or NEEDS CLARIFICATION]
**Project Type**: [single/web/mobile - determines source structure]  
**Performance Goals**: [domain-specific, e.g., 1000 req/s, 10k lines/sec, 60 fps or NEEDS CLARIFICATION]  
**Constraints**: [domain-specific, e.g., <200ms p95, <100MB memory, offline-capable or NEEDS CLARIFICATION]  
**Scale/Scope**: [domain-specific, e.g., 10k users, 1M LOC, 50 screens or NEEDS CLARIFICATION]

### Documentation (this feature)
```
ai_tasks/[###project - one of pipeline, qa-tool, jira, icons-pipeline, routines, another-big-project]/[###-feature]/
├── plan.md              # This file (/plan command output)
├── research.md          # Phase 0 output (/plan command)
├── quickstart.md        # Phase 1 output (/plan command)
└── tasks.md             # Phase 2 output (/tasks command - NOT created by /plan)
```

### Source Code (repository root)
```
src/
├── services/
├── bin/
└── utils/

tests/
└── unit/
```

## Phase 0: Outline & Research
1. **Extract unknowns from Technical Context** above:
    - For each NEEDS CLARIFICATION → research task
    - For each dependency → best practices task
    - For each integration → patterns task

2. **Generate and dispatch research agents**:
   ```
   For each unknown in Technical Context:
     Task: "Research {unknown} for {feature context}"
   For each technology choice:
     Task: "Find best practices for {tech} in {domain}"
   ```

3. **Consolidate findings** in `research_for_plan.md` using format:
    - Decision: [what was chosen]
    - Rationale: [why chosen]
    - Alternatives considered: [what else evaluated]

**Output**: research.md with all NEEDS CLARIFICATION resolved

## Phase 1: Design & Contracts
*Prerequisites: research.md complete*

1. **Extract entities from feature spec** → `data-model.md`:
    - Entity name, fields, relationships
    - Validation rules from requirements
    - State transitions if applicable
    - Make separation for input and output models, outline the intermidiate models if needed
    - Use JSON Schema for validation

2**Extract test scenarios** from user stories → `tests.md`:
    - Each story → integration test scenario
    - Quickstart test = story validation steps

**Output**: data-model.md, tests.md

## Phase 2: Task Planning Approach

**Task Generation Strategy**:
- Load `.specify/templates/tasks-template.md` as base
- Generate tasks from Phase 1 design docs (contracts, data model, quickstart)
- Each contract → contract test task [P]
- Each entity → model creation task [P]
- Each user story → integration test task
- Implementation tasks to make tests pass

**Ordering Strategy**:
- TDD order: Tests before implementation
- Dependency order: Models before services before UI
- Mark [P] for parallel execution (independent files)

**Estimated Output**: 25-30 numbered, ordered tasks in tasks.md

**IMPORTANT**: This phase is executed by the /tasks command, NOT by /plan

## Phase 3+: Future Implementation
*These phases are beyond the scope of the /plan command*

**Phase 3**: Task execution (/tasks command creates tasks.md)  
**Phase 4**: Implementation (execute tasks.md following constitutional principles)  
**Phase 5**: Validation (run tests, execute quickstart.md, performance validation)

## Complexity Tracking
*Fill ONLY if Constitution Check has violations that must be justified*

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| [e.g., 4th project] | [current need] | [why 3 projects insufficient] |
| [e.g., Repository pattern] | [specific problem] | [why direct DB access insufficient] |


## Tasks Breakdown

### Task N: [Task Name]
**Objective:** `[What this task accomplishes]`

**Requirements:**
- `[Specific requirement 1]`
- `[Specific requirement 2]`
- `[Integration points with existing systems]`
- `[Performance/quality criteria]`

**Key Components:**
```language
# Very Laconic pseudo code (or real code) examples or configuration snippets
# Briefly show expected structure and patterns in very laconic form
# Use plain English descriptions that Claude can understand and act on
```

**Files to Create/Modify:**``
- ✅ `path/to/file.ext` - Description of file purpose
- ⏳ `path/to/other.ext` - Description of what needs implementation
``
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

## Integration Points
- `[How this integrates with existing systems]`
- `[APIs or services used]`
- `[Data flow and dependencies]`
- `[Configuration management approach]`

## Success Criteria

### ✅ Core Functionality
- `[Primary feature working correctly]`
- `[Error handling implemented]`
- `[Performance meets requirements]`

### ✅ Quality & Reliability
- `[Tests passing]`
- `[Documentation complete]`
- `[Code review requirements met]`

### ✅ Deployment & Operations
- `[Configuration validated]`
- `[Monitoring and logging in place]`
- `[Rollback procedures documented]`

---

## Example Usage

```bash
# Basic usage examples
command --option value

# Advanced usage scenarios
command --complex-option --with-parameters
```

**Expected Output:**
```
Example output showing what success looks like
```

## Additional Context

### Design Decisions

### Best Practices Applied
- `[Coding standards followed]`
- `[Security considerations addressed]`
- `[Performance optimizations implemented]`

### Reference Documentation
- `[Links to relevant documentation]`
- `[Related projects or examples]`
- `[Technical specifications]`

### Performance Considerations

### Known Limitations

---

## Template Usage Notes

**For Claude Code Users:**
1. **Clear Requirements**: Specify exactly what needs to be built with concrete examples
2. **Modular Tasks**: Break complex work into discrete, testable components
4. **Code Examples**: Include expected more laconic patterns and structures to guide implementation
5. **Success Criteria**: Define clear, measurable outcomes for each task

**Best Practices:**
- Use plain laconic English descriptions that Claude can understand and act on
- Include specific file paths, commands, and configuration examples
- Document integration points and dependencies clearly
- Provide example usage and expected outputs
- Reference existing patterns from similar projects when available
