---
name: jobflow-index
description: This skill should be used when users ask how to use jobflow and the correct generated documentation skill must be selected before going deeper into source code.
---

# jobflow Skills Index

## Route the request
- Classify the request into one of the generated topic skills listed below.
- Prefer abstract, workflow-level guidance for large scientific packages; do not attempt full function-by-function coverage unless explicitly requested.
- If the request combines multiple concerns (for example install + managers), start with blocking prerequisites (`jobflow-build-and-install`) and then hand off to the behavior skill.

## Generated topic skills
- `jobflow-getting-started`: Getting Started (initial setup, quickstarts, and core concepts)
- `jobflow-examples-and-tutorials`: Examples and Tutorials (worked examples, tutorials, and cookbook usage)
- `jobflow-build-and-install`: Build and Install (build, installation, compilation, and environment setup)
- `jobflow-jobflow-utils`: Jobflow Utils (documentation grouped under the 'jobflow-utils' theme)
- `jobflow-jobflow-managers`: Jobflow Managers (documentation grouped under the 'jobflow-managers' theme)
- `jobflow-license`: License and Failure Triage (license terms, missing references, and validation errors)
- `jobflow-jobflow-settings`: Jobflow Settings (documentation grouped under the 'jobflow-settings' theme)
- `jobflow-advanced-topics`: Advanced Topics (changelog, contributors, contributing guide, and API index pages)

## Fast routing hints
- `install`, `pip`, `extras`, `fireworks setup`, `env`: route to `jobflow-build-and-install`.
- `first flow`, `quickstart`, `job.output`, `JobStore`: route to `jobflow-getting-started`.
- `maker`, `dynamic flow`, `Response`, `tutorial example`: route to `jobflow-examples-and-tutorials`.
- `run_locally`, `flow_to_workflow`, `job_to_firework`, `backend`: route to `jobflow-jobflow-managers`.
- `JOBFLOW_CONFIG_FILE`, `JOB_STORE`, `UID_TYPE`, `LOG_FORMAT`: route to `jobflow-jobflow-settings`.
- `apply_mod`, `find_key`, `itergraph`, `to_mermaid`, `initialize_logger`, `suid`: route to `jobflow-jobflow-utils`.
- `OnMissing`, `output_schema`, `ValidationError`, `license`: route to `jobflow-license`.
- `changelog`, `contributors`, `contributing`, `genindex`: route to `jobflow-advanced-topics`.

## Documentation-first inputs
- `docs`
- `paper`

## Tutorials and examples roots
- `examples`
- `docs/tutorials`

## Test roots for behavior checks
- `tests`

## Escalate only when needed
- Start from topic skill primary references.
- If those references are insufficient, search the topic skill `references/doc_map.md`.
- If documentation still leaves ambiguity, open `references/source_map.md` inside the same topic skill and inspect the suggested source entry points.
- Use targeted symbol search while inspecting source (e.g., `rg -n "<symbol_or_keyword>" src`).

## Simulation-first default
- Prefer runnable local checks before distributed backend advice.
- Use topic-specific validation checkpoints from each selected skill before declaring convergence.

## Source directories for deeper inspection
- `src`
