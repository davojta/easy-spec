# Functional Requirements: Migrate format-book to UV

**Created**: 2025-11-25
**Status**: Ready for Implementation

## Project Overview

### Project Name
`format-book Poetry to UV Migration`

### Problem Statement
Migrate Python project from Poetry to UV for faster dependency resolution while maintaining functionality.

### Solution Summary
Convert Poetry configuration to UV format, preserve all dependencies, update workflows.

---
## Functional Requirements

### ⚡ Quick Guidelines
- ✅ Focus on WHAT users need and WHY
- ❌ Avoid HOW to implement (no tech stack, APIs, code structure)
- 👥 Written for business stakeholders, not developers
- Use more laconic style

### User Scenarios & Testing

#### Primary User Story
Developer runs migration tool, syncs dependencies, continues development with identical command behavior.

#### Acceptance Scenarios

1. **Given** Poetry pyproject.toml, **When** migration runs, **Then** UV-compatible configuration generated
2. **Given** migrated config, **When** sync dependencies, **Then** all dependencies install correctly
3. **Given** UV environment, **When** run CLI commands, **Then** identical behavior to Poetry version
4. **Given** migrated project, **When** add/remove dependencies, **Then** lockfile updates correctly
5. **Given** migrated project, **When** run tests, **Then** all tests pass unchanged

#### Questions & Answers

- **Q**: Poetry-specific plugins or build backends?
  - **A**: Not needed, standard poetry-core only
- **Q**: Version constraints fail conversion?
  - **A**: Only major version mapping needed
- **Q**: Jupyter kernel references Poetry environment?
  - **A**: Jupyter support not required
- **Q**: Pre-commit hooks reference Poetry?
  - **A**: Replace with UV-managed version
- **Q**: Conflicting dependencies in poetry.lock?
  - **A**: Must resolve conflicts during migration

#### Edge Cases

- Resolve conflicting dependencies during migration

## Requirements *(mandatory)*

### Input Data

- **FR-001**: Accept pyproject.toml with Poetry configuration
- **FR-002**: Accept poetry.lock lockfile
- **FR-003**: Preserve Python ^3.13 requirement
- **FR-004**: Accept production dependencies (pandas, openai, click, python-dotenv, pre-commit)
- **FR-005**: Accept dev dependencies (flake8, pytest, black)
- **FR-006**: Map major version from Poetry format

### Output Data

- **FR-007**: Produce UV pyproject.toml with [project] section
- **FR-008**: Generate uv.lock replacing poetry.lock
- **FR-009**: Preserve metadata (name, version, description, authors)
- **FR-010**: Production dependencies in [project.dependencies]
- **FR-011**: Dev dependencies in [dependency-groups]
- **FR-012**: Convert to major version format
- **FR-013**: Preserve dependency relationships
- **FR-014**: Remove Poetry sections ([tool.poetry], [build-system])

### Business Logic

- **FR-015**: Convert dependencies without losing constraints
- **FR-016**: Validate UV format resolution
- **FR-017**: Create isolated virtual environment
- **FR-018**: Install dependencies matching lockfile
- **FR-019**: Support adding dependencies with lockfile update
- **FR-020**: Support removing dependencies with lockfile update
- **FR-021**: Run scripts in managed environment
- **FR-022**: Run tests in managed environment
- **FR-023**: Maintain project isolation

### Workflow

- **FR-024**: Sync dependencies from lockfile
- **FR-025**: Execute Python scripts with dependencies
- **FR-026**: Run CLI commands (format-folder, generate-stats, combine-chapters)
- **FR-027**: Run test suite
- **FR-028**: Add dependencies via command line
- **FR-029**: Remove dependencies via command line
- **FR-030**: Activate environment for interactive work

### Validation & Errors

- **FR-031**: Validate pyproject.toml syntax before conversion
- **FR-032**: Report clear error for malformed Poetry config
- **FR-033**: Warn if Poetry features cannot convert
- **FR-034**: Detect and resolve conflicting constraints
- **FR-035**: Validate lockfile matches pyproject.toml
- **FR-036**: Report clear error if package not found
- **FR-037**: Report error if Python version unsatisfied

### Documentation

- **FR-038**: Output successful conversion message
- **FR-039**: Update docs with UV commands
- **FR-040**: Include installation instructions
- **FR-041**: Include command examples

### Compatibility

- **FR-042**: Preserve Python source unchanged
- **FR-043**: Preserve tests unchanged
- **FR-044**: Replace pre-commit hooks with UV-managed version
- **FR-045**: Support same CLI with identical behavior
- **FR-046**: Maintain identical runtime behavior

---

## Additional Context

### Reference Documentation
- UV Documentation: https://docs.astral.sh/uv
- Migration Guide: https://www.loopwerk.io/articles/2024/migrate-poetry-to-uv/
- Project: /Users/dzianissheka/projects/dev/private/format-book

### Command Equivalence

| Operation | Poetry | UV |
|-----------|--------|-----|
| Install | `poetry install` | `uv sync` |
| Add | `poetry add pkg` | `uv add pkg` |
| Remove | `poetry remove pkg` | `uv remove pkg` |
| Run | `poetry run python main.py` | `uv run python main.py` |
| Test | `poetry run pytest` | `uv run pytest` |
| Shell | `poetry shell` | `source .venv/bin/activate` |

### CLI Commands
- `generate-stats <input> <output>` - Text statistics
- `format-folder <input> <output>` - Format text files
- `combine-chapters <input>` - Combine chapters
