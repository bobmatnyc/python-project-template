# Repository Structure

## Complete Directory Layout

```
python-project-template/
├── README.md                        # Template documentation and usage guide
├── TEMPLATE_README.md              # README for understanding the template
├── STRUCTURE.md                    # This file - structure documentation
├── copier.yml                      # Copier configuration with template questions
└── template/                       # Template files (copied to generated projects)
    ├── .gitignore.jinja           # Python .gitignore template
    ├── LICENSE.jinja              # MIT License template
    ├── Makefile.jinja             # Main Makefile with conditional includes
    ├── README.md.jinja            # Project README template
    ├── pyproject.toml.jinja       # Python project configuration
    ├── .makefiles/                # Modular Makefile components
    │   ├── common.mk              # Variables, colors, utility functions
    │   ├── deps.mk                # Dependency management (install, update)
    │   ├── quality.mk             # Code quality (lint, format, type-check)
    │   ├── release.mk             # Release management (version, build, publish)
    │   └── testing.mk             # Test execution and coverage
    └── scripts/                   # Project automation scripts directory
        └── .gitkeep               # Preserve empty directory in git
```

## File Descriptions

### Root Level

**README.md**
- Comprehensive template documentation
- Quick start guide with copier usage
- Feature overview and design philosophy
- Customization instructions
- Troubleshooting section

**TEMPLATE_README.md**
- Simplified template overview
- Quick reference for template features
- Usage and update instructions

**copier.yml**
- Template configuration for Copier
- Defines 10 template questions:
  - `project_name`: Human-readable project name
  - `project_slug`: URL/package-safe name (auto-generated)
  - `project_description`: One-line project summary
  - `python_version`: Minimum Python version (3.10-3.13)
  - `use_testing`: Include pytest/coverage targets
  - `use_release_automation`: Include version/publish targets
  - `use_docker`: Include Docker targets (future)
  - `github_username`: GitHub user/organization
  - `author_name`: Full author name
  - `author_email`: Author email address

### Template Directory

All files in `template/` are processed by Jinja2 and copied to generated projects.

**Makefile.jinja**
- Main Makefile for generated projects
- Conditionally includes modular components based on template questions
- Contains `help` target with automatic documentation
- Project-specific variables (PROJECT_NAME, PYTHON_VERSION)
- Uses `-include` for graceful fallback if modules missing

**pyproject.toml.jinja**
- Modern Python project configuration (PEP 621)
- Build system configuration (setuptools)
- Project metadata (name, version, authors)
- Dependencies and optional dev dependencies
- Tool configuration (ruff, mypy, pytest, coverage)
- Conditional pytest/coverage configuration

**README.md.jinja**
- Generated project README
- Quick start instructions
- Available Make targets documentation
- Project structure overview
- Contributing guidelines

**.gitignore.jinja**
- Standard Python .gitignore patterns
- Virtual environment exclusions
- Build artifacts and distribution files
- Testing and coverage output
- IDE-specific files

**LICENSE.jinja**
- MIT License template
- Auto-populated with author name and current year

### .makefiles/ Modules

**common.mk** (1.2KB)
- Color definitions (RED, GREEN, YELLOW, BLUE, NC)
- Path detection (PROJECT_ROOT)
- Python/pip detection with fallback
- Virtual environment detection and paths
- Directory constants (SRC_DIR, TESTS_DIR, BUILD_DIR, DIST_DIR)
- Environment detection (ENV variable)
- Utility functions:
  - `print_header`: Blue section headers
  - `print_success`: Green success messages
  - `print_error`: Red error messages
  - `print_warning`: Yellow warning messages

**quality.mk** (999B)
- `quality`: Run all quality checks (lint + type-check)
- `lint`: Alias for lint-check
- `lint-check`: Run ruff check (read-only)
- `lint-fix`: Auto-fix issues with ruff
- `format`: Format code with ruff format
- `format-check`: Check formatting without changes
- `type-check`: Run mypy type checking

**testing.mk** (1.2KB)
- `test`: Run pytest with coverage (default)
- `test-fast`: Run tests without coverage
- `test-verbose`: Run tests with -vv output
- `test-parallel`: Run tests with pytest-xdist (-n auto)
- `coverage`: Alias for test
- `coverage-report`: Generate coverage report to terminal
- `coverage-html`: Generate HTML coverage report
- `test-watch`: Run tests in watch mode (pytest-watch)
- Configurable via PYTEST_ARGS and COVERAGE_MIN variables

