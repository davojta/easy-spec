Generate technical implementation plan (plan.md) from functional requirements specification.

## Process

1. **FIRST**: Check if the user already provided folder path in their message
   - Look for absolute paths like /Users/... or relative paths like ai_tasks/...
   - Only prompt if folder path is missing
2. Read the spec.md from the specified feature folder (REQUIRED)
3. Read research.md if available for technical decisions
4. Find and read the implementation plan template:
   - Use Glob to find: `easy-spec/templates/implementation-plan_template.md`
   - This will locate the template in the installed plugin directory
   - Read it to understand the structure you should follow
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

**Now generate the plan.md file following the template structure, translating functional requirements from spec.md into technical implementation details.**
