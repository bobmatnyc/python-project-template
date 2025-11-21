# Python Project Template

A comprehensive Copier-based template for Python projects with a modular, enterprise-grade Makefile system.

## Overview

This template generates Python projects with battle-tested build automation, derived from production codebases with 97+ Make targets and 1,200+ lines of proven automation.

### Key Features

- **Modular Makefile Architecture**: 5 specialized modules (common, quality, testing, deps, release)
- **Ruff-First Quality**: 10-200x faster linting than Black+Flake8+isort
- **Release Automation**: Semantic versioning with one-command publishing
- **Environment-Aware**: Automatic dev/staging/prod detection
- **Dependency Intelligence**: Poetry/uv integration with lockfile management

## Quick Start

### Prerequisites

```bash
# Install Copier
pip install copier
# or
pipx install copier
```

### Generate Project

```bash
# From GitHub (when published)
copier copy gh:YOUR_USERNAME/python-project-template my-project

# From local clone
copier copy /path/to/python-project-template my-project

# Follow interactive prompts
cd my-project
make help
```

### Template Questions

The template will ask:
- **project_name**: Human-readable name (e.g., "My Awesome Project")
- **project_slug**: URL/package name (auto-generated from name)
- **project_description**: One-line summary
- **python_version**: Minimum Python version (3.10-3.13)
- **use_testing**: Include pytest/coverage targets (default: yes)
- **use_release_automation**: Include version bumping/publishing (default: yes)
- **use_docker**: Include Docker targets (default: no)
- **github_username**: Your GitHub username/org
- **author_name**: Your full name
- **author_email**: Your email address

## Generated Structure

```
my-project/
├── Makefile                    # Main Makefile (includes modules)
├── .makefiles/
│   ├── common.mk              # Variables, colors, helpers
│   ├── quality.mk             # lint, format, type-check
│   ├── testing.mk             # test, coverage, benchmarks
│   ├── deps.mk                # install, sync, update deps
│   └── release.mk             # version bumping, publishing
├── pyproject.toml             # Python project config
├── README.md                  # Project README
├── .gitignore                 # Standard Python ignores
└── scripts/                   # Project automation scripts
```

## Makefile Targets

### Development Workflow

```bash
make install        # Install in development mode
make quality        # Run all quality checks (lint + type-check)
make test           # Run test suite with coverage
make lint-fix       # Auto-fix linting issues
```

### Release Management

```bash
make patch          # Bump patch version (0.1.0 -> 0.1.1)
make minor          # Bump minor version (0.1.0 -> 0.2.0)
make major          # Bump major version (0.1.0 -> 1.0.0)
make release        # Full release: quality + test + build + publish
```

### Dependency Management

```bash
make deps-install   # Install dependencies
make deps-update    # Update dependencies
make deps-sync      # Sync lockfile with pyproject.toml
```

## Customization

### Adding Custom Targets

Edit the generated `Makefile`:

```makefile
# At the bottom of Makefile
.PHONY: custom-task
custom-task: ## Run custom project task
	@echo "Running custom automation"
	./scripts/my_script.py
```

### Modifying Module Behavior

Edit `.makefiles/*.mk` files:

```makefile
# .makefiles/quality.mk
lint: ## Run linting (customized)
	ruff check src/ tests/ --fix
	mypy src/ --strict
```

### Updating Template

When the template receives updates:

```bash
cd my-project
copier update
```

Copier will:
- Fetch latest template changes
- Show diff of proposed changes
- Allow selective acceptance of updates

## Design Philosophy

### Modular Makefile Architecture

The template separates concerns into specialized modules:

1. **common.mk**: Shared variables, colors, utility functions
2. **quality.mk**: Code quality (linting, formatting, type-checking)
3. **testing.mk**: Test execution, coverage reporting, benchmarks
4. **deps.mk**: Dependency installation, updates, synchronization
5. **release.mk**: Version management, changelog, publishing

**Benefits**:
- **Maintainability**: Each module has single responsibility
- **Reusability**: Drop-in modules across projects
- **Extensibility**: Add new modules without touching existing ones
- **Clarity**: Find targets by category, not by searching 1,200 lines

### Ruff-First Quality Strategy

Traditional Python tooling:
- Black (formatter) + Flake8 (linter) + isort (imports) + pydocstyle
- **4 tools**, **4 config files**, **slow execution**

This template:
- Ruff (all-in-one)
- **1 tool**, **1 config section**, **10-200x faster**

### Environment-Aware Builds

The template detects environment automatically:

```makefile
# In common.mk
ENV ?= development
ifeq ($(ENV),production)
    BUILD_FLAGS := --optimized --no-dev
else
    BUILD_FLAGS := --dev
endif
```

Override with: `make build ENV=production`

## Advanced Features

### Parallel Test Execution

```bash
make test PYTEST_ARGS="-n auto"  # Auto-detect CPU cores
make test PYTEST_ARGS="-n 4"     # Use 4 workers
```

### Coverage Thresholds

```bash
make test                        # Fail if coverage < 85%
make test COVERAGE_MIN=90        # Custom threshold
```

### Release Workflows

```bash
# Automatic changelog generation
make changelog                   # Generate CHANGELOG.md

# Pre-release testing
make pre-release                 # quality + test + build (no publish)

# Full release pipeline
make release                     # Full workflow with publishing
```

## Examples

### Generate CLI Tool Project

```bash
copier copy gh:YOUR_USERNAME/python-project-template my-cli-tool

# Answer prompts:
# project_name: My CLI Tool
# project_slug: my-cli-tool
# python_version: 3.11
# use_testing: yes
# use_release_automation: yes
# use_docker: no

cd my-cli-tool
make install
make help
```

### Generate Library Project

```bash
copier copy gh:YOUR_USERNAME/python-project-template my-library

# After generation, add to Makefile:
.PHONY: docs
docs: ## Build documentation
	sphinx-build docs/ docs/_build/
```

## Troubleshooting

### Copier Not Found

```bash
pip install --user copier
# or
pipx install copier
```

### Make Targets Not Working

Check that `.makefiles/*.mk` files are included:

```bash
ls -la .makefiles/
# Should show: common.mk, quality.mk, testing.mk, deps.mk, release.mk
```

### Python Version Mismatch

Update `pyproject.toml`:

```toml
[tool.poetry.dependencies]
python = "^3.11"  # Change to your version
```

## Contributing

Improvements welcome! To contribute:

1. Fork this repository
2. Create feature branch: `git checkout -b feature/improvement`
3. Test with: `copier copy . /tmp/test-project`
4. Submit PR with description of changes

### Testing Template Changes

```bash
# Test template generation
copier copy . /tmp/test-generation

# Verify generated project
cd /tmp/test-generation
make help
make quality
make test
```

## License

MIT License - see LICENSE file for details.

## Credits

Based on production Makefiles from:
- **claude-mpm**: 1,205 lines, 97 targets
- Real-world Python projects with years of refinement

## Support

- **Issues**: [GitHub Issues](https://github.com/YOUR_USERNAME/python-project-template/issues)
- **Discussions**: [GitHub Discussions](https://github.com/YOUR_USERNAME/python-project-template/discussions)
- **Documentation**: See `TEMPLATE_README.md` for generated project docs
