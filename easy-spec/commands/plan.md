Generate technical implementation plan (plan.md) from functional requirements specification.

## Process

1. **FIRST**: Check if the user already provided folder path in their message
   - Look for absolute paths like /Users/... or relative paths like ai_tasks/...
   - Only prompt if folder path is missing
2. Read the spec.md from the specified feature folder (REQUIRED)
3. Read research.md if available for technical decisions
4. Use the embedded implementation plan template structure provided below
5. Extract functional requirements and translate to technical approach
6. Define technical context:
   - Language/version and dependencies
   - Storage and testing framework
   - Target platform and performance goals
   - Project structure and architecture
7. Design implementation phases:
   - Phase 0: Research & outline (resolve technical unknowns)
   - Phase 1: Design & contracts (data models, APIs, tests)
   - Phase 2: Task planning approach (execution strategy)
   - Phase 3+: Implementation roadmap
8. Define source code structure and file organization
9. Create success criteria and integration points
10. Output plan.md to the feature folder

## Key Principles

**Focus on HOW (technical implementation):**
- ✅ "Use @mapbox/maps-snapshotter library for rendering"
- ✅ "Node.js 20+ with TypeScript 5.7+ strict mode"
- ✅ "Commander.js for CLI interface"
- ❌ "System must render images" (that's spec, not plan)

**Translate requirements to architecture:**
- Functional requirements → Technical components
- User scenarios → Integration tests
- Data requirements → Data models and schemas
- Performance targets → Concrete metrics

**Be specific about technology:**
- Exact versions of languages and frameworks
- Specific libraries and tools
- File structure and organization
- Testing strategy and tools

## Plan Structure

### 1. Summary
Extract from spec.md:
- Primary requirement (WHAT from spec)
- Technical approach (HOW to implement)
- Key outcomes expected

### 2. Technical Context
Define technology stack:
```
**Language/Version**: Node.js 20+, TypeScript 5.7+
**Primary Dependencies**: @mapbox/maps-snapshotter 11.15.1, commander.js
**Storage**: File system (GeoJSON input, PNG output)
**Testing**: Vitest for unit/integration tests
**Target Platform**: macOS, Linux
**Performance Goals**: <500ms for small files, <2s for medium
**Constraints**: ~1500MB memory per instance
**Scale/Scope**: Single CLI tool, batch processing support
```

### 3. Documentation Structure
```
ai_tasks/[project]/[feature]/
├── plan.md          # This file
├── research.md      # Phase 0 technical research
├── spec.md          # Input (functional requirements)
└── tasks.md         # Phase 2 output (from /tasks command)
```

### 4. Source Structure
Define project layout:
```
project-root/
├── bin/              # CLI entry point
├── src/
│   ├── service/      # Business logic
│   ├── config/       # Configuration
│   └── types.d.ts    # Type definitions
├── config/           # Config files, templates
├── tests/
│   ├── unit/         # Unit tests
│   └── integration/  # Integration tests
└── package.json
```

### 5. Implementation Phases

**Phase 0: Research & Outline**
- Identify technical unknowns from spec
- Research best practices for chosen tech stack
- Validate technology choices
- Resolve NEEDS CLARIFICATION items
- Output: research.md

**Phase 1: Design & Contracts**
- Extract data models from functional requirements
- Define input/output schemas (use JSON Schema)
- Create test scenarios from user stories
- Define API contracts/interfaces
- Output: data-model.md, tests.md (optional)

**Phase 2: Task Planning Approach**
- Strategy for generating tasks.md
- Task ordering (setup → config → core → integration → polish)
- Identify parallel execution opportunities
- Estimate task count (15-30 typical)
- **NOTE**: Executed by /tasks command, not /plan

**Phase 3+: Implementation Roadmap**
- Brief outline of execution and validation phases
- Integration approach
- Deployment considerations

### 6. Configuration & Setup
- Package dependencies with versions
- Environment setup commands
- Development workflow
- Build and deployment process

### 7. Success Criteria
Organize by category:
- ✅ Core Functionality (features working)
- ✅ Quality & Reliability (tests passing)
- ✅ Operations (deployment ready)

### 8. Integration Points
- How feature connects to existing systems
- External APIs and services used
- Data flow and dependencies

## Translation from Spec to Plan

**Functional Requirements → Technical Implementation:**

| Spec (WHAT) | Plan (HOW) |
|-------------|------------|
| FR-001: Accept GeoJSON files | Use fs.readFileSync, validate with JSON.parse |
| FR-009: Calculate bounding box | Implement recursive coordinate extraction |
| FR-020: Use GL Native renderer | @mapbox/maps-snapshotter with setStyleURI |
| FR-026: Support token resolution | CLI flag → env var → AWS Secrets (priority) |

**User Scenarios → Test Structure:**

| Scenario | Test Implementation |
|----------|-------------------|
| Render Point with minimal style | Integration test with fixture point.geojson |
| Handle invalid GeoJSON | Unit test expecting InvalidGeoJSONError |
| Performance < 500ms | Performance test with timing assertions |

## Technical Context Guidelines

Fill each field based on requirements:

- **Language/Version**: Specific version (e.g., "Python 3.11+", "Node.js 20+")
- **Primary Dependencies**: Core libraries with versions
- **Storage**: Database, filesystem, or N/A
- **Testing**: Test framework and approach
- **Target Platform**: OS, runtime environment
- **Performance Goals**: Concrete metrics from spec
- **Constraints**: Memory, latency, size limits
- **Scale/Scope**: Users, data size, request volume

Mark "NEEDS CLARIFICATION" if research is needed, then resolve in Phase 0.

## Research Phase (Phase 0)

If Technical Context has NEEDS CLARIFICATION items:

1. **Identify unknowns**:
   - Technology choices not yet made
   - Best practices to research
   - Integration patterns to investigate

2. **Research each unknown**:
   - What are the options?
   - What are the tradeoffs?
   - What's the recommendation?

3. **Document in research.md**:
   ```
   **Decision**: Chose [technology/approach]
   **Rationale**: [Why this choice]
   **Alternatives**: [What else was considered]
   ```

4. **Update Technical Context**: Replace NEEDS CLARIFICATION with decisions

## Design Phase (Phase 1)

**Data Models**:
- Extract entities from spec requirements
- Define schemas (prefer JSON Schema)
- Separate input, output, intermediate models
- Include validation rules

**Test Scenarios**:
- Map user stories to integration tests
- Define test fixtures needed
- Outline expected behaviors
- Create quickstart validation steps

## Arguments Expected

When using this command, provide:
1. Feature folder path (e.g., `ai_tasks/another-big-project/ml-automation/feature-snapshotter/`)
2. (Optional) Custom instructions or technical focus areas

Example usage:
```
/plan ai_tasks/another-big-project/ml-automation/feature-snapshotter/
```

With custom prompt:
```
/plan ai_tasks/data-pipeline/etl-process/ Focus on scalability and error recovery patterns
```

## Success Criteria

Generated plan.md should:
- Start from spec.md functional requirements
- Define complete technical stack with versions
- Include detailed project structure
- Specify all major dependencies
- Define clear implementation phases
- Map spec requirements to technical components
- Include concrete success criteria
- Be actionable and specific (no ambiguity)
- Follow the template structure
- Use laconic, technical language

## Common Pitfalls to Avoid

❌ **Don't include**:
- Business justifications (that's in spec)
- Functional requirements lists (already in spec)
- User scenarios verbatim (summarize technical approach)
- Vague technology choices ("use a database" → specify PostgreSQL 15+)

✅ **Do include**:
- Specific technology versions
- File paths and project structure
- Implementation patterns and practices
- Technical design decisions with rationale
- Concrete success criteria

**Now generate the plan.md file following the embedded template structure below, translating functional requirements from spec.md into technical implementation details.**

---

## Embedded Implementation Plan Template

Use this template structure when generating plan.md files:

```markdown
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
```
