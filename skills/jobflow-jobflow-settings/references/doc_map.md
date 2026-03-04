# jobflow documentation map: Jobflow Settings

Generated from documentation roots:
- `docs`
- `docs/tutorials`
- `examples`
- `tests`

Use this map for configuration semantics (`JOBFLOW_CONFIG_FILE`, `JOB_STORE`, `UID_TYPE`, logging and directory defaults).

## Start here
- `docs/jobflow.settings.rst` (settings API)
- `docs/stores.md` (store configuration patterns)
- `docs/tutorials/2-introduction.ipynb` (store usage context)

## Simulation startup commands
```bash
pytest tests/test_settings.py tests/utils/test_uid.py -q
```

## Validation checkpoints
- Expected config file is loaded (`JOBFLOW_CONFIG_FILE` or default path).
- `JOB_STORE` parses into intended backend.
- UID mode and timestamp behavior match runtime needs.

## Extended references
- `tests/core/test_store.py`
- `tests/managers/test_local.py`
