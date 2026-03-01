---
name: sisl-analysis-and-output
description: This skill should be used when users ask about analysis and output in sisl; it prioritizes documentation references and then source inspection only for unresolved details.
---

# sisl: Analysis and Output

## High-Signal Playbook
### Route conditions
- Use this skill for post-processing after a model/simulation exists: DOS/PDOS, bands, grids, and visualization outputs.
- Route model construction and coupling setup to `sisl-inputs-and-modeling`.
- Route command-order behavior for `sgrid`/`sdata` to `sisl-scripts`.
- Route first-install/toolchain issues to `sisl-getting-started`.

### Triage questions
- What is the input artifact: `Geometry`, `Hamiltonian`, `Grid`, or file (`.fdf`, `.nc`, `.TBT.nc`, `.cube`)?
- Is the target quantity scalar (`DOS`), orbital-resolved (`PDOS`), k-resolved (bands), or real-space grids?
- Is the output requirement interactive (backend viewer) or reproducible artifact (saved arrays/files/figures)?
- Do we need one-shot diagnostics or a loop over many energies/k-points?

### Canonical workflow
1. Identify the minimal data reader (`sisl.get_sile(...).read_*`) or in-memory object path.
2. Run one low-cost quantity first (small k-mesh, short energy grid).
3. Expand to full analysis only after shape/units/sanity checks pass.
4. For visualization, keep backend and processor assumptions explicit (`plotters`/`processors`).
5. If docs are insufficient, inspect `references/source_map.md` function entry points and matching tests.

### Minimal working example
```python
import numpy as np
import sisl as si
from sisl.physics.electron import DOS

bond = 1.42
g = si.geom.graphene(bond)
H = si.Hamiltonian(g)
for ia in g:
    near = g.close(ia, [0.1 * bond, bond + 0.01])
    H[ia, near[0]] = 0.0
    H[ia, near[1]] = -2.7

bs = si.BandStructure(H, [[0.0, 0.0, 0.0], [2 / 3, 1 / 3, 0.0]], 40)
eig = bs.apply.array.eigh()
E = np.linspace(-5, 5, 400)
dos = DOS(E, eig.ravel())
print(eig.shape, dos.shape)
```

```bash
# File-based grid export path
sgrid Rho.grid.nc --geometry RUN.fdf Rho.cube
```

### Pitfalls and fixes
- DOS/PDOS cost explodes with oversized energy grids; validate on a coarse grid first.
- Plot backend mismatches (`matplotlib`/`plotly`/`blender`) can look like analysis bugs; isolate numeric stage first.
- File polymorphism can choose unexpected sile classes; force specifiers in `get_sile` when ambiguous.
- Large grid exports can dominate runtime/storage; subselect/reduce before writing final artifacts.

### Convergence and validation checks
- Numeric outputs have expected dimensions (`nk`, `nE`, spin/orbital axes).
- `DOS` integrates to physically plausible state counts for the chosen model.
- For grid export, output shape and lattice metadata match input intent.
- At least one related test path from `references/source_map.md` is identified before source-level claims.

## Scope
- Handle questions about output formats, analysis, and post-processing.
- Keep responses docs-first and practical for repeatable simulation analysis.

## Primary documentation references
- `docs/visualization/index.rst`
- `docs/api/viz/index.rst`
- `docs/api/physics.electron.rst`
- `docs/api/physics.brillouinzone.rst`
- `docs/api/io/index.rst`
- `docs/scripts/sgrid.rst`
- `docs/visualization/ase/index.rst`
- `docs/visualization/blender/First animation.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with ranked function-level entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `docs/visualization/basic-tutorials/Demo.ipynb`
- `docs/visualization/combining-plots/Intro to combining plots.ipynb`
- `docs/visualization/showcase/BandsPlot.ipynb`
- `docs/visualization/showcase/PdosPlot.ipynb`
- `docs/visualization/showcase/GridPlot.ipynb`
- `examples`

## Test references
- `src/sisl/viz/plots/tests`
- `src/sisl/viz/processors/tests`
- `src/sisl/viz/data/tests`
- `src/sisl/physics/tests/test_electron.py`
- `src/sisl/physics/tests/test_brillouinzone.py`
- `src/sisl/io/tests/test_cube.py`

## Optional deeper inspection
- `src`
- `tools`

## Source entry points for unresolved issues
- `src/sisl/physics/electron.py` (`DOS`, `PDOS`, eigenstate-level helpers)
- `src/sisl/physics/brillouinzone.py` (`BrillouinZone`, `MonkhorstPack`, apply dispatch)
- `src/sisl/physics/_brillouinzone_apply.py` (parallel/collection internals)
- `src/sisl/viz/plot.py` (high-level plotting entry object)
- `src/sisl/viz/plots/pdos.py`, `src/sisl/viz/plots/bands.py`, `src/sisl/viz/plots/grid.py` (plot classes)
- `src/sisl/viz/plotters/matrix.py`, `src/sisl/viz/plotters/grid.py` (backend plotters)
- `src/sisl/viz/processors/matrix.py`, `src/sisl/viz/processors/grid.py`, `src/sisl/viz/processors/geometry.py` (data processors)
- `src/sisl/io/cube.py`, `src/sisl/io/sile.py` (output serialization + sile dispatch)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src tools`).
