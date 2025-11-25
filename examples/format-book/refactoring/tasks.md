# Tasks: Migrate format-book to UV

**Created**: 2025-11-25
**Status**: Ready for Execution
**Input**: plan.md, spec.md, research.md
**Project**: /Users/dzianissheka/projects/dev/private/format-book

---

## Tasks Breakdown

### Phase 3.1: Setup & Preparation

- [ ] **T001**: Verify current project state and backup
- [ ] **T002**: Install UV package manager
- [ ] **T003**: Create pre-migration git commit

### Phase 3.2: Migration Execution

- [ ] **T004**: Run automated migration tool
- [ ] **T005**: Review generated pyproject.toml
- [ ] **T006**: Validate dependency conversions

### Phase 3.3: Environment Setup

- [ ] **T007**: Remove old Poetry artifacts
- [ ] **T008**: Create UV virtual environment
- [ ] **T009**: Sync dependencies with UV

### Phase 3.4: Integration Testing

- [ ] **T010** [P]: Test CLI command: generate-stats
- [ ] **T011** [P]: Test CLI command: format-folder
- [ ] **T012** [P]: Test CLI command: combine-chapters
- [ ] **T013**: Run pytest suite validation

### Phase 3.5: Configuration Updates

- [ ] **T014**: Update .pre-commit-config.yaml
- [ ] **T015**: Update README.md with UV commands
- [ ] **T016**: Validate pre-commit hooks

### Phase 3.6: Final Validation

- [ ] **T017**: Test dependency add/remove operations
- [ ] **T018**: Verify all acceptance scenarios
- [ ] **T019**: Create migration commit

---

## Dependencies

```
T001 → T002 → T003 → T004
T004 → T005 → T006
T006 → T007 → T008 → T009
T009 → T010, T011, T012 (parallel)
T009 → T013
T013 → T014, T015 (parallel)
T015 → T016
T016 → T017 → T018 → T019
```

**Parallel Execution Opportunities**:
- T010, T011, T012 (different CLI commands, independent tests)
- T014, T015 (different files, no dependencies)

---

## Detailed Task Specifications

### Task T001: Verify Current Project State and Backup

**Objective**: Ensure project is in clean state before migration and document current Poetry setup

**Requirements**:
- Git working directory clean (no uncommitted changes)
- Current Poetry installation working
- Document current Poetry version and dependencies
- Verify all tests pass with Poetry

**Key Components**:
```bash
# Check git status
git status

# Verify Poetry working
poetry --version
poetry show

# Run tests to establish baseline
poetry run pytest

# Document current state
poetry export -f requirements.txt --output pre-migration-requirements.txt
```

**Implementation Details**:
- Navigate to /Users/dzianissheka/projects/dev/private/format-book
- Run git status to verify clean state
- If uncommitted changes exist, prompt user to commit or stash
- Run `poetry run pytest` to verify tests pass
- Export dependencies for comparison after migration

**Files to Verify**:
- ✅ `/Users/dzianissheka/projects/dev/private/format-book/.git/` - Git repo exists
- ✅ `/Users/dzianissheka/projects/dev/private/format-book/pyproject.toml` - Poetry config
- ✅ `/Users/dzianissheka/projects/dev/private/format-book/poetry.lock` - Lock file

---

### Task T002: Install UV Package Manager

**Objective**: Install UV package manager on system

**Requirements**:
- Internet connectivity for download
- Shell access for installation script
- Verify UV available in PATH after install

**Key Components**:
```bash
# Install UV
curl -LsSf https://astral.sh/uv/install.sh | sh

# Verify installation
uv --version

# Check UV available
which uv
```

**Implementation Details**:
- Run official UV installation script
- Script installs to ~/.cargo/bin/ or appropriate location
- Verify UV command accessible in PATH
- Expected version: 0.4.0 or later

**Files Created**:
- ⏳ `~/.cargo/bin/uv` - UV executable

---

### Task T003: Create Pre-Migration Git Commit