**deps.mk** (1.4KB)
- `install`: Install project in development mode (pip install -e .)
- `install-dev`: Install with dev dependencies (.[dev])
- `deps-install`: Alias for install-dev
- `deps-update`: Update all dependencies
- `deps-sync`: Sync dependencies from pyproject.toml
- `venv`: Create virtual environment with upgraded pip/setuptools
- `deps-clean`: Remove all installed packages

**release.mk** (2.3KB)
- `version`: Show current version from VERSION file
- `version-patch`: Bump patch version (0.1.0 -> 0.1.1)
- `version-minor`: Bump minor version (0.1.0 -> 0.2.0)
- `version-major`: Bump major version (0.1.0 -> 1.0.0)
- `patch`: Bump patch + build
- `minor`: Bump minor + build
- `major`: Bump major + build
- `build`: Build distribution packages (python -m build)
- `publish`: Publish to PyPI (twine upload)
- `release`: Full workflow (quality + test + build)
- `clean`: Remove build artifacts, caches, __pycache__

## Jinja2 Template Features

### Conditional Includes

The main `Makefile.jinja` conditionally includes modules:

```jinja
{% if use_testing -%}
-include .makefiles/testing.mk
{% endif -%}

{% if use_release_automation -%}
-include .makefiles/release.mk
{% endif -%}
```

This allows users to opt-out of features they don't need.

### Variable Substitution

All `.jinja` files support Copier variable substitution:

```jinja
PROJECT_NAME := {{ project_slug }}
PYTHON_VERSION := {{ python_version }}
```

### Filters

Copier provides built-in Jinja2 filters:

```yaml
project_slug:
  default: "{{ project_name|lower|replace(' ', '-')|replace('_', '-') }}"
```

## Generation Process

When a user runs `copier copy python-project-template my-project`:

1. Copier reads `copier.yml` configuration
2. Prompts user for answers to template questions
3. Processes all `.jinja` files with Jinja2 template engine
4. Removes `.jinja` extensions from output filenames
5. Copies processed files to `my-project/` directory
6. Saves answers to `.copier-answers.yml` for future updates

## Updating Generated Projects

Users can update their projects when the template changes:

```bash
cd my-project
copier update
```

Copier will:
1. Load answers from `.copier-answers.yml`
2. Fetch latest template changes
3. Show diff of proposed changes
4. Allow selective acceptance of updates

## Customization Points

### For Template Maintainers

1. **Add new questions**: Edit `copier.yml`
2. **Add new modules**: Create `.makefiles/new_module.mk` and include in `Makefile.jinja`
3. **Modify defaults**: Update default values in `copier.yml`
4. **Add validators**: Use Copier's validation features in `copier.yml`

### For Generated Projects

1. **Custom targets**: Add to main `Makefile` (not `.jinja`)
2. **Module modifications**: Edit `.makefiles/*.mk` files directly
3. **Configuration**: Modify `pyproject.toml` tool sections
4. **Re-generation**: Run `copier update` to pull template changes

## Design Decisions

### Why Modular Makefiles?

- **Separation of Concerns**: Each module handles one domain
- **Optional Features**: Include only needed modules
- **Maintainability**: 5 small files easier than 1 large file
- **Reusability**: Drop-in modules across projects
- **Clarity**: Find targets by category

### Why Ruff?

- **Speed**: 10-200x faster than traditional tools
- **All-in-One**: Replaces Black + Flake8 + isort + more
- **Single Config**: One `[tool.ruff]` section vs. multiple files
- **Modern**: Built in Rust, actively maintained

### Why Copier?

- **Update Support**: Sync template changes to existing projects
- **Jinja2 Power**: Full template engine capabilities
- **Type Validation**: Built-in answer validation
- **Industry Standard**: Used by major Python projects

## File Size Summary

```
Total template files: 11
Total size: ~8KB

Breakdown:
- common.mk:         1.2KB
- deps.mk:           1.4KB
- quality.mk:        999B
- release.mk:        2.3KB
- testing.mk:        1.2KB
- Makefile.jinja:    ~800B
- pyproject.toml:    ~1.5KB
- README.md.jinja:   ~800B
- .gitignore.jinja:  ~400B
- LICENSE.jinja:     ~1KB
```

## Next Steps

See README.md for:
- Installation instructions
- Usage examples
- Troubleshooting guide
- Contributing guidelines
