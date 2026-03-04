---
name: jobflow-jobflow-utils
description: This skill should be used for jobflow utility-layer questions (dict_mods, find helpers, graph helpers, logging utilities, UID generation), with docs-first routing and targeted source/test escalation.
---

# jobflow: Jobflow Utils

## High-Signal Playbook

### Route conditions
- Use this skill for utility-layer questions (`dict_mods`, `find`, `graph`, logging helpers, UID generation).
- Route workflow API composition questions to `jobflow-examples-and-tutorials`.
- Route global settings/environment behavior to `jobflow-jobflow-settings`.
- Route manager/backend execution semantics to `jobflow-jobflow-managers`.

### Triage questions
- Are you modifying nested dictionaries (`apply_mod`) or querying them (`find_key` / `find_key_value`)?
- Do you need graph iteration/rendering (`itergraph`, `draw_graph`, `to_mermaid`)?
- Is issue about UUID/ULID mode and timestamp extraction (`suid`, `get_timestamp_from_uid`)?
- Is logging format/handler initialization in scope (`initialize_logger`)?
- Do you need strict behavior references from tests for edge cases?

### Canonical workflow
1. Pick utility module by problem type (`docs/jobflow.utils.rst`).
2. Reproduce with smallest input object/flow.
3. Validate expected behavior against corresponding unit test.
4. Apply fix (dict-mod syntax, graph assumptions, UID mode, etc.).
5. Re-run same minimal check and one integration call from `jobflow` API.

### Minimal working example
```python
from jobflow.utils.dict_mods import apply_mod
from jobflow.utils.find import find_key

doc = {"a": {"b": {"count": 1}}, "tags": ["x"]}
apply_mod({"_inc": {"a->b->count": 2}}, doc)
apply_mod({"_push": {"tags": "y"}}, doc)

assert doc["a"]["b"]["count"] == 3
assert doc["tags"] == ["x", "y"]
assert ["a", "b"] in find_key(doc, "count")
```

```python
from jobflow import Flow, job
from jobflow.utils.graph import to_mermaid

@job
def add(a, b):
    return a + b

j1 = add(1, 2)
j2 = add(j1.output, 3)
flow = Flow([j1, j2])

graph_src = to_mermaid(flow)
assert "flowchart TD" in graph_src
```

### Pitfalls and fixes
- Nested dict updates with `.` path fail. Fix: use `->` path notation in dict mods (`src/jobflow/utils/dict_mods.py`, `tests/utils/test_dict_mods.py`).
- Unsupported dict-mod action raises `ValueError`. Fix: use supported `_set/_inc/_push/...` actions only (`tests/utils/test_dict_mods.py`).
- `_add_to_set`, `_pull`, `_pop` on non-list fields raise errors. Fix: ensure target field is list-like (`tests/utils/test_dict_mods.py`).
- `itergraph` fails for cyclic graphs. Fix: enforce DAG before iteration (`src/jobflow/utils/graph.py`, `tests/utils/test_graph.py`).
- `draw_graph` requires plotting dependencies. Fix: install visualization extras (`tests/utils/test_graph.py`, `pyproject.toml`).
- UUID4 has no timestamp. Fix: use `uuid1` or `ulid` when timestamp extraction is needed (`src/jobflow/utils/uid.py`, `tests/utils/test_uid.py`).
- ULID mode without dependency fails. Fix: install `python-ulid` (`src/jobflow/utils/uid.py`).

### Convergence and validation checks
- Dict-mod transformations match expected post-state exactly (`tests/utils/test_dict_mods.py`).
- `find_key`/`update_in_dictionary` locate and mutate intended nested paths only (`tests/utils/test_find.py`).
- Graph utilities produce deterministic structure for simple connected flows (`tests/utils/test_graph.py`).
- UID mode returns valid IDs and correct timestamp behavior (`tests/utils/test_uid.py`).

## Scope
- Handle utility helper behavior and edge cases across dicts, graphs, IDs, and logging.
- Keep answers anchored to tests for correctness-sensitive questions.

## Primary documentation references
- `docs/jobflow.utils.rst`

## Workflow
- Start with module docs.
- Cross-check with utility tests for exact behavior.
- Escalate to source for unresolved implementation details.
- Cite exact file paths in responses.

## Tutorials and examples
- `docs/tutorials/1-quickstart.ipynb` (graph usage)
- `docs/tutorials/6-makers.ipynb` (dict-mod usage through maker updates)

## Test references
- `tests/utils/test_dict_mods.py`
- `tests/utils/test_find.py`
- `tests/utils/test_graph.py`
- `tests/utils/test_uid.py`

## Optional deeper inspection
- `src`

## Source entry points for unresolved issues
- `src/jobflow/utils/dict_mods.py`
- `src/jobflow/utils/find.py`
- `src/jobflow/utils/graph.py`
- `src/jobflow/utils/log.py`
- `src/jobflow/utils/uid.py`
- `src/jobflow/utils/enum.py`
- Prefer targeted source search: `rg -n "apply_mod|find_key|itergraph|to_mermaid|suid|initialize_logger" src tests`.
