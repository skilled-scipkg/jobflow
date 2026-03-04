---
name: jobflow-build-and-install
description: This skill should be used when users ask about build and install in jobflow; it prioritizes documentation references and then source inspection only for unresolved details.
---

# jobflow: Build and Install

## High-Signal Playbook

### Route conditions
- Use this skill for installation, extras selection, editable installs, and FireWorks setup prerequisites.
- Route first workflow authoring/execution to `jobflow-getting-started`.
- Route local-vs-FireWorks runtime behavior to `jobflow-jobflow-managers`.
- Route store/config-file semantics to `jobflow-jobflow-settings`.

### Triage questions
- Are you installing from PyPI or from local source? (`docs/install.rst`)
- Do you need optional extras: `fireworks`, `vis`, `tests`, `docs`, `ulid`? (`docs/install.rst`, `pyproject.toml`)
- Is Python version >= 3.10? (`README.md`, `pyproject.toml`)
- Are you preparing local runs only or FireWorks dispatch/queue runs? (`docs/install_fireworks.md`)
- Do you need editable/dev install and test/docs tooling? (`docs/install.rst`)

### Canonical workflow
1. Create/activate clean Python environment with Python 3.10+.
2. Install baseline package (`pip install jobflow`) (`docs/install.rst`).
3. Add needed extras (`pip install jobflow[fireworks]`, `jobflow[vis]`, etc.) (`docs/install.rst`, `pyproject.toml`).
4. For source install: `git clone`, `pip install .` or `pip install -e .` (`docs/install.rst`).
5. Run a smoke test: import + one `run_locally` execution.
6. If using FireWorks, install/configure FireWorks YAML files and `FW_CONFIG_FILE` (`docs/install_fireworks.md`).
7. Validate setup with `pytest` (when relevant) and/or FireWorks launchpad connectivity (`docs/install.rst`, `docs/install_fireworks.md`).

### Minimal working example
```bash
python -m venv .venv
source .venv/bin/activate

pip install -U pip
pip install jobflow
pip install jobflow[vis]
pip install jobflow[fireworks]

python - <<'PY'
from jobflow import job, run_locally

@job
def f(x):
    return x + 1

j = f(1)
print(run_locally(j)[j.uuid][1].output)
PY
```

```bash
# Source/developer path (docs/install.rst)
git clone https://github.com/materialsproject/jobflow.git
cd jobflow
pip install -e .[tests]
pytest
```

### Pitfalls and fixes
- Missing `matplotlib`/`pydot` when plotting flows. Fix: install `jobflow[vis]` (`docs/install.rst`, `pyproject.toml`).
- FireWorks conversion/import errors. Fix: install `jobflow[fireworks]` (or `fireworks`) (`docs/install.rst`, `docs/install_fireworks.md`).
- ULID ID mode fails due missing dependency. Fix: install `jobflow[ulid]` or `python-ulid` (`src/jobflow/utils/uid.py`, `pyproject.toml`).
- FireWorks launch commands fail due config path. Fix: export `FW_CONFIG_FILE` to your `FW_config.yaml` (`docs/install_fireworks.md`).
- Accidental data loss during setup checks. Fix: only run `lpad reset` for new/throwaway launchpads (`docs/install_fireworks.md`).
- Broken settings parse after install. Fix: validate YAML and file path used by `JOBFLOW_CONFIG_FILE` (`src/jobflow/settings.py`, `tests/test_settings.py`).

### Convergence and validation checks
- `python -c "import jobflow; print(jobflow.__version__)"` works.
- Minimal local job returns expected output via `run_locally`.
- Optional extras are importable (`fireworks`, `matplotlib`/`pydot`) if requested.
- FireWorks launchpad connectivity check succeeds before production submission (`docs/install_fireworks.md`).
- For source installs, targeted tests pass (`tests/test_settings.py`, `tests/managers/test_local.py`).

## Scope
- Handle package installation, extras, source installs, and setup sanity checks.
- Keep guidance docs-first; source/tests only for unresolved behavior questions.

## Primary documentation references
- `docs/install.rst`
- `docs/install_fireworks.md`
- `README.md`
- `pyproject.toml`

## Workflow
- Start with install docs.
- Use `pyproject.toml` for exact optional dependency groups.
- Escalate to tests or source only when install behavior is unclear.
- Cite exact file paths in responses.

## Tutorials and examples
- `docs/tutorials/1-quickstart.ipynb`
- `docs/tutorials/8-fireworks.md`

## Test references
- `tests/test_settings.py`
- `tests/managers/test_local.py`
- `tests/managers/test_fireworks.py`

## Optional deeper inspection
- `src`

## Source entry points for unresolved issues
- `pyproject.toml` (canonical extras and dependency groups)
- `src/jobflow/settings.py` (config/env loading and parsing)
- `src/jobflow/managers/fireworks.py` (FireWorks integration surface)
- `src/jobflow/managers/local.py` (local smoke-test behavior)
- `src/jobflow/utils/uid.py` (UID/ULID dependency behavior)
- `tests/test_settings.py` (real config-file edge cases)
- `tests/managers/test_fireworks.py` (conversion and spec behavior)
- Prefer targeted search: `rg -n "<install|fireworks|settings|uid>" src tests pyproject.toml`.
