---
name: jobflow-examples-and-tutorials
description: This skill should be used when users ask about examples and tutorials in jobflow; it prioritizes documentation references and then source inspection only for unresolved details.
---

# jobflow: Examples and Tutorials

## High-Signal Playbook

### Route conditions
- Use this skill for API patterns: `@job`, `Flow`, `OutputReference`, dynamic `Response`, and `Maker` composition.
- Route install/environment issues to `jobflow-build-and-install`.
- Route backend execution/dispatch policy to `jobflow-jobflow-managers`.
- Route config/store defaults to `jobflow-jobflow-settings`.
- Route missing-reference and schema-validation failures to `jobflow-license`.

### Triage questions
- Are you building static flows, dynamic flows (`replace`/`addition`/`detour`), or maker-driven flows? (`docs/tutorials/5-dynamic-flows.ipynb`, `docs/tutorials/6-makers.ipynb`)
- Do downstream jobs consume `job.output` correctly? (`docs/tutorials/3-defining-jobs.ipynb`, `docs/tutorials/4-creating-flows.ipynb`)
- Do you need output schemas (`output_schema`) or plain Python outputs? (`docs/tutorials/9-pydantic-validation.ipynb`)
- Should failed parent outputs be tolerated? (`docs/tutorials/9-missing-references.ipynb`)
- Are you running with `run_locally` first before backend escalation? (`docs/tutorials/1-quickstart.ipynb`)

### Canonical workflow
1. Define small pure functions and decorate with `@job` (`docs/tutorials/3-defining-jobs.ipynb`).
2. Build dependency edges by passing `producer_job.output` into consumers (`docs/tutorials/4-creating-flows.ipynb`).
3. Wrap jobs in `Flow`, optionally set flow output explicitly (`docs/tutorials/2-introduction.ipynb`).
4. Run locally and inspect `responses` and/or `JobStore` outputs (`docs/tutorials/1-quickstart.ipynb`).
5. Introduce dynamic behavior with `Response(replace=...)`, `Response(addition=...)`, or `Response(detour=...)` as needed (`docs/tutorials/5-dynamic-flows.ipynb`).
6. Encapsulate reusable patterns using `Maker` classes and `flow.update_maker_kwargs(...)` (`docs/tutorials/6-makers.ipynb`, `docs/tutorials/7-generalized-makers.ipynb`).
7. Add explicit validation/tolerance controls for production robustness (`docs/tutorials/9-pydantic-validation.ipynb`, `docs/tutorials/9-missing-references.ipynb`).

### Minimal working example
```python
from jobflow import Flow, job
from jobflow.managers.local import run_locally

@job
def add(a, b, c=0):
    return a + b + c

j1 = add(1, 2, c=5)
j2 = add(j1.output, 3)
flow = Flow([j1, j2], output=j2.output)

responses = run_locally(flow)
assert responses[j2.uuid][1].output == 11
```

```python
from dataclasses import dataclass
from jobflow import Maker, Response, job

@dataclass
class AddMaker(Maker):
    name: str = "add"
    inc: int = 1

    @job
    def make(self, x):
        return x + self.inc

@job
def branch(x):
    return Response(addition=AddMaker(inc=2).make(x)) if x < 10 else None
```

### Pitfalls and fixes
- Passing `Job`/`Flow` objects where references are expected causes warnings/confusion. Fix: use `.output` for dependencies (`src/jobflow/core/flow.py`, `tests/core/test_flow.py`).
- Reusing the same `Job` object in multiple flows raises ownership errors. Fix: create fresh jobs per flow (`tests/core/test_flow.py`).
- Invalid `Response` return shape (`Response` mixed with other outputs). Fix: return a single `Response` object (`tests/core/test_job.py`).
- Dynamic flow behavior appears out-of-order unexpectedly. Fix: inspect `replace`/`detour`/`addition` semantics and flow order (`docs/tutorials/5-dynamic-flows.ipynb`, `tests/managers/test_local.py`).
- Maker updates not applied where expected. Fix: use `name_filter`/`class_filter` and check `nested` setting (`src/jobflow/core/maker.py`, `tests/core/test_maker.py`).
- Reference failures from failed parents. Fix: set `on_missing_references` explicitly and handle `None` when using `OnMissing.NONE` (`docs/tutorials/9-missing-references.ipynb`).

### Convergence and validation checks
- Response map contains expected UUIDs and indices after run (`docs/tutorials/1-quickstart.ipynb`).
- Flow graph edges match reference wiring (`src/jobflow/core/flow.py`, `tests/core/test_flow.py`).
- Dynamic branches create expected extra/replacement jobs and outputs (`tests/managers/test_local.py`).
- Maker parameter updates propagate only to intended targets (`tests/core/test_maker.py`).
- Optional schema checks fail fast on malformed outputs (`tests/core/test_job.py`, `docs/tutorials/9-pydantic-validation.ipynb`).

## Scope
- Handle worked examples, tutorials, and cookbook API usage.
- Keep answers implementation-grounded and minimal first, then expand.

## Primary documentation references
- `docs/tutorials.rst`
- `docs/tutorials/1-quickstart.ipynb`
- `docs/tutorials/2-introduction.ipynb`
- `docs/tutorials/3-defining-jobs.ipynb`
- `docs/tutorials/4-creating-flows.ipynb`
- `docs/tutorials/5-dynamic-flows.ipynb`
- `docs/tutorials/6-makers.ipynb`
- `docs/tutorials/7-generalized-makers.ipynb`
- `docs/tutorials/9-missing-references.ipynb`
- `docs/tutorials/9-pydantic-validation.ipynb`
- `examples/websites.txt`

## Workflow
- Start from tutorial notebook(s) matching the requested pattern.
- Cross-check the same behavior in `tests/core` and `tests/managers`.
- Escalate to source only if docs and tests still leave ambiguity.
- Cite exact file paths in responses.

## Tutorials and examples
- `docs/tutorials`
- `examples`

## Test references
- `tests/core/test_job.py`
- `tests/core/test_flow.py`
- `tests/core/test_maker.py`
- `tests/core/test_reference.py`
- `tests/managers/test_local.py`

## Optional deeper inspection
- `src`

## Source entry points for unresolved issues
- `src/jobflow/core/job.py` (`@job`, `Response`, output schema and config propagation)
- `src/jobflow/core/flow.py` (dependency graphing, output compatibility checks)
- `src/jobflow/core/maker.py` (`Maker` update/filter recursion)
- `src/jobflow/core/reference.py` (`OutputReference` behavior and on-missing modes)
- `src/jobflow/managers/local.py` (dynamic execution semantics in local manager)
- `src/jobflow/utils/graph.py` (Mermaid/graph rendering)
- Prefer targeted source search: `rg -n "<job|flow|maker|response|on_missing|output_schema>" src tests`.
