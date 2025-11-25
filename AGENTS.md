# Repository Guidelines

## Project Structure & Module Organization

- Core library lives in `blenderproc/` (CLI, API, utilities, dataset loaders, renderers).
- Public API is exposed via `blenderproc/api/*` and points to implementations in `blenderproc/python/*`.
- Examples and small pipelines are under `examples/` (`basics/`, `advanced/`, `datasets/`).
- Tests are in `tests/` (high‑level) and `blenderproc/python/tests/` (test helpers).
- Shared metadata (id mappings, taxonomy files, etc.) is in `blenderproc/resources/` and top‑level `resources/`.

## Build, Test & Development Commands

- Install editable: `pip install -e .` from the repo root.
- Run an example (internal Blender mode):  
  `blenderproc run examples/basics/basic/main.py <camera_file> <scene.obj> <output_dir>`
- Run unit tests (requires CC textures & Haven paths):  
  `BP_CC_MATERIALS_PATH=<cc_path> BP_HAVEN_PATH=<haven_path> python3 cli.py run tests/run_all.py`
- Build docs (from `docs/`, internal Blender env): `./generate.sh`

## Coding Style & Naming Conventions

- Follow the existing Python style and [PEP 8]; use 4‑space indentation and type hints where practical.
- Keep module, class and function names descriptive and lowercase_with_underscores (classes in PascalCase).
- Prefer small, focused modules under `blenderproc/python/*`; re‑export public APIs from `blenderproc/api/*`.

## Testing Guidelines

- Tests use the standard `unittest` framework; name new tests `test_*.py` and classes `UnitTest*`.
- Place high‑level integration tests in `tests/`; helper utilities in `blenderproc/python/tests/`.
- Ensure new features are covered by at least one test or an adapted example.

## Commit & Pull Request Guidelines

- Follow `CONTRIBUTING.md`:
  - Commit messages: `<type>(<scope>): <subject>` (e.g. `feat(loader): add usd support`).
  - Branch names: `iss_<issue-number>_<short_description>`.
- For PRs, link the relevant GitHub issue, describe the change and its impact, and update examples/docs when behavior changes.

## Agent Notes (for AI Assistants)

- When you need an overview, scan in this order: `README.md`, `README_BlenderProc4BOP.md`, `CONTRIBUTING.md`, this file, then `docs/Readme.md` and `docs/tutorials/*.md`.
- For API questions, prefer reading `blenderproc/api/*/__init__.py` first, then follow imports into `blenderproc/python/**` for implementation details.
- Use `rg`/`grep` to locate symbols (`bproc.camera.*`, `bproc.loader.*`, etc.) and confirm behavior before editing.
- Be cautious running tests or examples: many require Blender binaries and large external datasets (CC textures, Haven, BOP, SUNCG, 3D-Front, etc.). If the environment is missing these, explain how to run them instead of forcing execution.
- Keep changes minimal and localized: adjust `blenderproc/python/*` implementations, preserve public API signatures in `blenderproc/api/*` and CLI behavior in `blenderproc/command_line.py`.
- When adding new functionality, consider whether it belongs in an existing utility/loader/renderer module and mirror the usual pattern: implementation under `blenderproc/python/...` plus an API re-export under `blenderproc/api/...`.

### Fork & PR Workflow (Personal Files)

- This fork’s `main` branch may contain local-only helper files such as `AGENTS.md` and `docs/ARCHITECTURE.md`. These are not intended for upstream.
- Always create PR branches from `upstream/main`, not from `main`:
  - `git fetch upstream`
  - `git checkout -b feature/my-change upstream/main`
- Push PR branches to `origin` and open PRs from `feature/*` branches only. Do not open PRs from `main`.
- To keep `main` up to date while preserving personal docs:
  - `git checkout main`
  - `git fetch upstream`
  - `git merge upstream/main` (or `git rebase upstream/main`)
  - `git push origin main`
