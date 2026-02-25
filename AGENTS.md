# AGENTS.md

## Cursor Cloud specific instructions

### Overview

Python CLI tool (AI Model Catalog) that evaluates AI/ML models from GitHub and Hugging Face Hub. Single-package `src` layout, no Docker, no database.

### Running commands

All dev tools (`pytest`, `pylint`, `catalog`, `pre-commit`, `coverage`, `isort`, `black`) install to `~/.local/bin`. Ensure `PATH` includes it:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

### Key commands

See `README.md` "Development" section for full reference. Quick summary:

| Task | Command |
|------|---------|
| Install deps | `pip install -e ".[dev]"` |
| Run tests | `pytest` |
| Run tests with coverage | `pytest --cov=src --cov-report=html` |
| Auto-grader test | `./run test` |
| Lint | `pylint src tests` |
| CLI help | `catalog --help` |
| Analyze HF model | `catalog hf-model --model-id bert-base-uncased` |
| Analyze GitHub repo | `catalog models --owner <owner> --repo <repo>` |

### Non-obvious caveats

- `pylint` exit code 28 is normal -- it means "convention" messages only (duplicate-code warnings). The codebase scores 8.68/10. This is not a failure.
- `pytest.ini` and `pyproject.toml` both define pytest config. `pytest.ini` takes precedence (pytest warns about this).
- No API tokens are required to run tests -- all external calls are mocked.
- `GITHUB_TOKEN` is required for live GitHub repo analysis (`catalog models`). Without it, you get rate-limited or errors.
- `HUGGINGFACE_HUB_TOKEN` is optional for Hugging Face analysis but recommended.
- `PURDUE_GENAI_API_KEY` is optional; LLM-enhanced analysis falls back gracefully to keyword-based methods without it.
- The `./run` script is the auto-grader interface (bash). `./run install` installs deps, `./run test` runs tests with coverage.
