---
name: jobflow-getting-started
description: This skill should be used when users ask about getting started in jobflow; it prioritizes documentation references and then source inspection only for unresolved details.
---

# jobflow: Getting Started

## High-Signal Playbook

### Route conditions
- Use this skill for first-run setup, first `Job`/`Flow`, and output inspection.
- Route package/environment breakages to `jobflow-build-and-install`.
- Route backend execution choices (local vs FireWorks launch/queue behavior) to `jobflow-jobflow-managers`.
- Route config-file/store customization to `jobflow-jobflow-settings`.
- Route missing references and schema-validation failures to `jobflow-license`.

### Triage questions
- Are you running local only, or local + FireWorks? (`docs/tutorials/8-fireworks.md`)
- Do you need persistent storage or is in-memory enough for now? (`docs/stores.md`)
- Which Python version/environment are you using? (`README.md`, `docs/install.rst`)
- Are you trying to connect jobs with `job.output` references? (`docs/tutorials/1-quickstart.ipynb`)
- Do you need graph visualization (`draw_graph`) or only execution/output checks? (`docs/tutorials/1-quickstart.ipynb`)

### Canonical workflow
1. Install base package (`pip install jobflow`) and optional extras if needed (`docs/install.rst`).
2. Define one or two `@job` functions and call them to create `Job` objects (`docs/tutorials/1-quickstart.ipynb`).
3. Connect jobs using `job.output` and wrap in `Flow` (`docs/tutorials/1-quickstart.ipynb`, `docs/tutorials/2-introduction.ipynb`).
4. Run with `run_locally`; capture `responses` mapping (`docs/tutorials/1-quickstart.ipynb`).
5. Inspect outputs through `responses[job.uuid][index].output` and (optionally) `JobStore.get_output` (`docs/tutorials/2-introduction.ipynb`).
6. If persistence is needed, set `JOBFLOW_CONFIG_FILE` and configure a Mongo-backed `JOB_STORE` (`docs/stores.md`).
7. For remote/queued execution, convert to FireWorks workflow after local smoke test (`docs/tutorials/8-fireworks.md`, `docs/install_fireworks.md`).

### Minimal working example
```python
from maggma.stores import MemoryStore
from jobflow import Flow, JobStore, job
from jobflow.managers.local import run_locally

@job
def add(a, b):
    return a + b

j1 = add(1, 5)
j2 = add(j1.output, 3)
flow = Flow([j1, j2], output=j2.output)

store = JobStore(MemoryStore())
responses = run_locally(flow, store=store)

assert responses[j2.uuid][1].output == 9
assert store.get_output(j2.uuid) == 9
```

```yaml
# jobflow.yaml (docs/stores.md)
JOB_STORE:
  docs_store:
    type: MongoStore
    host: <host-or-uri>
    port: 27017
    database: <database>
    collection_name: <collection>
```

### Pitfalls and fixes
- `job.output` is a future reference, not computed data. Fix: run the flow before reading concrete values (`docs/tutorials/1-quickstart.ipynb`, `src/jobflow/core/reference.py`).
- Flow output wiring errors (`jobs array does not contain all jobs needed for flow output`). Fix: ensure every referenced producer job is inside the same flow (`src/jobflow/core/flow.py`, `tests/core/test_flow.py`).
- Visualization failures with `draw_graph()`. Fix: install `jobflow[vis]` (`docs/tutorials/1-quickstart.ipynb`, `pyproject.toml`).
- Outputs disappear after process exit with defaults. Fix: switch from default `MemoryStore` to persistent store config (`docs/stores.md`, `src/jobflow/settings.py`).
- External reference failures across separately-run flows. Fix: only enable when intentional using `allow_external_references=True` (`src/jobflow/managers/local.py`, `tests/managers/test_local.py`).

### Convergence and validation checks
- `responses` shape is `{uuid: {index: Response}}` and each expected job UUID appears (`docs/tutorials/1-quickstart.ipynb`).
- `JobStore.get_output(job_uuid)` returns the same terminal value as `responses[job_uuid][index].output` (`docs/tutorials/2-introduction.ipynb`).
- Dependency order is respected (producer completes before consumer) (`src/jobflow/core/flow.py`, `tests/core/test_flow.py`).
- Optional: graph rendering or Mermaid export succeeds for connected jobs (`docs/tutorials/1-quickstart.ipynb`, `src/jobflow/utils/graph.py`).

## Scope
- Handle questions about initial setup, quickstarts, and core concepts.
- Keep responses docs-first and workflow-level unless the user asks for implementation details.

## Primary documentation references
- `README.md`
- `docs/index.rst`
- `docs/install.rst`
- `docs/stores.md`
- `docs/tutorials/1-quickstart.ipynb`
- `docs/tutorials/2-introduction.ipynb`
- `docs/install_fireworks.md`
- `docs/tutorials/8-fireworks.md`

## Workflow
- Start with the docs above.
- If details are missing, inspect `references/doc_map.md`.
- Use tutorials/examples as executable patterns.
- If ambiguity remains, inspect `references/source_map.md` and targeted source/tests.
- Cite exact file paths in responses.

## Tutorials and examples
- `docs/tutorials`
- `examples`

## Test references
- `tests/core/test_flow.py`
- `tests/core/test_job.py`
- `tests/managers/test_local.py`

## Optional deeper inspection
- `src`

## Source entry points for unresolved issues
- `src/jobflow/core/job.py` (decorator behavior, response handling, output schema hooks)
- `src/jobflow/core/flow.py` (dependency graph, output validation, ordering)
- `src/jobflow/core/reference.py` (`OutputReference` semantics and missing-reference behavior)
- `src/jobflow/core/store.py` (`JobStore.get_output`, persistence behavior)
- `src/jobflow/managers/local.py` (`run_locally`, external refs, failure behavior)
- `src/jobflow/settings.py` (default store/config loading)
- `src/jobflow/managers/fireworks.py` (handoff to FireWorks)
- Prefer targeted source search: `rg -n "<symbol_or_keyword>" src tests`.