**Objective**: Create safety checkpoint before migration begins

**Requirements**:
- All current changes committed
- Clear commit message indicating pre-migration state
- Tag commit for easy rollback if needed

**Key Components**:
```bash
# Stage all files
git add .

# Create pre-migration commit
git commit -m "[format-book] Pre-migration checkpoint before Poetry to UV migration"

# Tag for easy reference
git tag pre-uv-migration
```

**Implementation Details**:
- Commit current state with descriptive message
- Tag allows easy `git reset --hard pre-uv-migration` if needed
- Push to remote if working with team

**Files Modified**:
- ✅ `.git/` - New commit created

---

### Task T004: Run Automated Migration Tool

**Objective**: Execute uvx migrate-to-uv to convert Poetry config to UV format

**Requirements**:
- UV installed and available
- Current directory is project root
- pyproject.toml contains Poetry configuration

**Key Components**:
```bash
# Navigate to project
cd /Users/dzianissheka/projects/dev/private/format-book

# Run migration tool
uvx migrate-to-uv

# Check migration output
echo $?  # Should be 0 for success
```

**Implementation Details**:
- Tool reads pyproject.toml with [tool.poetry] sections
- Converts to [project] sections compatible with UV
- Automatically handles dependency version conversion
- If tool fails, note error and prepare for manual fallback (T004-ALT)

**Fallback (T004-ALT): Manual Migration**:
```bash
# If automated fails, use pdm import
uvx pdm import pyproject.toml

# Then manually edit pyproject.toml:
# - Rename [tool.pdm.dev-dependencies] to [dependency-groups]
# - Remove [tool.poetry] and [build-system] sections
# - Add [tool.uv] with default-groups = []
```

**Files Modified**:
- ⏳ `/Users/dzianissheka/projects/dev/private/format-book/pyproject.toml` - Converted to UV format

---

### Task T005: Review Generated pyproject.toml

**Objective**: Manually validate migration tool output is correct

**Requirements**:
- pyproject.toml successfully converted
- [project] section exists with correct metadata
- All dependencies present
- No Poetry-specific sections remain

**Key Components**:
```toml
# Expected structure
[project]
name = "format-book"
version = "0.1.0"
description = "Project to format the book with chatgpt"
authors = [{name = "Dzianis Sheka", email = "dzianis.sheka@gmail.com"}]
requires-python = ">=3.13"
dependencies = [
    "pandas>=2.2.3",
    "openai>=1.54.3",
    "click>=8.1.7",
    "python-dotenv>=1.0.1",
    "pre-commit>=4.0.1",
]

[dependency-groups]
dev = [
    "flake8>=7.1.1",
    "pytest>=7.0.0",
    "black>=24.10.0",
]

[tool.uv]
default-groups = []
```

**Validation Checklist**:
- ✅ [project] section exists
- ✅ name, version, description, authors preserved
- ✅ requires-python = ">=3.13" (converted from ^3.13)
- ✅ All 5 production dependencies listed
- ✅ All 3 dev dependencies in [dependency-groups]
- ✅ No [tool.poetry] section
- ✅ No [build-system] section
- ✅ Version constraints use >= format (not ^)

**Implementation Details**:
- Read pyproject.toml
- Compare against expected structure above
- Verify ipykernel NOT included (not required per user)
- Check version constraints converted correctly

**Files to Review**:
- ✅ `/Users/dzianissheka/projects/dev/private/format-book/pyproject.toml` - Validate conversion

---

### Task T006: Validate Dependency Conversions

**Objective**: Verify all dependencies converted with correct version constraints

**Requirements**:
- All original Poetry dependencies present
- Version constraints use major version format (>=x.y.z)
- No missing or extra dependencies

