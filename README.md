# Custom Claude Code Commands

This directory contains custom slash commands for automating common workflows.

## Available Commands

### `/research` - Generate Research Document

Automatically generates a `research.md` file by investigating existing patterns and technical approaches in the codebase.

**Purpose**: Find and document existing implementations, analyze options, and provide implementation guidance based on verified code patterns.

**Usage**:

```bash
# Basic usage
/research ai_tasks/another-big-project/ml-automation/new-feature/

# With problem statement
/research ai_tasks/another-big-project/ml-automation/s3-downloads/ Investigate concurrent S3 download patterns

# With project focus
/research ai_tasks/pipeline/icon-processing/ Focus on pipeline landmark icons patterns
```

**What it does**:

1. Reads the user's problem statement
2. Uses `docs/templates/research_template.md` as structure guidance
3. Searches for existing patterns in relevant projects:
   - pipeline (pipeline)
   - qa-tool (QA tools)
   - another-big-project-frontend
   - data-pipeline-exports
   - map-pipeline
4. Documents findings with actual file paths and code
5. Analyzes options with decision matrix
6. Provides implementation guidance
7. Outputs `research.md` in the feature folder

**Requirements**:

- Template file must exist at `docs/templates/research_template.md`

**Key Principles**:

- ✅ Reference **actual code** with full file paths
- ✅ Include **verified findings** (not assumptions)
- ✅ Show **laconic code examples** from existing implementations
- ✅ Provide **decision rationale** with trade-offs
- ❌ Don't invent solutions without checking existing code

**Example Output**:

```markdown
## Existing Patterns

### Reference Implementation

**Location**: `/Users/dzianissheka/projects/dev/work/pipeline/src/service/s3-download.ts`
**Project**: pipeline
**Pattern**: Concurrent S3 downloads with parallel-transform

\`\`\`typescript
// Actual code from the project
const downloader = new S3Downloader({ concurrency: 10 });
await downloader.downloadFiles(urls, outputDir);
\`\`\`

## Decision Matrix

| Approach | Pros | Cons | Recommendation |
|----------|------|------|----------------|
| ThreadPoolExecutor | Python stdlib | Limited to I/O | ✅ Recommended |
| asyncio | Non-blocking | More complex | Consider if needed |
```

---

### `/spec` - Generate Functional Requirements Specification

Automatically generates a `spec.md` file from research documents, focusing on functional requirements (WHAT users need, not HOW to implement).

**Purpose**: Convert research findings into clear, testable functional requirements for business stakeholders and developers.

**Usage**:

```bash
# Basic usage
/spec ai_tasks/another-big-project/ml-automation/feature-snapshotter/

# With custom instructions
/spec ai_tasks/data-pipeline/etl-process/ Focus on data validation and error recovery requirements
```

**What it does**:

1. Reads research documents from the feature folder (`research.md`, `*research*.md`)
2. Uses `docs/templates/spec_template.md` as the structure template
3. Extracts key information:
   - Problem statement and solution approach
   - User needs and scenarios
   - Input/output data requirements
   - Business logic and data transformations
4. Generates numbered functional requirements (FR-001, FR-002, ...)
5. Creates acceptance scenarios in Given-When-Then format
6. Identifies edge cases and error scenarios
7. Outputs `spec.md` in the feature folder

**Requirements**:

- Feature folder should contain research documents
- Template file must exist at `docs/templates/spec_template.md`

**Key Principles**:

- ✅ Focus on **WHAT** users need (functional requirements)
- ❌ Avoid **HOW** to implement (no tech stack, code, architecture)
- 👥 Written for business stakeholders, not just developers
- 📝 Use laconic, testable language

**Example Output**:

```markdown
## Functional Requirements

### Input Data
- **FR-001**: System MUST accept GeoJSON files in RFC 7946 format
- **FR-002**: System MUST support Point, LineString, Polygon geometries

### Output Data
- **FR-010**: System MUST generate PNG images with configurable dimensions
- **FR-011**: System MUST name output files matching input basename

### Business Logic
- **FR-020**: System MUST calculate bounding box from all coordinates
- **FR-021**: System MUST calculate optimal zoom level to fit features
```

---

### `/plan` - Generate Technical Implementation Plan

