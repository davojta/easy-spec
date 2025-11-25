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