**Key Components**:
```python
# Expected conversions
# Poetry → UV
"pandas = ^2.2.3" → "pandas>=2.2.3"
"openai = ^1.54.3" → "openai>=1.54.3"
"click = ^8.1.7" → "click>=8.1.7"
"python-dotenv = ^1.0.1" → "python-dotenv>=1.0.1"
"pre-commit = ^4.0.1" → "pre-commit>=4.0.1"
"flake8 = ^7.1.1" → "flake8>=7.1.1"
"pytest >= 7.0.0" → "pytest>=7.0.0"  # Already correct
"black = ^24.10.0" → "black>=24.10.0"
```

**Validation Steps**:
1. Count dependencies: 5 production + 3 dev = 8 total
2. Verify each dependency name matches
3. Check version constraints use >= format
4. Confirm ipykernel excluded (not required)

**Implementation Details**:
- Parse pyproject.toml [project.dependencies]
- Parse [dependency-groups.dev]
- Compare against expected list
- Flag any discrepancies

**Files to Validate**:
- ✅ `/Users/dzianissheka/projects/dev/private/format-book/pyproject.toml` - Check dependencies

---

### Task T007: Remove Old Poetry Artifacts

**Objective**: Clean up Poetry-specific files before UV sync

**Requirements**:
- Migration validated (T005, T006 complete)
- Backup commit created
- Ready to transition to UV

**Key Components**:
```bash
cd /Users/dzianissheka/projects/dev/private/format-book

# Remove Poetry lockfile
rm poetry.lock

# Remove old virtual environment
rm -rf .venv/

# Verify cleanup
ls -la | grep -E "(poetry\.lock|\.venv)"  # Should return nothing
```

**Implementation Details**:
- Delete poetry.lock (will be replaced by uv.lock)
- Delete .venv/ directory (will be recreated by UV)
- These files are safe to delete after backup commit

**Files to Remove**:
- ✅ `/Users/dzianissheka/projects/dev/private/format-book/poetry.lock` - Old lockfile
- ✅ `/Users/dzianissheka/projects/dev/private/format-book/.venv/` - Old venv

---

### Task T008: Create UV Virtual Environment

**Objective**: Initialize UV-managed virtual environment

**Requirements**:
- UV installed
- pyproject.toml in UV format
- poetry.lock and old .venv/ removed
- Python 3.13 available on system

**Key Components**:
```bash
cd /Users/dzianissheka/projects/dev/private/format-book

# UV will create .venv automatically on sync
# Verify Python version
python3 --version  # Should be 3.13+
```

**Implementation Details**:
- UV creates .venv/ automatically during first sync
- Uses Python 3.13 based on requires-python = ">=3.13"
- Virtual environment isolated to project directory

**Expected Result**:
- .venv/ directory will be created by T009 (uv sync)

---

### Task T009: Sync Dependencies with UV

**Objective**: Install all project dependencies using UV

**Requirements**:
- pyproject.toml in UV format
- Old Poetry files removed
- Python 3.13 available

**Key Components**:
```bash
cd /Users/dzianissheka/projects/dev/private/format-book

# Sync dependencies (creates .venv and uv.lock)
uv sync

# Verify success
echo $?  # Should be 0

# Check uv.lock created
ls -la uv.lock

# Verify packages installed
ls .venv/lib/python3.13/site-packages/ | grep -E "(pandas|openai|click)"
```

**Implementation Details**:
- `uv sync` reads pyproject.toml
- Creates .venv/ if doesn't exist
- Generates uv.lock with resolved dependencies
- Installs all [project.dependencies] and [dependency-groups.dev]
- First sync may take 1-2 minutes downloading packages

**Expected Output**:
```
Creating virtualenv at: .venv
Resolved 42 packages in 1.2s
Installed 42 packages in 850ms
```

**Files Created**:
- ⏳ `/Users/dzianissheka/projects/dev/private/format-book/.venv/` - Virtual environment
- ⏳ `/Users/dzianissheka/projects/dev/private/format-book/uv.lock` - Lockfile

---

### Task T010 [P]: Test CLI Command generate-stats

**Objective**: Verify generate-stats command works identically with UV

