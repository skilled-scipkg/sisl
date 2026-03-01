---
name: sisl-examples-and-tutorials
description: This skill should be used when users ask about examples and tutorials in sisl; it prioritizes documentation references and then source inspection only for unresolved details.
---

# sisl: Examples and Tutorials

## High-Signal Playbook
### Route conditions
- Use this skill when users ask for runnable learning paths, minimal reproductions, or notebook-first onboarding.
- Route production modeling choices to `sisl-inputs-and-modeling`.
- Route API symbol semantics to `sisl-api-and-scripting`.
- Route CLI option ordering to `sisl-scripts`.

### Triage questions
- Does the user need the fastest minimal run (`examples/ex_01.py`) or a notebook workflow?
- Is the target tight-binding setup, IO handling, visualization, or Siesta/TBtrans data?
- Is the environment script-only or Jupyter-enabled?
- Do they need a copy/paste starter or explanation of why the example works?

### Canonical workflow
1. Start from the smallest executable artifact that matches user intent.
2. Run once unchanged to establish a known-good baseline.
3. Change one parameter at a time (cell size, k-grid, coupling, file path).
4. Promote the working snippet into user-specific workflow only after baseline checks pass.
5. If behavior is surprising, inspect linked source entry points and paired tests.

### Minimal working example
```bash
python examples/ex_01.py
python examples/ex_02.py
```

```python
import sisl as si

g = si.geom.graphene(1.42)
H = si.Hamiltonian(g)
H.construct(H.create_construct([0.142, 1.43], [0.0, -2.7]))
print(H.eigh([2.0 / 3, 1.0 / 3, 0.0]))
```

```bash
# Notebook execution path (if Jupyter tooling is installed)
jupyter nbconvert --execute --to notebook --inplace docs/quickstart/intro_tutorials/01_geometry.ipynb
```

### Pitfalls and fixes
- Many notebook examples assume optional dependencies; verify extras before execution.
- `examples/ex_03.py` requires external GULP output (`zz.gout`); do not use it as a first smoke test.
- Large notebook cells can hide state; restart kernel and run all to validate reproducibility.
- Keep modifications isolated; changing geometry + coupling + k-grid together obscures root cause.

### Convergence and validation checks
- Baseline example runs without exceptions before modifications.
- Eigenvalue/shape outputs are finite and stable across repeated runs.
- Notebook execution completes end-to-end without stale-cell dependencies.
- Modified script still matches documented object flow (`Geometry -> Hamiltonian -> analysis`).

## Scope
- Handle questions about worked examples, tutorials, and cookbook usage.
- Prioritize executable, minimal references that can seed real simulations.

## Primary documentation references
- `docs/tutorials/index.rst`
- `docs/quickstart/intro_tutorials/01_geometry.ipynb`
- `docs/quickstart/intro_tutorials/02_geometry_orbitals.ipynb`
- `docs/quickstart/intro_tutorials/03_geometry_sile.ipynb`
- `docs/tutorials/tutorial_01.rst`
- `docs/tutorials/tutorial_02.rst`
- `docs/tutorials/tutorial_04.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use examples/tutorials as executable patterns before source inspection.
- If ambiguity remains after docs/examples, inspect `references/source_map.md`.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `examples/ex_01.py`
- `examples/ex_02.py`
- `examples/ex_03.py`
- `docs/tutorials`
- `docs/visualization/showcase`
- `docs/visualization/basic-tutorials/Demo.ipynb`

## Test references
- `src/sisl/tests`
- `src/sisl/_core/tests/test_geometry.py`
- `src/sisl/physics/tests/test_hamiltonian.py`
- `src/sisl/physics/tests/test_electron.py`
- `src/sisl/viz/plots/tests`

## Optional deeper inspection
- `src`
- `tools`

## Source entry points for unresolved issues
- `examples/ex_01.py`, `examples/ex_02.py` (canonical minimal scripts)
- `src/sisl/geom/special.py` and `src/sisl/_core/geometry.py` (geometry constructors and behavior)
- `src/sisl/physics/hamiltonian.py` and `src/sisl/physics/_ufuncs_hamiltonian.py` (matrix construction/operations)
- `src/sisl/physics/electron.py` (analysis helpers used by tutorial flows)
- `src/sisl/io/sile.py` and `src/sisl/io/siesta/fdf.py` (file-driven tutorial parsing)
- `src/sisl/viz/plots/geometry.py`, `src/sisl/viz/plots/bands.py` (tutorial visualization endpoints)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src tools examples`).
