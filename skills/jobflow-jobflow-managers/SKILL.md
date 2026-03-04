---
name: jobflow-jobflow-managers
description: This skill should be used for job execution manager behavior in jobflow (local execution and FireWorks conversion/dispatch), with docs-first guidance and function-level source/test escalation.
---

# jobflow: Jobflow Managers

## High-Signal Playbook

### Route conditions
- Use this skill for `run_locally`, `flow_to_workflow`, `job_to_firework`, and backend behavior differences.
- Route package install/environment setup to `jobflow-build-and-install`.
- Route first API composition (`@job`, `Flow`, `Maker`) to `jobflow-examples-and-tutorials`.
- Route store/config file defaults to `jobflow-jobflow-settings`.
- Route missing-reference/schema failures to `jobflow-license`.

### Triage questions
- Do you need immediate local simulation, queued FireWorks submission, or both? (`docs/tutorials/1-quickstart.ipynb`, `docs/tutorials/8-fireworks.md`)
- Is the issue in response handling (`replace`/`detour`/`addition`) during local execution? (`src/jobflow/managers/local.py`, `tests/managers/test_local.py`)
- Are parent links and workflow conversion correct in FireWorks? (`src/jobflow/managers/fireworks.py`, `tests/managers/test_fireworks.py`)
- Are missing references expected to fail or pass through? (`src/jobflow/core/reference.py`, `src/jobflow/core/job.py`)
- Do you need custom run directories or logging formats? (`src/jobflow/settings.py`, `src/jobflow/managers/local.py`)

### Canonical workflow
1. Start with a minimal local run using `run_locally` to confirm flow semantics.
2. Enable `ensure_success=True` for strict simulation checks while debugging.
3. Validate dynamic behavior (`replace`, `detour`, `addition`) locally before backend conversion.
4. Convert to FireWorks via `flow_to_workflow` and inspect resulting Firework names/links.
5. Submit only after local and conversion checks pass.
6. Reproduce unexpected behavior against matching manager tests.

### Minimal working example
```python
from jobflow import Flow, job
from jobflow.managers.local import run_locally

@job
def add(a, b):
    return a + b

j1 = add(2, 3)
j2 = add(j1.output, 4)
responses = run_locally(Flow([j1, j2], output=j2.output), ensure_success=True)
assert responses[j2.uuid][1].output == 9
```

```python
from jobflow import Flow, job
from jobflow.managers.fireworks import flow_to_workflow

@job
def add(a, b):
    return a + b

j1 = add(1, 2)
j2 = add(j1.output, 5)
wf = flow_to_workflow(Flow([j1, j2]))
assert len(wf.fws) == 2
```

### Pitfalls and fixes
- Silent partial execution masks failures. Fix: set `ensure_success=True` while validating (`src/jobflow/managers/local.py`, `tests/managers/test_local.py`).
- `replace`/`detour`/`addition` behavior appears surprising. Fix: inspect response handling order in local manager and compare with tests (`src/jobflow/managers/local.py`, `tests/managers/test_local.py`).
- FireWorks conversion fails with unresolved dependencies. Fix: check `job_to_firework` parent mapping and reference constraints (`src/jobflow/managers/fireworks.py`, `tests/managers/test_fireworks.py`).
- External reference usage fails across separately run flows. Fix: use `allow_external_references=True` only when intentionally chaining prior outputs (`src/jobflow/managers/local.py`, `tests/managers/test_local.py`).
- Logging or folder layout not matching expectations. Fix: verify `LOG_FORMAT` and `DIRECTORY_FORMAT` in settings (`src/jobflow/settings.py`, `src/jobflow/managers/local.py`).

### Convergence and validation checks
- Local run returns expected `{uuid: {index: Response}}` mapping with terminal output.
- Dynamic responses create expected additional/replacement/detour jobs.
- FireWorks conversion preserves dependency edges and job names.
- Targeted manager tests pass for the behavior in question (`tests/managers/test_local.py`, `tests/managers/test_fireworks.py`).

## Scope
- Handle manager-level execution behavior (local + FireWorks) and operational triage.
- Keep guidance simulation-first: local deterministic checks before distributed dispatch.

## Primary documentation references
- `docs/jobflow.managers.rst`
- `docs/tutorials/1-quickstart.ipynb`
- `docs/tutorials/8-fireworks.md`
- `docs/install_fireworks.md`

## Workflow
- Start with manager docs and tutorials.
- Validate behavior quickly with minimal local and conversion snippets.
- Escalate to manager tests for expected semantics.
- Escalate to source for unresolved implementation details.
- Cite exact file paths in responses.

## Tutorials and examples
- `docs/tutorials/1-quickstart.ipynb`
- `docs/tutorials/8-fireworks.md`
- `examples`

## Test references
- `tests/managers/test_local.py`
- `tests/managers/test_fireworks.py`
- `tests/core/test_flow.py`
- `tests/core/test_job.py`

## Optional deeper inspection
- `src`

## Source entry points for unresolved issues
- `src/jobflow/managers/local.py` (`run_locally`, response propagation, dynamic branches)
- `src/jobflow/managers/fireworks.py` (`flow_to_workflow`, `job_to_firework`, `JobFiretask`)
- `src/jobflow/core/job.py` (`JobConfig.on_missing_references`, `Response.from_job_returns`)
- `src/jobflow/core/flow.py` (job ordering and dependency graph behavior)
- `src/jobflow/settings.py` (`LOG_FORMAT`, `DIRECTORY_FORMAT`, default store usage)
- Prefer targeted search: `rg -n "run_locally|flow_to_workflow|job_to_firework|ensure_success|allow_external_references" src tests`.