**Requirements**:
- UV environment synced (T009 complete)
- Test input data available
- Command produces expected output

**Key Components**:
```bash
cd /Users/dzianissheka/projects/dev/private/format-book

# Test help text
uv run python main.py generate-stats --help

# Expected output: help text with usage and options
# Exit code: 0
```

**Implementation Details**:
- Run command using `uv run` prefix
- Verify help text displays
- Command should work identically to `poetry run python main.py generate-stats --help`
- If test input data available, run actual command

**Validation**:
- Exit code 0
- Help text contains "generate-stats"
- No import errors
- No dependency resolution errors

---

### Task T011 [P]: Test CLI Command format-folder

**Objective**: Verify format-folder command works identically with UV

**Requirements**:
- UV environment synced (T009 complete)
- Test input data available
- Command produces expected output

**Key Components**:
```bash
cd /Users/dzianissheka/projects/dev/private/format-book

# Test help text
uv run python main.py format-folder --help

# Expected output: help text with usage and options
# Exit code: 0
```

**Implementation Details**:
- Run command using `uv run` prefix
- Verify help text displays
- Command should work identically to `poetry run python main.py format-folder --help`

**Validation**:
- Exit code 0
- Help text contains "format-folder"
- No import errors

---

### Task T012 [P]: Test CLI Command combine-chapters

**Objective**: Verify combine-chapters command works identically with UV

**Requirements**:
- UV environment synced (T009 complete)
- Test input data available
- Command produces expected output

**Key Components**:
```bash
cd /Users/dzianissheka/projects/dev/private/format-book

# Test help text
uv run python main.py combine-chapters --help

# Expected output: help text with usage and options
# Exit code: 0
```

**Implementation Details**:
- Run command using `uv run` prefix
- Verify help text displays
- Command should work identically to `poetry run python main.py combine-chapters --help`

**Validation**:
- Exit code 0
- Help text contains "combine-chapters"
- No import errors

---

### Task T013: Run pytest Suite Validation

**Objective**: Verify all tests pass with UV environment

**Requirements**:
- UV environment synced
- pytest installed in dev dependencies
- Test suite exists in tests/ directory

**Key Components**:
```bash
cd /Users/dzianissheka/projects/dev/private/format-book

# Run full test suite
uv run pytest

# Expected: all tests pass
# Exit code: 0
```

**Implementation Details**:
- Run pytest using `uv run` prefix
- All tests should pass (same as Poetry version)
- If any tests fail, investigate dependency issues

**Validation**:
- pytest exit code 0
- No test failures
- No import errors
- Test count matches pre-migration baseline

**Success Criteria**:
```
===== test session starts =====
collected X items

tests/... PASSED
...

===== X passed in Y.XXs =====
```

---

### Task T014: Update .pre-commit-config.yaml

**Objective**: Add UV lockfile validation hook to pre-commit

**Requirements**:
- Migration complete
- UV working correctly
- Pre-commit installed

**Key Components**:
```yaml
repos:
-   repo: https://github.com/astralsh/uv-pre-commit
    rev: 0.4.0
    hooks:
    -   id: uv-lock
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v3.2.0
    hooks:
    -   id: trailing-whitespace
    -   id: end-of-file-fixer
    -   id: check-yaml
    -   id: check-added-large-files
-   repo: https://github.com/psf/black
    rev: 22.10.0
    hooks:
    -   id: black
```

**Implementation Details**:
- Add uv-lock hook as first repo
- Keep existing hooks (pre-commit-hooks, black)
- uv-lock validates uv.lock stays in sync with pyproject.toml

**Files to Modify**:
- ⏳ `/Users/dzianissheka/projects/dev/private/format-book/.pre-commit-config.yaml` - Add uv-lock hook

---

### Task T015: Update README.md with UV Commands

**Objective**: Replace Poetry commands with UV equivalents in documentation

**Requirements**:
- Migration complete
- UV commands tested and working

