---
name: jobflow-license
description: This skill should be used for jobflow license questions and for troubleshooting missing references or Pydantic validation failures, using docs first and source/tests only for unresolved details.
---

# jobflow: License and Failure Triage

## High-Signal Playbook

### Route conditions
- Use this skill for legal-license requests and for high-frequency workflow failure triage (missing references, schema validation).
- Route install/environment breakages to `jobflow-build-and-install`.
- Route runtime backend dispatch details (local vs FireWorks execution policy) to `jobflow-jobflow-managers`.
- Route settings file/store configuration questions to `jobflow-jobflow-settings`.

### Triage questions
- Is the request legal (BSD terms) or runtime failure triage? (`docs/license.rst` vs tutorial docs)
- Do errors include `Could not resolve reference` / missing parent outputs? (`docs/tutorials/9-missing-references.ipynb`)
- Is `on_missing_references` currently `ERROR`, `NONE`, or `PASS`? (`src/jobflow/core/reference.py`, `src/jobflow/core/job.py`)
- Is failure from input/output schema mismatch (`ValidationError`, missing required field, extra field)? (`docs/tutorials/9-pydantic-validation.ipynb`)
- Is config parsing/store loading involved (`JOBFLOW_CONFIG_FILE`, `JOB_STORE`)? (`src/jobflow/settings.py`)

### Canonical workflow
1. If legal request: answer from `docs/license.rst` only.
2. If runtime request: classify error as missing reference, schema validation, or config loading.
3. Reproduce with minimal local run (`run_locally`) before changing backends.
4. Missing references:
   - Keep default `OnMissing.ERROR` for strict mode.
   - Use `OnMissing.NONE` only with explicit `None` handling.
   - Use `OnMissing.PASS` only if downstream can process unresolved references.
5. Validation failures:
   - Define explicit `output_schema` or return typed Pydantic models.
   - Fix required/extra fields based on model constraints.
6. Config failures:
   - Check `JOBFLOW_CONFIG_FILE` path, YAML validity, and `JOB_STORE` format.
7. Confirm fix against equivalent test pattern and rerun minimal flow.

### Minimal working example
```python
from jobflow import Flow, OnMissing, job, run_locally

@job
def maybe_fail(fail=False):
    if fail:
        raise ValueError("boom")
    return 1

@job
def collect(values):
    return sum(v for v in values if v is not None)

j1, j2, j3 = maybe_fail(), maybe_fail(), maybe_fail(fail=True)
collector = collect([j1.output, j2.output, j3.output])
collector.config.on_missing_references = OnMissing.NONE

responses = run_locally(Flow([j1, j2, j3, collector]))
assert responses[collector.uuid][1].output == 2
```

```python
from pydantic import BaseModel
from jobflow import job, run_locally

class AddSchema(BaseModel):
    result: float

@job(output_schema=AddSchema)
def add(a, b):
    return {"result": a + b}

j = add(1, 2)
assert run_locally(j)[j.uuid][1].output.result == 3.0
```

### Pitfalls and fixes
- Missing reference defaults to hard failure. Fix: set `on_missing_references` intentionally per job (`src/jobflow/core/job.py`, `src/jobflow/core/reference.py`).
- `OnMissing.NONE` used without handling `None`. Fix: explicitly guard `None` values in consumer jobs (`docs/tutorials/9-missing-references.ipynb`).
- `OnMissing.PASS` misunderstood as resolved value. Fix: treat returned value as `OutputReference` object (`tests/core/test_reference.py`).
- Output schema mismatch (primitive or wrong keys) raises validation/value errors. Fix: return dict/model matching schema exactly (`tests/core/test_job.py`).
- Missing required schema field fails later than expected. Fix: validate at job boundary via `output_schema`/Pydantic models (`docs/tutorials/9-pydantic-validation.ipynb`).
- Bad/empty config file causes warnings/errors. Fix: repair YAML and path in `JOBFLOW_CONFIG_FILE` (`tests/test_settings.py`, `src/jobflow/settings.py`).
- Circular reference in stored outputs can raise runtime errors. Fix: inspect and break recursive output references (`tests/core/test_store.py`).

### Convergence and validation checks
- Missing-reference mode behaves as intended (`ERROR` fails, `NONE` yields `None`, `PASS` returns reference object).
- Schema-backed jobs pass with valid payloads and fail fast with malformed payloads.
- Equivalent tests can be mapped directly (`tests/core/test_reference.py`, `tests/core/test_job.py`, `tests/core/test_store.py`, `tests/test_settings.py`).
- Legal answers match `docs/license.rst` text and do not rely on inferred terms.

## Scope
- Handle license text lookup and high-impact runtime troubleshooting patterns.
- Keep advice docs-first, then source/tests for behavior confirmation.

## Primary documentation references
- `docs/license.rst`
- `docs/tutorials/9-missing-references.ipynb`
- `docs/tutorials/9-pydantic-validation.ipynb`

## Workflow
- Start with docs.
- Use tests to confirm exact error behavior and edge cases.
- Escalate to source for unresolved implementation detail.
- Cite exact file paths in responses.

## Tutorials and examples
- `docs/tutorials/9-missing-references.ipynb`
- `docs/tutorials/9-pydantic-validation.ipynb`

## Test references
- `tests/core/test_reference.py`
- `tests/core/test_job.py`
- `tests/core/test_store.py`
- `tests/test_settings.py`

## Optional deeper inspection
- `src`

## Source entry points for unresolved issues
- `src/jobflow/core/reference.py` (`OnMissing`, reference resolution semantics)
- `src/jobflow/core/job.py` (`JobConfig.on_missing_references`, `Response.from_job_returns`, `output_schema`)
- `src/jobflow/core/store.py` (`get_output` reference resolution and on-missing behavior)
- `src/jobflow/settings.py` (config file parsing, `JOB_STORE` loading)
- `src/jobflow/managers/local.py` (local error propagation and external references)
- Prefer targeted search: `rg -n "on_missing|output_schema|ValidationError|JOBFLOW_CONFIG_FILE" src tests`.
