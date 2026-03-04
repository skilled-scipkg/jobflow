# jobflow source map: Jobflow Settings

Generated from source roots:
- `src`

Use this map only after exhausting settings docs in `references/doc_map.md`.

## Topic query tokens
- `JOBFLOW_CONFIG_FILE`
- `JOB_STORE`
- `UID_TYPE`
- `LOG_FORMAT`
- `DIRECTORY_FORMAT`

## Fast source navigation
- `rg -n "JOBFLOW_CONFIG_FILE|JOB_STORE|UID_TYPE|LOG_FORMAT|DIRECTORY_FORMAT" src/jobflow/settings.py`
- `rg -n "class JobStore|from_dict|_construct_store" src/jobflow/core/store.py`
- `rg -n "def suid|def get_timestamp_from_uid" src/jobflow/utils/uid.py`

## Suggested source entry points
- `src/jobflow/settings.py` | central settings model and config-file loading
- `src/jobflow/core/store.py` | parsing and construction of `JobStore` backends
- `src/jobflow/utils/uid.py` | UID mode behavior (`uuid1`, `uuid4`, `ulid`)
- `src/jobflow/managers/local.py` | runtime use of log and directory settings
- `src/jobflow/__init__.py` | `SETTINGS` singleton initialization
- `tests/test_settings.py` | config file and env override behavior
- `tests/utils/test_uid.py` | UID behavior and error handling
- `tests/core/test_store.py` | output and reference resolution behavior tied to store config

## Function-level validation commands
```bash
pytest tests/test_settings.py tests/utils/test_uid.py tests/core/test_store.py -q
```

## Validation checkpoints
- Config path and schema assumptions are verified against settings code and tests.
- Store backend behavior is validated using `JobStore` tests.
- UID mode behavior aligns with simulation reproducibility needs.
