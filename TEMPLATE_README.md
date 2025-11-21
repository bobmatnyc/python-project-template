# Python Project Template

This is a Copier-based template for Python projects with a comprehensive, modular Makefile system.

## Features

- **Modular Makefile**: Split into quality, testing, dependencies, and release components
- **Ruff-only linting**: 10-200x faster than Black+Flake8+isort
- **ENV-aware builds**: Automatic environment detection (development/staging/production)
- **Release automation**: Semantic versioning with automated publishing
- **Dependency management**: Poetry/uv integration with lock file management

## Quick Start

### Using Copier

```bash
# Install Copier
pip install copier
# or
pipx install copier

# Generate new project
copier copy gh:YOUR_USERNAME/python-project-template my-new-project

# Navigate and start working
cd my-new-project
make help
```

### Updating Template

When the template receives updates:

```bash
cd my-project
copier update
```

## Makefile Targets

See `make help` in generated project for all available targets.

Key targets:
- `make quality` - Run all quality checks (lint, type-check)
- `make test` - Run test suite
- `make install` - Install project in development mode
- `make patch` - Bump patch version and release

## Template Structure

```
python-project-template/
├── copier.yml              # Template configuration
├── TEMPLATE_README.md      # This file
└── template/
    ├── .makefiles/         # Modular Makefile components
    │   ├── common.mk       # Core variables & helpers
    │   ├── quality.mk      # Linting, formatting, type-checking
    │   ├── testing.mk      # Test execution, coverage
    │   ├── deps.mk         # Dependency management
    │   └── release.mk      # Version management, publishing
    ├── Makefile.jinja      # Main Makefile template
    └── scripts/            # Project scripts
```

## Customization

After generating a project, you can:

1. Add custom targets to `Makefile`
2. Modify `.makefiles/*.mk` files for project-specific needs
3. Update `copier.yml` answers with `copier update`

## Contributing

Improvements to this template are welcome! Please submit issues or PRs.

## License

This template is released under the MIT License.
