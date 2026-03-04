# jobflow documentation map: Examples and Tutorials

Generated from documentation roots:
- `docs`
- `docs/tutorials`
- `examples`
- `tests`

Use this map for runnable API patterns (`@job`, `Flow`, dynamic responses, makers, references, and schema checks).

## Start here
- `docs/tutorials.rst` (tutorial index)
- `docs/tutorials/1-quickstart.ipynb`
- `docs/tutorials/2-introduction.ipynb`
- `docs/tutorials/3-defining-jobs.ipynb`
- `docs/tutorials/4-creating-flows.ipynb`
- `docs/tutorials/5-dynamic-flows.ipynb`
- `docs/tutorials/6-makers.ipynb`
- `docs/tutorials/7-generalized-makers.ipynb`
- `docs/tutorials/9-missing-references.ipynb`
- `docs/tutorials/9-pydantic-validation.ipynb`

## Simulation startup commands
```bash
pytest tests/core/test_job.py tests/core/test_flow.py tests/core/test_maker.py -q
pytest tests/managers/test_local.py -q
```

## Validation checkpoints
- Tutorial wiring uses `producer_job.output` references correctly.
- Dynamic behavior (`replace`/`addition`/`detour`) is validated against tests.
- Schema and missing-reference behavior is validated before production runs.

## Extended references
- `docs/jobflow.core.rst`
- `examples/websites.txt`
- `tests/core/test_reference.py`
