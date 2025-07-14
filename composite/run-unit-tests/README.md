# Run Unit Tests with Coverage (Reusable GitHub Action)

This composite GitHub Action runs FastAPI (or any Python) unit tests using [uv](https://github.com/astral-sh/uv) for dependency management and enforces 100% code coverage. It uploads the coverage report as a workflow artifact for PR review.

## Features
- Installs dependencies from `pyproject.toml` using `uv`
- Runs tests with `pytest` via `uv`
- Enforces 100% coverage on changed code
- Uploads `coverage.xml` as a workflow artifact
- Customizable test path and pytest arguments
- Descriptive, grouped logs for easy debugging

## Usage

### 1. Copy the Action to Your Repo (Local Usage)
If you want to use this action locally (recommended for private or internal use):

1. Copy the `composite/run-unit-tests` folder into your repository (e.g., under `.github/actions/` or keep the same path).
2. Reference it in your workflow YAML:

```yaml
jobs:
  unit-tests:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Run Unit Tests with Coverage
        uses: ./composite/run-unit-tests  # or ./.github/actions/run-unit-tests
        with:
          python-version: '3.10'           # Optional, default is 3.10
          test-path: 'tests/'              # Optional, default is 'tests/'
          test-args: '-v'                  # Optional, extra pytest args
```

### 2. Use as a Remote Action (If Published)
If you publish this action to a public repository, you can reference it like:

```yaml
      - name: Run Unit Tests with Coverage
        uses: your-org/repo-name/composite/run-unit-tests@main
        with:
          python-version: '3.10'
          test-path: 'tests/'
          test-args: '-v'
```

### 3. Example Workflow
See `example-1.yml` in this folder for a complete workflow example for PRs to `develop`, `release`, `master`, and `main` branches.

## Inputs
| Name            | Description                                      | Default   |
|-----------------|--------------------------------------------------|-----------|
| python-version  | Python version to use                            | 3.10      |
| test-path       | Path to test(s) to run (file or directory)       | tests/    |
| test-args       | Additional arguments to pass to pytest           | -v        |

## Requirements
- Your project must use `pyproject.toml` for dependencies.
- [uv](https://github.com/astral-sh/uv) will be installed automatically by the action.
- Your tests should be compatible with `pytest` and `pytest-cov`.

## Output
- Fails the workflow if coverage is less than 100%.
- Uploads `coverage.xml` as a workflow artifact for PR review.

---

For questions or improvements, feel free to open an issue or PR! 