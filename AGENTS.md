# AGENTS.md
Guidance for autonomous coding agents working in this repository.

## Scope and precedence
- Applies to the full repository rooted at `python-template/`.
- Follow direct user instructions first.
- Then follow repository rules in this file.
- Then follow conventions inferred from existing code.

## Repository snapshot
- Project type: lightweight Python service template.
- Runtime example: FastAPI app in `main.py`.
- Python version: `3.14` (from `.python-version`).
- Dependency metadata: `pyproject.toml`.
- Main tooling: `uv`, `ruff`, `ty`, `pre-commit`, GitHub Actions CI.

## Rules files discovered
- `.cursor/rules/`: not present.
- `.cursorrules`: not present.
- `.github/copilot-instructions.md`: not present.
- If any are added later, treat them as high-priority guidance.

## Environment setup
Use `uv` by default to stay aligned with README and CI.

```bash
# First-time virtualenv setup
uv venv create
source .venv/bin/activate

# Install project + dev dependencies
uv sync --dev
```

## Build, lint, type-check, and test commands
There is no formal packaging/build step yet beyond dependency resolution.

```bash
# CI-compatible dependency install
uv sync --all-extras --dev

# Lint and format
uv run ruff check .
uv run ruff check --fix .
uv run ruff format .

# Type-check
uv run ty check .

# Pre-commit hooks
uv run pre-commit run --all-files
```

## Test execution guidance
Current state:
- No `tests/` directory exists.
- No test runner is configured in `pyproject.toml`.
- CI currently runs lint and type checks only.

When tests are introduced, use `pytest` conventions unless repo direction changes.

```bash
# Run all tests
uv run pytest

# Run a single file
uv run pytest tests/test_example.py

# Run a single test function
uv run pytest tests/test_example.py::test_happy_path

# Run a specific parametrized case
uv run pytest 'tests/test_example.py::test_happy_path[param_value]'
```

If `pytest` is missing:

```bash
uv add --dev pytest
```

## CI parity commands
Use these to mirror GitHub Actions locally:

```bash
uv sync --all-extras --dev
uv run ruff check --output-format=github .
uv run ty check --output-format=github .
```

## Code style guidelines

### General
- Follow PEP 8 and keep changes minimal and focused.
- Prefer explicit, readable code over clever abstractions.
- Keep modules cohesive; avoid unrelated edits.

### Imports
- Group imports: standard library, third-party, local.
- Separate import groups with one blank line.
- Prefer explicit imports; avoid wildcard imports.
- Avoid import-time side effects.

### Formatting and linting
- `ruff format` is the canonical formatter.
- `ruff check` is the lint gate; use `--fix` where safe.
- Let tooling control wrapping/whitespace.
- Do not hand-format against tool output unless necessary.

### Typing
- Add type hints for new and modified functions/methods.
- Prefer built-in generics (`list[str]`, `dict[str, int]`).
- Use `Final` for true constants where helpful.
- Avoid `Any` unless justified.
- Keep `ty check` passing.
- FastAPI handlers should declare explicit return types.

### Naming conventions
- Modules/files: `snake_case`.
- Functions/variables: `snake_case`.
- Classes: `PascalCase`.
- Constants: `UPPER_SNAKE_CASE` (or typed `Final`).
- Future tests: `test_<behavior>`.

### Error handling
- Fail fast with clear exceptions.
- Avoid bare `except:` blocks.
- Catch specific exceptions only when adding useful handling.
- In FastAPI routes, raise meaningful HTTP errors (for example `HTTPException`).
- Include actionable context in error messages.

### FastAPI guidance
- Keep route handlers small and deterministic.
- Use typed request/response models as complexity grows.
- Keep business logic outside decorators/routes where possible.
- Prefer async handlers for I/O-bound work.

### Dependency and config hygiene
- Runtime dependencies belong in `[project.dependencies]`.
- Dev tools belong in `[dependency-groups].dev`.
- Keep changes compatible with Python `3.14`.
- Avoid introducing a second dependency workflow without a clear reason.

### Documentation and comments
- Write comments/docstrings only for non-obvious context.
- Keep README and command docs aligned with actual commands.
- Update this file when workflow/tooling conventions change.

## Pre-commit expectations
Current hooks:
- `ruff` with `--fix`
- `ruff-format`
- `ty check` via `uv run ty check .`

Before opening a PR:

```bash
uv run pre-commit run --all-files
```

## Notes
- No formal test harness is present yet.
- CI currently validates lint and types only.
- Keep this file updated when tooling or workflow changes.
