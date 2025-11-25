# Research: Migrate format-book from Poetry to UV

**Date**: 2025-11-25
**Project**: format-book
**Status**: Complete

---

## Problem / Question

Migrate Python project from Poetry to UV package manager to leverage faster dependency resolution and unified tooling.

**Why this matters:**
- UV provides 10-100x faster operations than pip/Poetry
- Consolidates multiple tools (Poetry, pipx, pyenv, virtualenv) into one
- Modern Rust-based architecture with better performance

---

## Existing Patterns

### Current format-book Project Structure

**Location**: `/Users/dzianissheka/projects/dev/private/format-book`
**Package Manager**: Poetry
**Python Version**: ^3.13

**Key Files:**
```
pyproject.toml           # Poetry configuration
poetry.lock              # Poetry lockfile
main.py                  # CLI entry point using Click
src/utils/               # Utility modules
tests/                   # Test directory
.pre-commit-config.yaml  # Pre-commit hooks
```

**Current Dependencies:**
```toml
[tool.poetry.dependencies]
python = "^3.13"
pandas = "^2.2.3"
openai = "^1.54.3"
click = "^8.1.7"
python-dotenv = "^1.0.1"
pre-commit = "^4.0.1"

[tool.poetry.group.dev.dependencies]
flake8 = "^7.1.1"
pytest = ">=7.0.0"
black = "^24.10.0"
ipykernel = "^6.29.5"
```

**Current Workflow:**
```bash
poetry install                    # Install dependencies
poetry run python main.py [cmd]   # Run CLI commands
```

### Migration Pattern

**Pattern**: Manual pyproject.toml conversion using pdm import + cleanup
**Alternative**: Automated tool `migrate-to-uv`

---

## Investigation Results

### ✅ Verified

- UV is an extremely fast Python package/project manager written in Rust
- UV replaces pip, pip-tools, pipx, poetry, pyenv, twine, virtualenv
- Migration tools exist: `migrate-to-uv` (automated) and `uvx pdm import` (manual)
- Current project uses standard dependencies compatible with UV
- Click CLI framework works seamlessly with UV's `uv run` command

### ⚠️ Assumptions

- Pre-commit hooks will work without modification
- Jupyter notebook (format-text.ipynb) will function with UV-managed kernel
- No Poetry-specific plugins or custom build backends are in use

### ❌ Gaps

- No information about current Poetry lock file issues or limitations
- Unknown if any dependencies have Poetry-specific configurations

---

## Technical Analysis

### Migration Data Flow

```
Current Poetry Setup
↓
Run migration tool (migrate-to-uv or uvx pdm import)
↓
Convert [tool.poetry] → [project] sections
↓
Remove Poetry-specific sections
↓
Generate uv.lock from converted pyproject.toml
↓
Install dependencies with uv sync
↓
UV-managed project
```

### Key Migration Changes

**pyproject.toml structure:**
```toml
# Before (Poetry)
[tool.poetry]
name = "format-book"
version = "0.1.0"
dependencies = {...}

[tool.poetry.group.dev.dependencies]
flake8 = "^7.1.1"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"

# After (UV)
[project]
name = "format-book"
version = "0.1.0"
dependencies = [...]

[dependency-groups]
dev = [
    "flake8>=7.1.1",
]

[tool.uv]
default-groups = []
```

**Command mapping:**
| Poetry | UV |
|--------|-----|
| `poetry install` | `uv sync` |
| `poetry add pkg` | `uv add pkg` |
| `poetry remove pkg` | `uv remove pkg` |
| `poetry run python main.py` | `uv run python main.py` |
| `poetry run pytest` | `uv run pytest` |
| `poetry shell` | `source .venv/bin/activate` (after `uv sync`) |

### Dependencies

- **migrate-to-uv**: Automated migration tool
- **pdm**: Used for manual import step (`uvx pdm import`)
- **UV**: Target package manager (install via `curl -LsSf https://astral.sh/uv/install.sh | sh`)

---

## Decision Matrix

| Approach | Pros | Cons | Effort | Recommendation |
|----------|------|------|--------|----------------|
| Automated (migrate-to-uv) | Fast, handles most cases automatically | May need manual review of edge cases | Low (5 min) | ✅ **Preferred** |
| Manual (pdm import + cleanup) | Full control over conversion | More time-consuming, error-prone | Medium (15-30 min) | Use if automated fails |
| Hybrid (automated + manual fixes) | Best of both worlds | Requires validation | Low-Medium | Good fallback |

