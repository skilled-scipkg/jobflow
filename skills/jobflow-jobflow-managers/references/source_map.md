# jobflow source map: Jobflow Managers

Generated from source roots:
- `src`

Use this map only after exhausting manager docs in `references/doc_map.md`.

## Topic query tokens
- `run_locally`
- `flow_to_workflow`
- `job_to_firework`
- `allow_external_references`
- `ensure_success`

## Fast source navigation
- `rg -n "def run_locally|allow_external_references|ensure_success" src/jobflow/managers/local.py`
- `rg -n "def flow_to_workflow|def job_to_firework|class JobFiretask" src/jobflow/managers/fireworks.py`
- `rg -n "on_missing_references|Response" src/jobflow/core/job.py src/jobflow/core/reference.py`

## Suggested source entry points
- `src/jobflow/managers/local.py` | local manager execution loop and dynamic response handling
- `src/jobflow/managers/fireworks.py` | conversion of jobs/flows to FireWorks artifacts
- `src/jobflow/core/job.py` | response object semantics consumed by managers
- `src/jobflow/core/flow.py` | dependency graph ordering used during execution
- `src/jobflow/core/reference.py` | missing-reference resolution behavior
- `src/jobflow/settings.py` | log and directory formatting defaults applied at runtime
- `tests/managers/test_local.py` | local manager behavior matrix
- `tests/managers/test_fireworks.py` | FireWorks conversion and integration checks
- `tests/core/test_flow.py` | flow semantics relevant to manager traversal

## Function-level validation commands
```bash
pytest tests/managers/test_local.py tests/managers/test_fireworks.py -q
```

## Validation checkpoints
- Manager behavior claims map to concrete functions in `local.py` or `fireworks.py`.
- Dynamic response semantics (`replace`, `detour`, `addition`) are validated by tests.
- Missing-reference and external-reference modes are explicitly verified.
