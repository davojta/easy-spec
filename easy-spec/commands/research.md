Generate research document for investigating existing patterns and technical approaches.

## Process

1. **FIRST**: Check if the user already provided folder path and problem statement in their message
   - Look for folder paths in the user's input (absolute paths like /Users/... or relative paths like ai_tasks/...)
   - Look for problem/question descriptions (e.g., "new night design for landmark icons")
   - Look for area of focus (e.g., "bin/create-anki-cards.ts bin/format-book.ts")
   - Look for project paths to search (e.g., "projects: /path/to/proj1/, /path/to/proj2/")
   - Only prompt if critical information (folder path and problem) is missing
2. Use the embedded research template structure provided below
3. Identify the relevant project context:
   - If user provided project paths, use those directories for searching
   - Otherwise, search in the current working directory and infer from context
4. Search for existing patterns and reference implementations
5. Investigate technical details and data flows
6. Document findings with code references and file paths
7. Analyze options and make recommendations
8. Output research.md to the feature folder

## Key Principles

**Focus on existing code patterns:**
- ✅ Reference real file paths from the codebase
- ✅ Include actual code snippets showing patterns
- ✅ Compare similar implementations across projects
- ❌ Don't invent solutions without checking existing code first

**Laconic and practical:**
- Brief problem statement (1-2 sentences)
- Clear verification of findings (✅ Verified, ⚠️ Assumptions, ❌ Gaps)
- Concrete code references with file paths
- Actionable implementation guidance

**Deep investigation:**
- Search for patterns in relevant projects
- Analyze API contracts and data flows
- Document decision rationale with trade-offs
- Identify blockers and unknowns

## Research Structure

### 1. Problem / Question
Define what needs investigation:
- Clear problem statement
- Why it matters (business/technical reason)

### 2. Existing Patterns
Find and document reference implementations:
- **Location**: Full file paths
- **Project**: Which codebase (pipeline, qa-tool, etc.)
- **Pattern**: What approach is used
- **Code**: Laconic examples
- **Analysis**: What works, what doesn't apply

### 3. Investigation Results
Organize findings:
- **✅ Verified**: Confirmed with evidence
- **⚠️ Assumptions**: Need validation
- **❌ Gaps**: Missing information

### 4. Technical Analysis
Document key details:
- Data flow diagrams
- API/Interface contracts
- Dependencies and versions

### 5. Decision Matrix
Compare options:
- Table with Pros/Cons/Effort
- Final decision with rationale

### 6. Implementation Guidance
Provide actionable direction:
- Recommended architecture
- Key implementation points with references
- Error handling strategies
- Performance considerations

### 7. Usage Example
Show expected usage patterns

### 8. Next Steps
List concrete actions with checkboxes

## Search Strategy

When investigating, search for:
1. Similar functionality (CLI commands, services, utilities)
2. Similar tech stack (Node.js/TypeScript, Python, AWS)
3. Similar data patterns (S3 downloads, Athena queries, rendering)
4. Error handling and performance patterns

**Project locations** should be provided by the user or inferred from their workspace. If not provided, search the current working directory and common project locations.

## Verification Requirements

For each finding:
- Include full file path
- Reference specific line numbers if helpful
- Show actual code (not pseudo-code)
- Mark as ✅ Verified only if confirmed from actual code

## Arguments Expected

When using this command, check the user's message for:
1. **Folder path** - Look for absolute or relative paths (e.g., `ai_tasks/another-big-project/ml-automation/feature-name/`)
2. **Problem statement** - Look for descriptions of what to investigate
3. **Area of focus** - Look for mentions of specific files, components, or areas
4. **Project paths** (optional) - Look for project directories to search in

Example usage patterns:
```
/research ai_tasks/another-big-project/ml-automation/new-feature/
```

With problem statement in same message:
```
/research
Investigate concurrent S3 download patterns in existing codebase
folder: ai_tasks/another-big-project/ml-automation/s3-downloads/
```

With inline problem, folder, and focus:
```
/research new night design for landmark icons
folder: /path/to/folder
focus: bin/create-anki-cards.ts bin/format-book.ts
```

With project paths to search:
```
/research new night design for landmark icons
folder: /path/to/folder
focus: bin/create-anki-cards.ts bin/format-book.ts
projects: /Users/dzianissheka/projects/dev/work/pipeline/, /Users/dzianissheka/projects/dev/work/qa-tool/
```

## Success Criteria

Generated research.md should:
- Start with clear problem/question
- Reference actual code with file paths
- Include ✅ Verified findings (not assumptions)
- Provide decision matrix with rationale
- Give implementation guidance with examples
- List next steps with dependencies
- Be laconic and actionable
- Follow the template structure

