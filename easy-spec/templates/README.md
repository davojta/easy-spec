# Templates

This directory contains templates used by the easy-spec commands.

## Available Templates

### `research_template.md`
Template for research documents that investigate existing patterns and technical approaches. Used by the `/research` command.

**Purpose**: Structure research findings with verified patterns, decision matrices, and implementation guidance.

### `spec_template.md`
Template for functional requirements specifications. Used by the `/spec` command.

**Purpose**: Document WHAT the system should do (functional requirements) in business-stakeholder language.

### `implementation-plan_template.md`
Template for technical implementation plans. Used by the `/plan` command.

**Purpose**: Define HOW to implement the requirements (technical stack, architecture, phases).

### `tasks_template.md`
Template for detailed task lists. Used by the `/tasks` command.

**Purpose**: Break down implementation plan into numbered, actionable tasks with dependencies and parallel execution markers.

## Usage

These templates are automatically referenced by the plugin commands using the `@templates/` prefix:

```markdown
Read @templates/research_template.md for structure guidance
```

## Template Structure

Each template follows a consistent laconic, actionable format:
- Clear section headers with purpose
- Examples and code snippets
- Checklists and verification criteria
- File path references
- Decision documentation

## Customization

These templates can be customized for specific project needs while maintaining the core structure expected by the commands.
