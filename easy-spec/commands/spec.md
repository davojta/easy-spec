Generate functional requirements specification (spec.md) from research documents.

## Process

1. **FIRST**: Check if the user already provided folder path in their message
   - Look for absolute paths like /Users/... or relative paths like ai_tasks/...
   - Only prompt if folder path is missing
2. Read research documents from the specified feature folder:
   - `research.md` or `*research*.md` files (primary source)
   - Any other analysis or investigation documents
3. Use the embedded spec template structure provided below
4. Extract key information:
   - Problem statement and solution approach
   - User needs and scenarios
   - Input/output data requirements
   - Business logic and data transformations
   - Edge cases and error scenarios
5. Generate spec.md focused on WHAT (functional requirements), not HOW (implementation)
6. Create numbered functional requirements (FR-001, FR-002, ...)
7. Write acceptance scenarios in Given-When-Then format
8. Output spec.md to the feature folder

## Key Principles

**Focus on WHAT, not HOW:**
- ✅ "System MUST accept GeoJSON files in RFC 7946 format"
- ❌ "Use @mapbox/maps-snapshotter library to render"

**Write for business stakeholders:**
- Use plain, laconic English
- Describe user needs and behaviors
- Avoid technical implementation details
- No code, APIs, or architecture

**Be specific and testable:**
- Each requirement must be verifiable
- Use precise language (MUST, SHOULD, MAY)
- Include measurable criteria where applicable
- Mark ambiguities with [NEEDS CLARIFICATION: question]

## Requirement Categories

Organize requirements by:

1. **Input Data**: What data does the system accept?
   - File formats, data structures
   - Validation rules
   - Size/scale requirements

2. **Output Data**: What does the system produce?
   - Output formats and naming
   - Quality requirements
   - Success criteria

3. **Business Logic**: How is data transformed?
   - Processing rules
   - Calculations and algorithms
   - User interactions
   - Error handling behaviors

4. **Performance & Scale**: Non-functional requirements
   - Response time targets
   - Throughput requirements
   - Resource constraints

## Functional Requirements Format

Each requirement:
- **FR-XXX**: System MUST/SHOULD/MAY [specific capability]
- Use active voice and imperative mood
- One requirement per line
- Numbered sequentially (FR-001, FR-002, ...)

Example:
```
- **FR-001**: System MUST accept GeoJSON files in standard RFC 7946 format
- **FR-002**: System MUST support Point, LineString, Polygon, and Multi* geometries
- **FR-003**: System MUST generate PNG images with configurable dimensions
```

## User Scenarios Format

**Primary User Story** (1 sentence):
```
User provides input file and configuration; system processes data and produces output with expected results.
```

**Acceptance Scenarios** (Given-When-Then):
```
1. **Given** valid input file, **When** user runs command with default settings, **Then** output file created with expected format
2. **Given** invalid input, **When** user runs command, **Then** clear error message displayed
```

**Edge Cases** (questions):
```
- What happens when input file is empty?
- What happens when input exceeds size limit?
- How does system handle missing required parameters?
```

## Output Structure

The spec.md should include:

1. **Project Overview**
   - Project name (clear, descriptive)
   - Problem statement (laconic, user-focused)
   - Solution summary (high-level approach)

2. **Functional Requirements**
   - User scenarios and testing (mandatory)
   - Requirements organized by category (mandatory)
   - Input data requirements (FR-001+)
   - Output data requirements (FR-010+)
   - Business logic requirements (FR-020+)
   - Error handling & logging (FR-030+)

3. **Additional Context** (optional)
   - Reference documentation
   - Related projects or examples
   - CLI interface examples (for tools)
   - Performance targets

## Common Pitfalls to Avoid

❌ **Don't include**:
- Technology choices (Node.js, Python, AWS)
- Implementation details (classes, functions, APIs)
- Code examples or structure
- Architecture diagrams
- Library/framework names

