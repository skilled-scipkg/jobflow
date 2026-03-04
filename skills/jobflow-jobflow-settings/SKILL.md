---
name: jobflow-jobflow-settings
description: This skill should be used when users ask about Jobflow configuration (JOBFLOW_CONFIG_FILE, JOB_STORE, UID_TYPE, logging/directory defaults), with docs-first guidance and source/test escalation only when needed.
---

# jobflow: Jobflow Settings

## High-Signal Playbook

### Route conditions
- Use this skill for `JOBFLOW_CONFIG_FILE`, `JOB_STORE`, `UID_TYPE`, logging format, and directory naming behavior.
- Route package install/extras issues to `jobflow-build-and-install`.
- Route first workflow usage to `jobflow-getting-started`.
- Route reference/schema runtime failures to `jobflow-license`.

### Triage questions
- Which config file is being loaded (`JOBFLOW_CONFIG_FILE` vs `~/.jobflow.yaml`)? (`docs/stores.md`, `src/jobflow/settings.py`)
- Is `JOB_STORE` declared as dict spec, serialized `JobStore`, or path to DB file? (`src/jobflow/settings.py`, `tests/test_settings.py`)
- Do you need only default in-memory behavior or persistent backends? (`docs/stores.md`)
- Are you customizing `UID_TYPE` (`uuid1`, `uuid4`, `ulid`)? (`src/jobflow/utils/uid.py`)
- Are logging/directory format defaults sufficient for your runtime? (`src/jobflow/settings.py`, `src/jobflow/managers/local.py`)

### Canonical workflow
1. Decide config file location and set `JOBFLOW_CONFIG_FILE` if not using default.
2. Define `JOB_STORE` in YAML (or point to serialized DB settings file).
3. Instantiate `JobflowSettings` / use module `SETTINGS` and verify parsed values.
4. Run a small local job using default settings-driven store.
5. Query output from store to confirm persistence behavior.
6. If using custom UID mode, validate generation/timestamp expectations.
7. Lock settings in environment/project docs for reproducibility.

### Minimal working example
```yaml
# ~/.jobflow.yaml or file referenced by JOBFLOW_CONFIG_FILE
JOB_STORE:
  docs_store:
    type: MongoStore
    host: <host-or-uri>
    port: 27017
    database: <database>
    collection_name: <collection>
UID_TYPE: uuid4
LOG_FORMAT: "%(asctime)s %(levelname)s %(message)s"
```

```python
from jobflow import SETTINGS, job, run_locally

@job
def ping(x):
    return x

j = ping("ok")
responses = run_locally(j)  # uses SETTINGS.JOB_STORE by default
assert responses[j.uuid][1].output == "ok"
```

### Pitfalls and fixes
- Config file silently not used due wrong env var/path. Fix: verify `JOBFLOW_CONFIG_FILE` and file existence (`src/jobflow/settings.py`).
- Empty config file only emits warning but no custom settings applied. Fix: populate file or remove it (`tests/test_settings.py`).
- Malformed YAML causes parse failure. Fix: repair format and retest settings initialization (`tests/test_settings.py`).
- Unexpected ephemeral outputs. Fix: default is in-memory docs store unless configured otherwise (`src/jobflow/settings.py`, `docs/stores.md`).
- Unsupported UID type crashes ID creation. Fix: use `uuid1`, `uuid4`, or `ulid` only (`src/jobflow/utils/uid.py`, `tests/utils/test_uid.py`).
- `ulid` mode fails without dependency. Fix: install `python-ulid` / `jobflow[ulid]` (`src/jobflow/utils/uid.py`, `pyproject.toml`).

### Convergence and validation checks
- `SETTINGS.JOB_STORE` resolves to expected store backend after config load.
- A minimal `run_locally` call stores/retrieves output from the intended backend.
- Settings object creation fails fast on malformed config and succeeds on valid variants (`tests/test_settings.py`).
- UID generation mode and timestamp behavior match expectations (`tests/utils/test_uid.py`).

## Scope
- Handle configuration defaults/overrides for stores, logging, IDs, and runtime settings.
- Keep answers docs-first, with test-backed behavior confirmation.

## Primary documentation references
- `docs/jobflow.settings.rst`
- `docs/stores.md`

## Workflow
- Start with docs.
- Confirm behavior with `tests/test_settings.py` and `tests/utils/test_uid.py`.
- Escalate to source when parsing/loading behavior is unclear.
- Cite exact file paths in responses.

## Tutorials and examples
- `docs/tutorials/2-introduction.ipynb`
- `docs/tutorials/1-quickstart.ipynb`

## Test references
- `tests/test_settings.py`
- `tests/utils/test_uid.py`
- `tests/managers/test_local.py`

## Optional deeper inspection
- `src`

## Source entry points for unresolved issues
- `src/jobflow/settings.py` (central settings model and config-file loading)
- `src/jobflow/core/store.py` (`JobStore` formats and loaders)
- `src/jobflow/managers/local.py` (uses settings for logging/directory defaults)
- `src/jobflow/utils/uid.py` (`UID_TYPE` behavior)
- `src/jobflow/__init__.py` (`SETTINGS` initialization)
- Prefer targeted search: `rg -n "CONFIG_FILE|JOB_STORE|UID_TYPE|LOG_FORMAT|DIRECTORY_FORMAT" src tests`.
