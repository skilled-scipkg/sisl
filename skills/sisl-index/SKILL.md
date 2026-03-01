---
name: sisl-index
description: This skill should be used when users ask how to use sisl and the correct generated documentation skill must be selected before going deeper into source code.
---

# sisl Skills Index

## Route the request
- Install and first-run checks: `sisl-getting-started`.
- API symbol lookup, programmatic IO, visualization API: `sisl-api-and-scripting`.
- Geometry/Hamiltonian/input modeling and Siesta/TBtrans setup: `sisl-inputs-and-modeling`.
- Command-line workflows (`sgeom`/`sgrid`/`sdata`/`stoolbox`): `sisl-scripts`.
- Release-note migration for modern versions: `sisl-release`.
- Legacy changelog history and PR-trace migration: `sisl-changelog`.
- Analysis/post-processing workflows: `sisl-analysis-and-output`.
- Example/notebook-driven learning paths: `sisl-examples-and-tutorials`.
- Parallel/HPC-specific guidance: `sisl-parallel-hpc`.
- Consolidated low-frequency one-doc topics: `sisl-advanced-topics`.

## Generated topic skills
- `sisl-getting-started`: setup, quickstart, initial validation
- `sisl-api-and-scripting`: Python API navigation and scripting patterns
- `sisl-inputs-and-modeling`: geometry/orbital/Hamiltonian/input workflows
- `sisl-scripts`: command-line utilities and operation ordering
- `sisl-release`: release-note interpretation and version gating
- `sisl-changelog`: legacy change history and migration references
- `sisl-analysis-and-output`: downstream analysis, outputs, and plotting workflows
- `sisl-examples-and-tutorials`: examples/notebooks by task goal
- `sisl-parallel-hpc`: MPI/OpenMP/scaling related guidance
- `sisl-advanced-topics`: consolidated one-doc advanced topics

## Docs-first escalation policy
1. Start from the routed skill `SKILL.md` and its primary docs.
2. If still ambiguous, open that skill's `references/doc_map.md`.
3. Escalate to that skill's `references/source_map.md` only when docs are insufficient.
4. Use targeted source search (`rg -n "<symbol_or_keyword>" src tools`) and cite exact paths.

## Documentation-first inputs
- `docs`

## Tutorials and examples roots
- `examples`
- `docs/tutorials`
- `docs/visualization`
- `docs/quickstart/intro_tutorials`

## Test roots for behavior checks
- `src/sisl/tests`
- `src/sisl/_core/tests`
- `src/sisl/geom/tests`
- `src/sisl/io/tests`
- `src/sisl/physics/tests`
- `src/sisl/viz/plots/tests`

## Source directories for deeper inspection
- `src`
- `tools`
