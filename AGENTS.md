# AGENTS.md

## Project overview

This is a HACS "default repositories" list — the data source HACS reads to populate its integration/plugin/theme/appdaemon/netdaemon/python_script/template store. Content is plain JSON list files plus Python validation/sort tooling; there's no application to build or run. Default branch is `master`.

## Setup

```bash
scripts/setup   # python3 -m pip install -r requirements.txt
```

Python version is pinned via `.python-version` (3.10).

## Build / Run

N/A — this repo has no application, just data files and validation scripts.

## Test

```bash
python3 scripts/is_sorted.py                                       # verify category lists are sorted
jq --raw-output . appdaemon blacklist critical integration netdaemon plugin python_script removed template theme   # JSON syntax check
```

CI additionally validates against `tools/jsonschema/*.schema.json` (`.github/workflows/lint.yml`) and runs ownership/hassfest/HACS-action checks on labeled PRs (`.github/workflows/checks.yml`).

## Repository structure

- `appdaemon`, `blacklist`, `critical`, `integration`, `netdaemon`, `plugin`, `python_script`, `removed`, `template`, `theme` — JSON arrays of `owner/repo` entries, one file per HACS category (`blacklist`/`critical`/`removed` are special-purpose, not a category)
- `scripts/` — Python tooling: `is_sorted.py`, `sort.py`, `setup`, plus `scripts/check/` and `scripts/changed/` used by CI (referenced as `scripts.check.*` / `scripts.changed.*` modules)
- `tools/jsonschema/` — JSON schemas the category files are validated against
- `.github/workflows/` — `checks.yml` (PR label-driven validation for new/renamed/removed repos), `lint.yml` (JSON/schema/sort validation), `stale.yml`, `upload-critical.yml`, `upload-removed.yml`

## Commit and PR conventions

- Commit messages and PR titles must follow [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`, `ci:`, `build:`, `perf:`, `style:`, `revert:`), optionally with a scope, e.g. `fix(api): handle null response`.
- This repo squash-merges pull requests only; the PR title becomes the final commit message on `master`.
- A "Conventional Commits" CI check enforces this on both PR titles and direct-push commit messages.
- Branch protection on `master`: no force-pushes, no branch deletion, required status checks must pass.