**Key Components**:
```markdown
# format-book

set of utils to format books in different formats

## getting started

```bash
# Install UV
curl -LsSf https://astral.sh/uv/install.sh | sh
```

See also [UV docs](https://docs.astral.sh/uv)

### run script

```bash
uv run python main.py generate-stats --help
```

### install dependencies

```bash
uv sync
```

## What is the next

## next

- [ ] add logger
- [ ] compare by sentence (to see the difference between chapters?)
- [ ] add cli to extract text from epub
- [ ] add cli to extract text from pdf
- [ ] run prompt to get simple finnish text from normal text
- [ ] run prompt to get anki csv from the text file

[... rest unchanged ...]
```

**Implementation Details**:
- Replace `pipx install poetry` → `curl ... | sh` for UV install
- Replace `poetry install` → `uv sync`
- Replace `poetry run python main.py` → `uv run python main.py`
- Update docs link to UV docs
- Keep rest of README unchanged

**Files to Modify**:
- ⏳ `/Users/dzianissheka/projects/dev/private/format-book/README.md` - Update commands

---

### Task T016: Validate Pre-commit Hooks

**Objective**: Verify updated pre-commit config works correctly

**Requirements**:
- .pre-commit-config.yaml updated
- UV environment synced

**Key Components**:
```bash
cd /Users/dzianissheka/projects/dev/private/format-book

# Install pre-commit hooks
uv run pre-commit install

# Run all hooks
uv run pre-commit run --all-files

# Expected: all hooks pass
```

**Implementation Details**:
- Install pre-commit hooks to .git/hooks/
- Run hooks manually to verify
- uv-lock hook should pass
- Existing hooks (black, trailing-whitespace) should pass

**Validation**:
- All hooks exit 0
- No errors reported
- uv-lock hook validates lockfile

---

### Task T017: Test Dependency Add/Remove Operations

**Objective**: Verify UV dependency management commands work correctly

**Requirements**:
- UV environment synced
- pyproject.toml and uv.lock valid

**Key Components**:
```bash
cd /Users/dzianissheka/projects/dev/private/format-book

# Test add dependency
uv add requests
# Expected: pyproject.toml updated, uv.lock regenerated

# Verify added
grep "requests" pyproject.toml

# Test remove dependency
uv remove requests
# Expected: requests removed from pyproject.toml and uv.lock

# Verify removed
grep "requests" pyproject.toml  # Should return nothing
```

**Implementation Details**:
- Add test dependency (requests)
- Verify appears in pyproject.toml [project.dependencies]
- Verify uv.lock updated
- Remove test dependency
- Verify removed from both files
- This validates FR-019, FR-020 from spec

**Validation**:
- `uv add` exits 0, dependency added
- `uv remove` exits 0, dependency removed
- uv.lock regenerates correctly

---

### Task T018: Verify All Acceptance Scenarios

**Objective**: Validate all spec acceptance scenarios pass

**Requirements**:
- All previous tasks complete
- Migration fully validated

**Acceptance Scenarios** (from spec.md):

1. **Migration Success**:
   - ✅ Poetry pyproject.toml → UV-compatible format (T004, T005)
   - ✅ [project] section exists (T005)
   - ✅ [tool.poetry] removed (T005)

2. **Dependency Installation**:
   - ✅ `uv sync` completes successfully (T009)
   - ✅ All dependencies in .venv/ (T009)

3. **CLI Command Execution**:
   - ✅ generate-stats works (T010)
   - ✅ format-folder works (T011)
   - ✅ combine-chapters works (T012)

4. **Dependency Management**:
   - ✅ `uv add` works (T017)
   - ✅ `uv remove` works (T017)
   - ✅ Lockfile updates correctly (T017)

5. **Test Suite Execution**:
   - ✅ All tests pass (T013)

**Implementation Details**:
- Review checklist above
- Verify each scenario validated by corresponding task
- If any scenario fails, debug before proceeding

---

### Task T019: Create Migration Commit

**Objective**: Commit completed migration to git

