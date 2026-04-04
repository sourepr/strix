# AGENTS.md

## Cursor Cloud specific instructions

### Overview

Strike (formerly Strix) is an open-source AI-powered penetration testing agent (Python 3.12+, managed with `uv`). The host-side CLI/TUI orchestrates LLM-driven security agents that run inside a Docker sandbox container.

### Development commands

All standard dev commands are in the `Makefile`. Key targets:

| Command | Purpose |
|---------|---------|
| `make setup-dev` | Install dev deps + pre-commit hooks |
| `make test` | Run pytest (unit tests, no Docker needed) |
| `make lint` | Ruff + pylint |
| `make format` | Ruff format |
| `make type-check` | mypy + pyright |
| `make security` | Bandit security scan |
| `make check-all` | All of the above quality checks |

Run the CLI in dev mode: `uv run strike --target <target>`.

### Environment variables

All env vars use `STRIKE_` prefix (e.g., `STRIKE_LLM`, `STRIKE_SANDBOX_MODE`). Config keys use `strike_` prefix.

### Notes for Cloud agents

- **Docker is NOT available** in the Cloud VM. The `strike` CLI will exit with "DOCKER NOT INSTALLED" when attempting an actual scan. Unit tests do not require Docker.
- **LLM API keys** (`LLM_API_KEY`, `STRIKE_LLM`) are required for actual scans but not for unit tests.
- Pre-existing lint/type-check warnings (ruff, mypy, pyright) exist in the codebase. These are not regressions.
- If `make setup-dev` fails on pre-commit install due to `core.hooksPath`, run `git config --unset-all core.hooksPath` first.
- Disable telemetry with `export STRIKE_TELEMETRY=0` when running the CLI in non-interactive mode.