✅ **Do include**:
- User needs and workflows
- Data formats and validation rules
- Expected behaviors and outcomes
- Error handling requirements
- Performance targets

## Arguments Expected

When using this command, provide:
1. Feature folder path (e.g., `ai_tasks/another-big-project/ml-automation/feature-snapshotter/`)
2. (Optional) Custom instructions or focus areas

Example usage:
```
/spec ai_tasks/another-big-project/ml-automation/feature-snapshotter/
```

With custom prompt:
```
/spec ai_tasks/data-pipeline/etl-process/ Focus on data validation and error recovery requirements
```

## Success Criteria

Generated spec.md should:
- Be readable by non-technical stakeholders
- Contain only functional requirements (WHAT)
- Have numbered, testable requirements
- Include user scenarios with clear acceptance criteria
- Mark any ambiguities or assumptions
- Follow the template structure
- Use laconic, precise language

**Now generate the spec.md file following the embedded template structure below.**

---

## Embedded Spec Template

Use this template structure when generating spec.md files:

```markdown
# Functional Requirements Template for Claude Code

**Created**: [DATE]
**Status**: Draft
**Input**: User description: "$USER_RAW_PROMPT$"

> Template for creating comprehensive functional requirements for use with Claude Code. Based on analysis of the existing code base, user prompt, provided context, Claude Code best practices.

## Project Overview

### Project Name
`[Brief, descriptive name of the project/feature]`

### Problem Statement
`[Clear laconic description of the problem to be solved or feature to be implemented]`

### Solution Summary
`[High-level laconic approach on how to solve the problem specified in functional requirements ]`

---
## Functional Requirements

### ⚡ Quick Guidelines
- ✅ Focus on WHAT users need and WHY
- ❌ Avoid HOW to implement (no tech stack, APIs, code structure)
- 👥 Written for business stakeholders, not developers
- Use more laconic style

### Section Requirements
- **Mandatory sections**: Must be completed for every feature

### For AI Generation
When creating this spec from a user prompt:
1. **Mark all ambiguities**: Use [NEEDS CLARIFICATION: specific question] for any assumption you'd need to make
2. **Don't guess**: If the prompt doesn't specify something (e.g., "login system" without auth method), mark it
3. **Think like a tester**: Every vague requirement should fail the "testable and unambiguous" checklist item
4. **Common underspecified areas**:
    - Performance targets and scale
    - Error handling behaviors
    - Integration requirements
### User Scenarios & Testing *(mandatory)*
#### Primary User Story (main - 1 sentence)
[Describe the main user journey in plain and more laconic English language]

#### Acceptance Scenarios (1-5 main scenarios)
1. **Given** [initial state], **When** [action], **Then** [expected outcome]
2. **Given** [initial state], **When** [action], **Then** [expected outcome]

#### Edge Cases (0-5 edge cases)
- What happens when [edge case]?
- What happens when [boundary condition]?
- How does system handle [error scenario]?

## Requirements *(mandatory)*

Analyze input data and provide a list of functional requirements.
Analyze output data and provide a list of functional requirements.
Analyze business logic (data transformation, user interaction, how we need to process the data to produce the desired output) and provide a list of functional requirements.

### Functional Requirements
- **FR-001**: System MUST [specific capability, e.g., "allow users to create accounts"]
- **FR-002**: System MUST [specific capability, e.g., "validate email addresses"]
- **FR-003**: Users MUST be able to [key interaction, e.g., "reset their password"]
- **FR-004**: System MUST [data requirement, e.g., "persist user preferences"]
- **FR-005**: System MUST [behavior, e.g., "log all security events"]
---


## Additional Context *(optional)*

### Reference Documentation
- `[Links to relevant documentation]`
- `[Related projects or examples]`

---

## Template Usage Notes

**Best Practices:**
- Use plain and more laconic English descriptions that Claude can understand and act on
```
