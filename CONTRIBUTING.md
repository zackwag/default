# Contributing to default

This is HACS's default repository list — the source HACS reads from to populate its integration/plugin/theme/appdaemon/netdaemon/python_script/template "store". Contributing here almost always means adding, updating, or removing an entry in one of the category files (`integration`, `plugin`, `theme`, `appdaemon`, `netdaemon`, `python_script`, `template`), not writing application code.

If you want to get your own repository added, see the official HACS docs first:

- https://hacs.xyz/docs/publish/start
- https://hacs.xyz/docs/publish/include

## Getting started

```bash
git clone https://github.com/zackwag/default.git
cd default
scripts/setup   # pip install -r requirements.txt
```

## Development

The category files (e.g. `integration`, `plugin`) are plain JSON arrays of `owner/repo` strings and must stay sorted and schema-valid:

```bash
python3 scripts/is_sorted.py     # check the lists are alphabetically sorted
python3 scripts/sort.py          # sort them
jq --raw-output . appdaemon blacklist critical integration netdaemon plugin python_script removed template theme   # validate JSON syntax
```

CI (`.github/workflows/lint.yml`) validates JSON syntax, JSON-schema (`tools/jsonschema/*.schema.json`), and sort order on every push/PR to `master`. CI (`.github/workflows/checks.yml`) additionally runs ownership, HACS, and hassfest validation on PRs labeled for new/renamed/removed repositories.

Note: this repo's default branch is `master`, not `main`.

## Commit messages and pull requests

This repo uses [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, `docs:`, `chore:`, etc.). Pull requests are squash-merged, and the **PR title** becomes the commit on `master` — so PR titles must follow this format. This is enforced automatically by the "Conventional Commits" check.

Direct pushes to `master` are allowed but must also use a Conventional Commits-formatted commit message (validated by the same check).

## Opening a pull request

1. Fork the repo and create a branch off `master`.
2. Make your changes (usually a single-line addition/removal in a category file).
3. Open a pull request with a Conventional Commits-formatted title.
4. Wait for CI to pass — required checks must be green before merge.

## Reporting issues

Use [GitHub Issues](../../issues) for bugs and feature requests.