## Common Patterns to Look For

**CLI Tools:**
- Command structure (commander.js, typer)
- Argument parsing patterns
- Progress indicators (Rich, ora)

**AWS Integration:**
- S3 download patterns (concurrent, streaming)
- Athena query execution
- Secrets Manager access

**Data Processing:**
- GeoJSON handling
- Parquet read/write
- Schema validation (Pydantic, Zod)

**Testing:**
- Unit test patterns
- Integration test setup
- Mocking strategies

**Now generate the research.md file following the embedded template structure below, focusing on existing patterns and verified findings from the codebase.**

---

## Embedded Research Template

Use this template structure when generating research.md files:

```markdown
# Research: [Feature/Problem Name]

**Date**: YYYY-MM-DD
**Project**: [pipeline | data-pipeline | another-big-project | mobile-app-repo | qa-tool | map-pipeline | map-pipeline-common]
**Status**: [Draft | In Progress | Complete]

---

## Problem / Question

[Clear, very laconic statement of what needs to be investigated. One sentence if possible.]

**Why this matters:**
- [Business/technical reason]
- [Impact on workflow/system]

---

## Existing Patterns

### Reference Implementation

**Location**: `/path/to/reference/code`
**Project**: [project-name]
**Pattern**: [What pattern/approach is used]

**Key Files:**
```
path/to/file1.ts  # Purpose
path/to/file2.ts  # Purpose
```

**Code Pattern:**
```typescript
// Laconic example showing the key pattern
```

**What works:**
- [Strength 1]
- [Strength 2]

**What doesn't apply:**
- [Limitation 1]
- [Limitation 2]

### Alternative Approaches

**Option 1:** [Name]
- **Where**: `/path/to/code`
- **Pro**: [Key benefit]
- **Con**: [Key limitation]

**Option 2:** [Name]
- **Where**: `/path/to/code`
- **Pro**: [Key benefit]
- **Con**: [Key limitation]

---

## Investigation Results

### ✅ Verified

- [Confirmed finding 1 with evidence]
- [Confirmed finding 2 with evidence]

### ⚠️ Assumptions

- [Assumption 1 that needs validation]
- [Assumption 2 that needs validation]

### ❌ Gaps

- [Missing information 1]
- [Unknown 2 - needs further investigation]

---

## Technical Analysis

### Data Flow

```
Input → [Process 1] → [Process 2] → Output
```

**Key Points:**
- [Critical detail 1]
- [Critical detail 2]

### API/Interface Contract

**Input:**
```typescript
// Expected input structure
```

**Output:**
```typescript
// Expected output structure
```

**Dependencies:**
- [Dependency 1] - [Version/source]
- [Dependency 2] - [Version/source]

---

## Decision Matrix

| Approach | Pros | Cons | Effort | Recommendation |
|----------|------|------|--------|----------------|
| Option 1 | | | | |
| Option 2 | | | | |
| Option 3 | | | | |

**Final Decision:** [Chosen approach]

**Rationale:**
- [Reason 1]
- [Reason 2]

---

## Implementation Guidance

### Architecture

```
component1/
├── file1.ts  # Purpose
└── file2.ts  # Purpose
```

### Key Implementation Points

1. **[Component 1]**
   - Pattern: [Reference to existing code]
   - Critical detail: [What must be done]

2. **[Component 2]**
   - Pattern: [Reference to existing code]
   - Critical detail: [What must be done]

### Error Handling

- [Error case 1]: [Strategy]
- [Error case 2]: [Strategy]

### Performance Considerations

- [Consideration 1]
- [Consideration 2]

---

## Usage Example

```bash
# Basic usage
command --input value --output result

# Advanced usage
command --flag1 --flag2 value
```

---

## Next Steps

1. [ ] [Action 1]
2. [ ] [Action 2]
3. [ ] [Action 3]

**Blockers:**
- [Blocker 1 if any]

---

## References

### Code References
- Project: `/Users/dzianissheka/projects/dev/work/[project]/path/to/file`
- Pattern: `/Users/dzianissheka/projects/dev/work/[project]/path/to/pattern`

### Documentation
- [Doc link 1]
- [Doc link 2]

### Related Research
- `ai_tasks/[project]/[feature]/research.md`

---

## Quick Summary

**TL;DR:**
- [Key finding 1]
- [Key finding 2]
- [Recommended approach]

**Status:** [Ready for spec | Needs more investigation | Blocked]
```
```
