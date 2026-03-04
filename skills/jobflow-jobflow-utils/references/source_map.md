# jobflow source map: Jobflow Utils

Generated from source roots:
- `src`

Use this map only after exhausting utility docs in `references/doc_map.md`.

## Topic query tokens
- `apply_mod`
- `find_key`
- `itergraph`
- `to_mermaid`
- `initialize_logger`
- `suid`

## Fast source navigation
- `rg -n "class DictMods|def apply_mod" src/jobflow/utils/dict_mods.py`
- `rg -n "def find_key|def find_key_value|def update_in_dictionary" src/jobflow/utils/find.py`
- `rg -n "def itergraph|def draw_graph|def to_mermaid" src/jobflow/utils/graph.py`
- `rg -n "def initialize_logger" src/jobflow/utils/log.py`
- `rg -n "def suid|def get_timestamp_from_uid" src/jobflow/utils/uid.py src/jobflow/utils/uuid.py`

## Suggested source entry points
- `src/jobflow/utils/dict_mods.py` | dict modification operations and error semantics
- `src/jobflow/utils/find.py` | nested key/value discovery and targeted updates
- `src/jobflow/utils/graph.py` | graph traversal and visualization generation
- `src/jobflow/utils/log.py` | logger initialization behavior
- `src/jobflow/utils/uid.py` | configurable UUID/ULID generation and timestamps
- `src/jobflow/utils/uuid.py` | simple UUID helper used in tests and backwards compatibility paths
- `tests/utils/test_dict_mods.py` | dict-mod behavior matrix
- `tests/utils/test_find.py` | nested find/update behavior checks
- `tests/utils/test_graph.py` | graph traversal/rendering checks
- `tests/utils/test_log.py` | logger initialization checks
- `tests/utils/test_uid.py` | UID mode/timestamp behavior checks
- `tests/utils/test_uuid.py` | UUID helper validation

## Function-level validation commands
```bash
pytest tests/utils/test_dict_mods.py tests/utils/test_find.py tests/utils/test_graph.py tests/utils/test_log.py tests/utils/test_uid.py tests/utils/test_uuid.py -q
```

## Validation checkpoints
- Utility behavior claims are backed by the corresponding utility tests.
- Graph and dict-mod examples can be reproduced with minimal inputs.
- UID/logging semantics are validated before simulation scaling.
