---
name: jobflow-advanced-topics
description: This skill should be used for low-frequency meta topics in jobflow (release history, contributors, contribution policy, and API index navigation) after routing away from core simulation workflows.
---

# jobflow: Advanced Topics

## High-Signal Playbook

### Route conditions
- Use this skill for changelog, contributors, contribution process, and API index navigation.
- Route installation and environment setup to `jobflow-build-and-install`.
- Route first runnable simulations and API composition to `jobflow-getting-started` or `jobflow-examples-and-tutorials`.
- Route backend execution and dispatch behavior to `jobflow-jobflow-managers`.
- Route settings-store defaults to `jobflow-jobflow-settings`.

### Triage questions
- Is the question historical (release/provenance) or behavioral (runtime/API function semantics)?
- If historical: which exact release/version/date or contributor/process file is needed?
- If API index lookup: which symbol/module should be opened next for function-level checks?
- If runtime semantics are requested, should the request be rerouted to a core workflow skill?

### Canonical workflow
1. Answer metadata/history requests directly from docs.
2. For API index lookups, identify the exact module path and function/class name.
3. Open targeted source entry points only when a concrete symbol-level question remains.
4. If behavior verification is requested, map directly to tests and reroute to the corresponding core skill.

### Minimal working commands
```bash
# quick doc lookup
rg -n "changelog|contributors|contributing|jobflow\\.managers|jobflow\\.core" docs

# targeted symbol lookup from API index question
rg -n "def run_locally|def flow_to_workflow|class Job|class Flow" src/jobflow
```

### Validation checkpoints
- Exact file path cited for every release/process statement.
- API index question results in concrete symbol location (for example `src/jobflow/core/job.py`) and, when relevant, matching test location (for example `tests/core/test_job.py`).
- No speculative behavioral claims without source or test confirmation.

## Scope
- Keep this skill lightweight and routing-oriented.
- Do not replace core simulation workflow skills for execution/debugging details.

## Primary documentation references
- `docs/changelog.rst`
- `docs/contributors.rst`
- `docs/contributing.rst`
- `docs/genindex.rst`
- `docs/jobflow.rst`

## Workflow
- Start with the primary docs above.
- Use `docs/genindex.rst` and `docs/jobflow.rst` to find module/symbol entry points.
- Escalate to source or tests only for concrete unresolved symbol behavior.
- Cite exact file paths in responses.

## Tutorials and examples
- `docs/tutorials`
- `examples`

## Test references
- `tests/test_version.py`
- `tests/core`
- `tests/managers`

## Optional deeper inspection
- `src`

## Source entry points for unresolved issues
- `src/jobflow/__init__.py` (public API exports and `SETTINGS` singleton wiring)
- `src/jobflow/_version.py` (version source used by package metadata/tests)
- `src/jobflow/core/job.py` (`job` decorator, `Job`, `Response`)
- `src/jobflow/core/flow.py` (`Flow`, `flow` decorator, dependency handling)
- `src/jobflow/managers/local.py` (`run_locally`)
- `src/jobflow/managers/fireworks.py` (`flow_to_workflow`, `job_to_firework`)
- Prefer targeted source search: `rg -n "<symbol_or_keyword>" src tests`.
