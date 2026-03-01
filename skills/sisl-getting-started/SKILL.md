---
name: sisl-getting-started
description: This skill should be used when users ask about getting started in sisl; it prioritizes documentation references and then source inspection only for unresolved details.
---

# sisl: Getting Started

## High-Signal Playbook
### Route conditions
- Use this skill for first install, first-run checks, and first geometry workflow.
- Route deep build/environment internals to `sisl-advanced-topics` (`docs/environment.rst`), while keeping standard install flow here (`docs/quickstart/install.rst`).
- Route modeling and Siesta/TBtrans setup to `sisl-inputs-and-modeling` (`docs/tutorials/tutorial_05.rst`, `docs/api/io/index.rst`).
- Route command-line workflow requests to `sisl-scripts` (`docs/scripts/index.rst`).

### Triage questions
- Which install path is intended: `pip`, `conda`, or editable checkout? (`docs/quickstart/install.rst`)
- Is Python >=3.10 available? (`docs/quickstart/install.rst`)
- Are optional extras needed (`analysis`, `viz`)? (`docs/quickstart/install.rst`)
- Is the goal API-first (`import sisl`) or CLI-first (`sgeom`/`sgrid`/`sdata`)? (`docs/introduction.rst`)
- Is the user validating with the packaged test entrypoint? (`docs/quickstart/install.rst`)

### Canonical workflow
1. Pick an isolated environment (especially for conda) (`docs/quickstart/install.rst`).
2. Install base package, then optional extras if needed (`docs/quickstart/install.rst`).
3. Confirm import/version before deeper steps (`docs/release.rst`, `docs/introduction.rst`).
4. Run `pytest --pyargs sisl` for installation validation (`docs/quickstart/install.rst`).
5. If extended tests are needed, point `SISL_FILES_TESTS` to cloned `sisl-files` (`docs/quickstart/install.rst`).
6. Start with a small geometry creation snippet (`docs/introduction.rst`, `docs/tutorials/tutorial_01.rst`).
7. Escalate to source only after docs in this skill + `references/doc_map.md` are exhausted.

### Minimal working example
```bash
python3 -m pip install sisl
python3 -m pip install "sisl[analysis]"
pytest --pyargs sisl
```

```python
import sisl as si

graphene = si.geom.graphene(1.42).repeat(10, 0).repeat(10, 1)
print(graphene.na, graphene.no)
```

### Pitfalls and fixes
- Conda environments are "clever, but fragile"; use dedicated envs for sisl (`docs/quickstart/install.rst`).
- `pytest` is required for validation; install it explicitly if missing (`docs/quickstart/install.rst`).
- Windows compilation is not CI-covered; pass compiler/CMake options explicitly (`docs/quickstart/install.rst`).
- Compile flags are for debugging/perf analysis, not production defaults (`docs/quickstart/install.rst`).
- If optional plotting/analysis features fail, install the corresponding extras (`docs/quickstart/install.rst`).

### Convergence and validation checks
- `pytest --pyargs sisl` completes without import/runtime errors (`docs/quickstart/install.rst`).
- A minimal geometry object reports expected atom/orbital counts (`docs/tutorials/tutorial_01.rst`).
- Optional workflows only attempted after installing matching extras (`docs/quickstart/install.rst`).
- For version-conditional scripts, gate with `sisl.__version_tuple__` (`docs/release.rst`).

## Scope
- Handle questions about initial setup, quickstarts, and core concepts.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/introduction.rst`
- `docs/quickstart/index.rst`
- `docs/quickstart/overview.rst`
- `docs/quickstart/install.rst`
- `docs/visualization/blender/Getting started.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `docs/quickstart/intro_tutorials/01_geometry.ipynb`
- `docs/quickstart/intro_tutorials/02_geometry_orbitals.ipynb`
- `docs/quickstart/intro_tutorials/03_geometry_sile.ipynb`
- `docs/tutorials/tutorial_01.rst`
- `docs/tutorials/tutorial_02.rst`

## Test references
- `src/sisl/tests`
- `src/sisl/_core/tests`
- `src/sisl/io/tests`
- `src/sisl/geom/tests`

## Optional deeper inspection
- `src`
- `tools`

## Source entry points for unresolved issues
- `pyproject.toml` (install requirements, extras, CLI entry points)
- `src/sisl/__init__.py` (`__version__`, lazy module access, top-level imports)
- `src/sisl/_core/geometry.py` (`Geometry`, `sgeom`)
- `src/sisl/io/sile.py` (`get_sile`, sile registry and dispatch)
- `src/sisl/viz/figure/blender.py` (Blender integration path)
- `src/sisl/_core/tests/test_sgeom.py` and `src/sisl/_core/tests/test_sgrid.py` (CLI behavior checks)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src tools`).
