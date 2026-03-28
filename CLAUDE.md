# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Environment Setup

Uses `uv` for environment and dependency management:

```bash
uv venv create
source .venv/bin/activate
uv sync --all-extras --dev
```

## Common Commands

```bash
# Lint
uv run ruff check .
uv run ruff check --fix .   # auto-fix violations

# Format
uv run ruff format .

# Type check
uv run ty check .

# Pre-commit (run before PRs)
uv run pre-commit run --all-files

# Run app
uv run main.py
```

### Tests (when added)

```bash
uv run pytest                                         # all tests
uv run pytest tests/test_example.py                  # single file
uv run pytest tests/test_example.py::test_happy_path # single test
```

### CI parity

```bash
uv sync --all-extras --dev
uv run ruff check --output-format=github .
uv run ty check --output-format=github .
```

## Architecture

Lightweight FastAPI service template. The current codebase is minimal by design — `main.py` is a single FastAPI app with one `GET /` endpoint. The runtime dependencies (NumPy, Polars, Plotly) are included for data-processing use cases.

**Toolchain**: Python 3.14+, uv, ruff (linting + formatting), ty (type checking), pre-commit, Renovate (dependency updates).

## Code Style

- **Formatter**: `ruff format` is canonical — don't manually adjust what it enforces.
- **Imports**: stdlib → third-party → local.
- **Type hints**: required on new/modified functions; use built-in generics (`list[str]`, not `List[str]`).
- **Naming**: `snake_case` for modules/functions, `PascalCase` for classes, `UPPER_SNAKE_CASE` for constants.
- **FastAPI handlers**: must declare explicit return types.
- **Error handling**: fail fast, catch specific exceptions only.
