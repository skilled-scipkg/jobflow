# jobflow source map: Getting Started

Generated from source roots:
- `src`

Use this map only after exhausting onboarding docs in `references/doc_map.md`.

## Topic query tokens
- `job`
- `Flow`
- `run_locally`
- `JobStore`
- `SETTINGS`

## Fast source navigation
- `rg -n "from jobflow\.core|run_locally|SETTINGS" src/jobflow/__init__.py`
- `rg -n "def job\(|class Job|class Flow" src/jobflow/core/job.py src/jobflow/core/flow.py`
- `rg -n "class JobStore|get_output" src/jobflow/core/store.py`
- `rg -n "class JobflowSettings|JOB_STORE|JOBFLOW_CONFIG_FILE" src/jobflow/settings.py`

## Suggested source entry points
- `src/jobflow/__init__.py` | top-level imports used by most quickstart snippets
- `src/jobflow/core/job.py` | `@job` and runtime response packaging
- `src/jobflow/core/flow.py` | flow composition and dependency validation
- `src/jobflow/core/store.py` | output retrieval and storage behavior
- `src/jobflow/managers/local.py` | local execution path for first simulations
- `src/jobflow/settings.py` | default settings and config loading
- `src/jobflow/managers/fireworks.py` | optional backend escalation after local validation
- `tests/core/test_job.py` | baseline job behavior checks
- `tests/core/test_flow.py` | flow execution/dependency checks
- `tests/managers/test_local.py` | quick local manager behavior checks
- `tests/test_settings.py` | settings defaults and env/file overrides

## Function-level validation commands
```bash
pytest tests/core/test_job.py tests/core/test_flow.py tests/managers/test_local.py tests/test_settings.py -q
```

## Validation checkpoints
- Quickstart API calls map directly to source definitions.
- Local execution and output retrieval behavior is test-backed.
- Settings defaults and overrides are confirmed before deeper debugging.