**Final Decision:** Use automated `migrate-to-uv` tool, then validate and test

**Rationale:**
- Fastest migration path with lowest error risk
- Handles standard Poetry configurations automatically
- format-book has straightforward dependencies without complex Poetry-specific features
- Easy to fall back to manual approach if needed

---

## Implementation Guidance

### Architecture

```
format-book/
├── pyproject.toml      # UV-compatible configuration
├── uv.lock             # UV lockfile (replaces poetry.lock)
├── .venv/              # UV-managed virtual environment
├── main.py             # No changes needed
├── src/                # No changes needed
└── tests/              # No changes needed
```

### Key Implementation Points

1. **Install UV**
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

2. **Run Migration Tool**
   ```bash
   cd /Users/dzianissheka/projects/dev/private/format-book
   uvx migrate-to-uv
   ```

3. **Review pyproject.toml**
   - Verify [project] section has correct metadata
   - Check [dependency-groups] for dev dependencies
   - Ensure version constraints converted correctly (^2.2.3 → >=2.2.3)

4. **Clean and Reinstall**
   ```bash
   rm -rf .venv poetry.lock
   uv sync
   ```

5. **Update README.md**
   - Replace Poetry commands with UV equivalents
   - Update getting started instructions

6. **Update CI/CD (if exists)**
   - Replace `poetry install` with `uv sync`
   - Update caching keys from poetry.lock to uv.lock

### Error Handling

- **Migration tool fails**: Fallback to manual `uvx pdm import pyproject.toml`
- **Dependency resolution errors**: Check for conflicting version constraints
- **Import errors after migration**: Verify `uv sync` completed successfully

### Performance Considerations

- UV lockfile generation is significantly faster than Poetry
- First `uv sync` may take time downloading packages; subsequent syncs are instant
- UV caches packages globally for reuse across projects

---

## Usage Example

```bash
# Install UV
curl -LsSf https://astral.sh/uv/install.sh | sh

# Navigate to project
cd /Users/dzianissheka/projects/dev/private/format-book

# Run migration
uvx migrate-to-uv

# Verify and sync
uv sync

# Run existing commands (no code changes needed)
uv run python main.py generate-stats --help
uv run python main.py format-folder input/ output/
uv run pytest

# Add new dependency
uv add requests

# Remove old dependency
uv remove pandas
```

---

## Next Steps

1. [ ] Install UV on development machine
2. [ ] Backup current project (git commit or branch)
3. [ ] Run `uvx migrate-to-uv` in format-book directory
4. [ ] Review generated pyproject.toml
5. [ ] Delete poetry.lock and .venv
6. [ ] Run `uv sync` to create new environment
7. [ ] Test all CLI commands work correctly
8. [ ] Update README.md with UV commands
9. [ ] Update .github/workflows if CI/CD exists
10. [ ] Commit changes to git

**Blockers:**
- None identified; migration should be straightforward

---

## References

### Code References
- Current project: `/Users/dzianissheka/projects/dev/private/format-book/pyproject.toml`
- Main CLI: `/Users/dzianissheka/projects/dev/private/format-book/main.py`

### Documentation
- [UV Documentation](https://docs.astral.sh/uv)
- [UV GitHub Repository](https://github.com/astral-sh/uv)
- [Loopwerk: Migrate Poetry to UV](https://www.loopwerk.io/articles/2024/migrate-poetry-to-uv/)
- [Stack Overflow: Migrate from Poetry to UV](https://stackoverflow.com/questions/79118841/how-can-i-migrate-from-poetry-to-uv-package-manager)
- [Python Dev Tools: Migration Guide](https://pydevtools.com/handbook/how-to/how-to-migrate-from-poetry-to-uv/)

### Related Research
- UV project guide: https://docs.astral.sh/uv/guides/projects/
- Migration tool: https://pypi.org/project/poetry-to-uv/

---

## Quick Summary

**TL;DR:**
- UV is 10-100x faster than Poetry, written in Rust
- Use automated `uvx migrate-to-uv` tool for migration (5 minutes)
- Main changes: pyproject.toml structure, replace poetry.lock with uv.lock
- No code changes needed; CLI commands use `uv run` instead of `poetry run`
- Update README.md and CI/CD after migration

**Status:** Ready for implementation
