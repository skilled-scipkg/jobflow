# jobflow documentation map: Jobflow Utils

Generated from documentation roots:
- `docs`
- `docs/tutorials`
- `examples`
- `tests`

Use this map for utility behavior (`dict_mods`, nested find/update helpers, graph helpers, logging, UID generation).

## Start here
- `docs/jobflow.utils.rst` (utility module index)
- `docs/tutorials/1-quickstart.ipynb` (graph rendering context)
- `docs/tutorials/6-makers.ipynb` (dict-mod patterns via maker updates)

## Simulation startup commands
```bash
pytest tests/utils/test_dict_mods.py tests/utils/test_find.py tests/utils/test_graph.py -q
pytest tests/utils/test_uid.py tests/utils/test_log.py tests/utils/test_uuid.py -q
```

## Validation checkpoints
- Dict modifications and nested lookups match expected post-state.
- Graph traversal/rendering works on simple DAG flows.
- UID/log helpers behave as expected across supported modes.
