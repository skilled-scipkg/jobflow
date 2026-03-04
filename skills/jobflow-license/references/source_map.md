# jobflow source map: License and Failure Triage

Generated from source roots:
- `src`

Use this map only after exhausting `references/doc_map.md` and only for runtime triage (not license terms).

## Topic query tokens
- `OnMissing`
- `OutputReference`
- `output_schema`
- `Response.from_job_returns`
- `JOBFLOW_CONFIG_FILE`

## Fast source navigation
- `rg -n "class OnMissing|class OutputReference|def resolve_references" src/jobflow/core/reference.py`
- `rg -n "on_missing_references|output_schema|class Response|from_job_returns" src/jobflow/core/job.py`
- `rg -n "def get_output|on_missing" src/jobflow/core/store.py`
- `rg -n "JOBFLOW_CONFIG_FILE|JOB_STORE" src/jobflow/settings.py`

## Suggested source entry points
- `src/jobflow/core/reference.py` | missing-reference modes and resolution semantics
- `src/jobflow/core/job.py` | job config, schema application, and response packaging
- `src/jobflow/core/store.py` | reference resolution through stored outputs
- `src/jobflow/settings.py` | config parsing and store loading behavior
- `src/jobflow/managers/local.py` | local runtime error propagation and handling
- `tests/core/test_reference.py` | explicit `OnMissing` behavior checks
- `tests/core/test_job.py` | schema and response behavior checks
- `tests/core/test_store.py` | store/reference edge cases
- `tests/test_settings.py` | malformed/missing config handling

## Function-level validation commands
```bash
pytest tests/core/test_reference.py tests/core/test_job.py tests/core/test_store.py tests/test_settings.py -q
```

## Validation checkpoints
- License answers are doc-only from `docs/license.rst`.
- Runtime triage advice maps to concrete functions and test assertions.
- Missing-reference and schema behavior claims are validated in tests.
