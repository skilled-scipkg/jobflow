# jobflow source map: Advanced Topics

Generated from source roots:
- `src`

Use this map only after exhausting `references/doc_map.md` for history/index requests.

## Topic query tokens
- `__version__`
- `__all__`
- `job`
- `Flow`
- `run_locally`
- `flow_to_workflow`

## Fast source navigation
- `rg -n "__version__|__all__|run_locally|flow_to_workflow|job_to_firework" src/jobflow`
- `rg -n "class Job|class Flow|def job\(" src/jobflow/core`
- `rg -n "version" tests/test_version.py`

## Suggested source entry points
- `src/jobflow/__init__.py` | package exports and top-level API routing
- `src/jobflow/_version.py` | version constant source
- `src/jobflow/core/job.py` | `job` decorator, `Job`, `Response`
- `src/jobflow/core/flow.py` | `Flow` construction and dependency rules
- `src/jobflow/managers/local.py` | `run_locally`
- `src/jobflow/managers/fireworks.py` | `flow_to_workflow`, `job_to_firework`
- `tests/test_version.py` | version regression checks

## Function-level validation checkpoints
- Symbol lookup in docs/index maps to an actual source definition.
- Exported API objects in `__init__.py` match expected implementation modules.
- Version answers are consistent between source and tests.