Automatically generates a `plan.md` file from functional requirements (spec.md), defining the technical architecture and implementation approach.

**Purpose**: Convert functional requirements (WHAT) into technical implementation details (HOW) with specific technology choices, architecture, and project structure.

**Usage**:

```bash
# Basic usage
/plan ai_tasks/another-big-project/ml-automation/feature-snapshotter/

# With custom instructions
/plan ai_tasks/data-pipeline/etl-process/ Focus on scalability and error recovery patterns
```

**What it does**:

1. Reads `spec.md` from the feature folder (required)
2. Reads `research.md` if available for technical decisions
3. Uses `docs/templates/implementation-plan_template.md` as the structure template
4. Translates functional requirements to technical components
5. Defines complete technical context:
   - Language/version and dependencies
   - Storage, testing framework, target platform
   - Performance goals and constraints
   - Project structure and file organization
6. Outlines implementation phases:
   - Phase 0: Research & outline
   - Phase 1: Design & contracts (data models, tests)
   - Phase 2: Task planning approach
   - Phase 3+: Implementation roadmap
7. Creates success criteria and integration points
8. Outputs detailed `plan.md` in the feature folder

**Requirements**:

- Feature folder must contain `spec.md`
- Template file must exist at `docs/templates/implementation-plan_template.md`

**Key Principles**:

- ✅ Focus on **HOW** to implement (technical details)
- ✅ Specific technology versions and tools
- ✅ Concrete project structure and file paths
- ✅ Translate spec requirements to architecture
- ❌ Avoid repeating functional requirements (reference spec.md)

**Translation Examples**:

| Spec (WHAT) | Plan (HOW) |
|-------------|------------|
| Accept GeoJSON files | Use Node.js fs.readFileSync, validate with JSON.parse |
| Calculate bounding box | Recursive coordinate extraction function |
| Render images | @mapbox/maps-snapshotter 11.15.1 library |

**Example Output**:

```markdown
## Technical Context

**Language/Version**: Node.js 20+, TypeScript 5.7+
**Primary Dependencies**: @mapbox/maps-snapshotter 11.15.1, commander.js
**Storage**: File system (GeoJSON input, PNG output)
**Testing**: Vitest for unit/integration tests
**Target Platform**: macOS, Linux
**Performance Goals**: <500ms for <100KB files

## Source Structure

\`\`\`
feature-snapshotter/
├── bin/snapshotter.ts       # CLI entry
├── src/
│   ├── service/             # Business logic
│   └── config/              # Configuration
└── tests/                   # Unit & integration
\`\`\`
```

---

### `/tasks` - Generate Implementation Tasks

Automatically generates a detailed `tasks.md` file from a feature's `plan.md` and other design documents.

**Purpose**: Convert high-level implementation plans into actionable, numbered tasks with dependencies and parallel execution opportunities.

**Usage**:

```bash
# Basic usage
/tasks ai_tasks/another-big-project/ml-automation/feature-snapshotter/

# With custom instructions
/tasks ai_tasks/another-big-project/ml-automation/feature-name/ Focus on TypeScript strict mode and error handling best practices
```

**What it does**:

1. Reads `plan.md` from the feature folder (required)
2. Reads `spec.md`, `research.md`, `data-model.md` if available
3. Uses `docs/templates/tasks_template.md` as the structure template
4. Generates numbered tasks (T001-T0XX) organized by phase:
   - Setup (project init, dependencies)
   - Configuration & Templates
   - Core Implementation (business logic)
   - Integration (external systems)
   - Polish (tests, docs)
5. Marks tasks with `[P]` that can run in parallel
6. Creates dependency graph
7. Outputs detailed `tasks.md` in the feature folder

**Requirements**:

- Feature folder must contain `plan.md`
- Template file must exist at `docs/templates/tasks_template.md`

**Output Structure**:

Each task includes:
- Numbered ID (T001, T002, ...)
- [P] marker for parallel execution
- Detailed objective and requirements
- Code examples showing expected patterns
- List of files to create/modify
- Implementation guidance

**Example Output**:

