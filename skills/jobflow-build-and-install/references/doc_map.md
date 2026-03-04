# jobflow documentation map: Build and Install

Generated from documentation roots:
- `docs`
- `docs/tutorials`
- `examples`
- `tests`

Use this map for environment setup, package extras, source installs, and first execution smoke checks.

## Start here
- `docs/install.rst` (pip/source install and extras guidance)
- `docs/install_fireworks.md` (FireWorks installation and configuration)
- `README.md` (supported Python version and package overview)
- `pyproject.toml` (authoritative dependency and extras groups)

## Simulation startup commands
```bash
python -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install -e .[tests]
python -c "import jobflow; print(jobflow.__version__)"
pytest tests/test_settings.py tests/managers/test_local.py -q
```

## Validation checkpoints
- `import jobflow` succeeds in the target environment.
- Required extras (`fireworks`, `vis`, `ulid`) are installed only when needed.
- Local smoke tests pass before FireWorks submission.

## Extended references
- `docs/tutorials/1-quickstart.ipynb`
- `docs/tutorials/8-fireworks.md`
- `tests/managers/test_fireworks.py`
