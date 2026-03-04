# jobflow source map: Examples and Tutorials

Generated from source roots:
- `src`

Use this map only after exhausting tutorial docs in `references/doc_map.md`.

## Topic query tokens
- `@job`
- `Flow`
- `Response`
- `Maker`
- `OutputReference`
- `run_locally`

## Fast source navigation
- `rg -n "def job\(|class Job|class Response|def apply_schema" src/jobflow/core/job.py`
- `rg -n "class Flow|def flow\(|def get_flow" src/jobflow/core/flow.py`
- `rg -n "class Maker|update_kwargs|recursive_call" src/jobflow/core/maker.py`
- `rg -n "class OutputReference|def resolve_references|OnMissing" src/jobflow/core/reference.py`
- `rg -n "def run_locally" src/jobflow/managers/local.py`

## Suggested source entry points
- `src/jobflow/core/job.py` | `@job`, `Job.run`, `Response.from_job_returns`, schema application
- `src/jobflow/core/flow.py` | flow graph construction, output validation, decorators
- `src/jobflow/core/maker.py` | maker update/filter recursion semantics
- `src/jobflow/core/reference.py` | output reference resolution and missing-reference modes
- `src/jobflow/managers/local.py` | local execution semantics for dynamic flows
- `src/jobflow/utils/graph.py` | graph rendering helpers for tutorial visualization
- `tests/core/test_job.py` | job/response/schema behavior patterns
- `tests/core/test_flow.py` | dependency and flow behavior checks
- `tests/core/test_maker.py` | maker update behavior checks
- `tests/core/test_reference.py` | reference resolution edge cases
- `tests/managers/test_local.py` | dynamic local execution behavior

## Function-level validation commands
```bash
pytest tests/core/test_job.py tests/core/test_flow.py tests/core/test_maker.py tests/core/test_reference.py -q
pytest tests/managers/test_local.py -q
```

## Validation checkpoints
- Example behavior maps to concrete functions/classes in `src/jobflow/core`.
- Dynamic response modes are verified against manager tests.
- Output schema and missing-reference claims are test-backed.
