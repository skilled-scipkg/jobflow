# jobflow documentation map: Getting Started

Generated from documentation roots:
- `docs`
- `docs/tutorials`
- `examples`
- `tests`

Use this map for first-run onboarding, initial `Job`/`Flow` execution, and store/output inspection.

## Start here
- `README.md` (quick package orientation)
- `docs/index.rst` (documentation entry point)
- `docs/install.rst` (installation basics)
- `docs/stores.md` (in-memory vs persistent stores)
- `docs/tutorials/1-quickstart.ipynb`
- `docs/tutorials/2-introduction.ipynb`

## Optional next-step references
- `docs/install_fireworks.md`
- `docs/tutorials/8-fireworks.md`
- `paper/paper.md`

## Simulation startup commands
```bash
python - <<'PY'
from jobflow import Flow, job, run_locally

@job
def add(a, b):
    return a + b

j1 = add(1, 2)
j2 = add(j1.output, 3)
responses = run_locally(Flow([j1, j2], output=j2.output), ensure_success=True)
print(responses[j2.uuid][1].output)
PY
```

## Validation checkpoints
- Local execution returns expected terminal output.
- `responses` and optional `JobStore.get_output` return consistent values.
- FireWorks setup is attempted only after local checks pass.
