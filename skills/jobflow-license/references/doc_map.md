# jobflow documentation map: License and Failure Triage

Generated from documentation roots:
- `docs`
- `docs/tutorials`
- `examples`
- `tests`

Use this map for BSD license lookup and high-frequency runtime failure triage (missing references, schema validation, config parsing).

## Start here
- `docs/license.rst` (license terms)
- `docs/tutorials/9-missing-references.ipynb` (missing-reference behavior)
- `docs/tutorials/9-pydantic-validation.ipynb` (schema validation behavior)

## Simulation startup commands
```bash
pytest tests/core/test_reference.py tests/core/test_job.py tests/core/test_store.py -q
pytest tests/test_settings.py -q
```

## Validation checkpoints
- Missing-reference mode is explicit (`ERROR`, `NONE`, `PASS`) and tested.
- Output schema behavior is validated against expected failures/successes.
- Config parsing behavior is checked before blaming manager/runtime code.
