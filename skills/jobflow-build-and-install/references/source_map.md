# jobflow source map: Build and Install

Generated from source roots:
- `src`

Use this map only after exhausting `references/doc_map.md` install docs.

## Topic query tokens
- `JOBFLOW_CONFIG_FILE`
- `JOB_STORE`
- `UID_TYPE`
- `run_locally`
- `flow_to_workflow`

## Fast source navigation
- `rg -n "JOBFLOW_CONFIG_FILE|JOB_STORE|UID_TYPE" src/jobflow/settings.py src/jobflow/utils/uid.py`
- `rg -n "def run_locally|def flow_to_workflow|def job_to_firework" src/jobflow/managers`
- `rg -n "python-ulid|fireworks|extras" pyproject.toml`

## Suggested source entry points
- `pyproject.toml` | dependency groups/extras (`fireworks`, `vis`, `tests`, `docs`, `ulid`)
- `src/jobflow/settings.py` | settings model and config file loading
- `src/jobflow/utils/uid.py` | UID/ULID behavior and dependency requirements
- `src/jobflow/managers/local.py` | local smoke-test execution path
- `src/jobflow/managers/fireworks.py` | FireWorks integration path and runtime expectations
- `tests/test_settings.py` | config parsing and environment variable behavior
- `tests/managers/test_local.py` | local manager smoke/regression patterns
- `tests/managers/test_fireworks.py` | FireWorks conversion behavior checks

## Function-level validation commands
```bash
pytest tests/test_settings.py tests/managers/test_local.py -q
pytest tests/managers/test_fireworks.py -q
```

## Validation checkpoints
- Extras/config assumptions in docs are consistent with `pyproject.toml` and `settings.py`.
- Local manager smoke tests pass before FireWorks-specific troubleshooting.
- FireWorks conversion behavior is backed by manager tests.
