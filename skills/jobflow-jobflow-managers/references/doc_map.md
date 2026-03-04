# jobflow documentation map: Jobflow Managers

Generated from documentation roots:
- `docs`
- `docs/tutorials`
- `examples`
- `tests`

Use this map for backend execution behavior: local manager semantics and FireWorks conversion/dispatch.

## Start here
- `docs/jobflow.managers.rst` (manager API surface)
- `docs/tutorials/1-quickstart.ipynb` (local run baseline)
- `docs/tutorials/8-fireworks.md` (FireWorks workflow path)
- `docs/install_fireworks.md` (FireWorks setup prerequisites)

## Simulation startup commands
```bash
pytest tests/managers/test_local.py -q
pytest tests/managers/test_fireworks.py -q
```

## Validation checkpoints
- Local execution semantics are confirmed before backend conversion.
- FireWorks conversion preserves dependencies and expected job naming.
- Dynamic response behavior is verified with manager tests.

## Extended references
- `tests/core/test_flow.py`
- `tests/core/test_job.py`