```markdown
## Phase 3.1: Setup

- [ ] T001 Create project structure per implementation plan
- [ ] T002 Initialize Node.js project with dependencies
- [ ] T003 [P] Configure TypeScript with strict mode
- [ ] T004 [P] Configure linting and formatting tools

### Task T001: Create Project Structure
**Objective:** Initialize directory structure per plan.md design

**Requirements:**
- Create `src/service/` for business logic
- Create `config/` for configuration files
...
```

**Tips**:

- Run `/tasks` after completing the plan.md document
- Review generated tasks.md and adjust if needed
- Use parallel-marked tasks ([P]) to speed up implementation
- Execute tasks in order respecting dependencies

## Adding New Commands

To create a new command:

1. Create a `.md` file in `.claude/commands/`
2. Filename becomes the command (e.g., `deploy.md` → `/deploy`)
3. File content is the prompt that Claude will execute
4. Document usage in this README

## Complete Workflow Examples

### 1. Full Feature Development Workflow

```bash
# Phase 0: Investigation → Research
# Start by investigating existing patterns
/research ai_tasks/another-big-project/ml-automation/new-feature/ Focus on similar implementations in pipeline
# Review research.md - existing patterns, technical findings

# Phase 1: Research → Functional Spec
# Convert findings into functional requirements
/spec ai_tasks/another-big-project/ml-automation/new-feature/
# Review spec.md - focuses on WHAT users need

# Phase 2: Spec → Technical Plan
# Convert functional requirements to technical implementation
/plan ai_tasks/another-big-project/ml-automation/new-feature/
# Review plan.md - defines HOW to implement with tech stack

# Phase 3: Plan → Implementation Tasks
# Break down technical plan into numbered, actionable tasks
/tasks ai_tasks/another-big-project/ml-automation/new-feature/
# Review tasks.md - 15-30 tasks with dependencies

# Phase 4: Execute Tasks
# Start implementing following task order
Let's start with Phase 3.1 Setup tasks (T001-T005)
```

**Document Flow**:
```
Initial Problem/Question
    ↓
research.md (/research) ← Existing patterns & analysis
    ↓
spec.md (/spec) ← WHAT users need (functional requirements)
    ↓
plan.md (/plan) ← HOW to implement (technical approach)
    ↓
tasks.md (/tasks) ← Actionable steps (T001-T0XX)
    ↓
Implementation
```

### 2. Start with Research for New Feature

```bash
# Investigate patterns before writing spec
/research ai_tasks/another-big-project/ml-automation/geojson-rendering/

# With specific focus
/research ai_tasks/pipeline/s3-downloads/ Look for concurrent download patterns in pipeline and qa-tool

# Then generate spec from findings
/spec ai_tasks/another-big-project/ml-automation/geojson-rendering/
```

### 3. Generate Spec with Custom Focus

```bash
# For data pipeline with emphasis on validation
/spec ai_tasks/data-pipeline/etl-process/ Focus on data validation, error recovery, and retry logic

# For API service with security focus
/spec api/user-service/ Emphasize authentication, authorization, and data privacy requirements
```

### 4. Generate Tasks with Custom Instructions

```bash
# TypeScript project with strict mode
/tasks ai_tasks/another-big-project/ml-automation/feature-name/ Focus on TypeScript strict mode and comprehensive error handling

# Python project with performance focus
/tasks ai_tasks/ml-pipeline/model-training/ Emphasize performance optimization and parallel processing
```

### 5. Regenerate Documentation

```bash
# Regenerate research after new findings
/research ai_tasks/another-big-project/ml-automation/feature-snapshotter/ Updated investigation of snapshotter patterns

# Regenerate spec after research updates
/spec ai_tasks/another-big-project/ml-automation/feature-snapshotter/

# Regenerate plan after spec changes
/plan ai_tasks/another-big-project/ml-automation/feature-snapshotter/

# Regenerate tasks after plan changes
/tasks ai_tasks/another-big-project/ml-automation/feature-snapshotter/
```

### 6. Skip Steps for Existing Features

```bash
# If spec.md already exists, start from plan
/plan ai_tasks/existing-feature/

# If plan.md already exists, generate tasks
/tasks ai_tasks/existing-feature/

# If all docs exist, start implementing directly
Let's implement Task T001: Create project structure
```

---

**Note**: Commands are project-specific and stored in version control for team sharing.