**Requirements**:
- All tasks complete
- All acceptance scenarios validated
- Ready to finalize migration

**Key Components**:
```bash
cd /Users/dzianissheka/projects/dev/private/format-book

# Stage all changes
git add .

# Create migration commit
git commit -m "[BASEMAP-xxxx][format-book] Migrate from Poetry to UV

- Convert pyproject.toml to UV format
- Replace poetry.lock with uv.lock
- Update README.md with UV commands
- Add uv-lock pre-commit hook
- All tests passing
- All CLI commands working

🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude <noreply@anthropic.com>"

# Verify commit
git log -1 --stat
```

**Implementation Details**:
- Stage all modified and new files
- Create commit with clear description
- Follow commit message format from user's CLAUDE.md
- Include Claude Code attribution
- Verify commit shows expected file changes

**Files Modified in Commit**:
- pyproject.toml (converted to UV format)
- uv.lock (new lockfile)
- .pre-commit-config.yaml (added uv-lock hook)
- README.md (updated commands)

**Files Removed in Commit**:
- poetry.lock (old lockfile)

---

## Configuration & Setup

**Project Structure**:
```
/Users/dzianissheka/projects/dev/private/format-book/
├── pyproject.toml      # UV-compatible config
├── uv.lock             # UV lockfile
├── .venv/              # UV-managed venv
├── main.py             # CLI entry (unchanged)
├── src/
│   └── utils/          # Utils (unchanged)
├── tests/              # Tests (unchanged)
├── .pre-commit-config.yaml  # Updated
└── README.md           # Updated
```

**UV Installation**:
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Environment Setup**:
```bash
cd /Users/dzianissheka/projects/dev/private/format-book
uv sync
```

---

## Implementation Notes

### Parallel Execution Opportunities

**Can Run in Parallel**:
- T010, T011, T012 (CLI command tests - different commands, no shared state)
- T014, T015 (file updates - different files)

**Example Parallel Execution**:
```bash
# After T009 completes, run CLI tests in parallel
uv run python main.py generate-stats --help &
uv run python main.py format-folder --help &
uv run python main.py combine-chapters --help &
wait
```

### Rollback Procedure

If migration fails:
```bash
# Reset to pre-migration state
git reset --hard pre-uv-migration

# Or reset to specific commit
git reset --hard <commit-hash>

# Reinstall Poetry dependencies
poetry install
```

### Success Validation Checklist

✅ **Core Functionality**:
- [ ] UV-compatible pyproject.toml with [project] section
- [ ] uv.lock created successfully
- [ ] .venv/ populated with correct packages

✅ **CLI Commands**:
- [ ] generate-stats works with `uv run`
- [ ] format-folder works with `uv run`
- [ ] combine-chapters works with `uv run`

✅ **Testing**:
- [ ] pytest suite passes (exit code 0)
- [ ] No import errors

✅ **Documentation**:
- [ ] README.md updated with UV commands
- [ ] Pre-commit hooks include uv-lock
- [ ] Pre-commit hooks execute successfully

✅ **Dependency Management**:
- [ ] `uv add` works correctly
- [ ] `uv remove` works correctly
- [ ] uv.lock updates on changes

✅ **Git**:
- [ ] Migration committed with clear message
- [ ] Old Poetry files removed

---

## Reference

**Command Mapping**:
| Poetry | UV |
|--------|-----|
| `poetry install` | `uv sync` |
| `poetry add pkg` | `uv add pkg` |
| `poetry remove pkg` | `uv remove pkg` |
| `poetry run python main.py` | `uv run python main.py` |
| `poetry run pytest` | `uv run pytest` |
| `poetry shell` | `source .venv/bin/activate` |

**Documentation**:
- [UV Docs](https://docs.astral.sh/uv)
- [Migration Guide](https://www.loopwerk.io/articles/2024/migrate-poetry-to-uv/)
- [UV Pre-commit](https://github.com/astralsh/uv-pre-commit)
