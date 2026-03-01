---
name: sisl-inputs-and-modeling
description: This skill should be used when users ask about inputs and modeling in sisl; it prioritizes documentation references and then source inspection only for unresolved details.
---

# sisl: Inputs and Modeling

## High-Signal Playbook
### Route conditions
- Use this skill for geometry construction, orbital/coupling setup, and Siesta/TBtrans IO modeling paths.
- Route first-install and environment issues to `sisl-getting-started` (`docs/quickstart/install.rst`).
- Route pure API symbol lookup to `sisl-api-and-scripting` (`docs/api/index.rst`).
- Route CLI transformations (`sgeom`, `sdata`, `sgrid`) to `sisl-scripts` (`docs/scripts/*.rst`).

### Triage questions
- Are you building from scratch or reading existing files? (`docs/tutorials/tutorial_01.rst`, `docs/api/io/index.rst`)
- Which object is the anchor: `Geometry`, `Hamiltonian`, `Grid`, or code-specific sile?
- Are lattice vectors and periodicity (`nsc`) known and physically consistent? (`docs/tutorials/tutorial_05.rst`)
- Is interaction range encoded in orbital radii and neighbor shells? (`docs/tutorials/tutorial_06.rst`)
- Are you working with Siesta/TBtrans-specific inputs/outputs? (`docs/api/io/siesta.rst`, `docs/api/io/tbtrans.rst`)
- Is system size large enough to require block iteration patterns? (`docs/tutorials/tutorial_06.rst`)

### Canonical workflow
1. Create/inspect geometry (`docs/tutorials/tutorial_01.rst`, `docs/tutorials/tutorial_02.rst`).
2. Set lattice and periodic supercell (`nsc`) consistent with intended couplings (`docs/tutorials/tutorial_05.rst`).
3. Build Hamiltonian and define onsite/hopping terms (`docs/tutorials/tutorial_05.rst`).
4. For robust, scalable assignment use `Geometry.close` and block iteration (`docs/tutorials/tutorial_06.rst`).
5. Use `sisl.get_sile(...)` for code-agnostic IO and explicit specifiers when extensions are ambiguous (`docs/api/io/index.rst`).
6. For generic file-driven workflows, use `sdata`; for explicit geometry/grids, use `sgeom`/`sgrid` (`docs/scripts/sdata.rst`).
7. Escalate to source maps for parser internals or edge-case model behavior.

### Minimal working example
```python
from sisl import Atom, Geometry, Hamiltonian, Lattice

H_atom = Atom(1, R=1.0)
square = Geometry([[0.5, 0.5, 0.0]], H_atom, lattice=Lattice([1, 1, 10], nsc=[3, 3, 1]))
H = Hamiltonian(square)

for ia in square:
    idx_a = square.close(ia, R=[0.1, 1.1])
    H[ia, idx_a[0]] = -4.0
    H[ia, idx_a[1]] = 1.0
```

```bash
sdata RUN.fdf --help
sgeom RUN.fdf RUN.xyz
```

### Pitfalls and fixes
- `Geometry` defaults can hide assumptions (default species/orbital/lattice); set atom/lattice explicitly for production models (`docs/tutorials/tutorial_01.rst`).
- sisl does not enforce Hamiltonian symmetry automatically; assign conjugate/paired terms intentionally (`docs/tutorials/tutorial_05.rst`).
- Naive neighbor loops become too slow for very large systems; use `iter_block()` path (`docs/tutorials/tutorial_06.rst`).
- Ambiguous extensions can pick wrong sile class; force with filename specifier (`docs/api/io/index.rst`).
- CLI operations are order-sensitive when chaining transformations (`docs/scripts/sgeom.rst`, `docs/scripts/sgrid.rst`).

### Convergence and validation checks
- Check `Geometry` summary (`na`, `no`, `nsc`, `maxR`) after construction (`docs/tutorials/tutorial_01.rst`).
- Validate Hamiltonian sparsity and expected non-zero couplings before expensive runs (`docs/tutorials/tutorial_05.rst`).
- For orbital-range workflows, verify neighbor shells returned by `close(...)` match intended model radius (`docs/tutorials/tutorial_06.rst`).
- Round-trip test one representative IO file with `get_sile(...).read_*` for parser sanity (`docs/api/io/index.rst`).

## Scope
- Handle questions about inputs, system setup, models, and physical parameterization.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/tutorials/tutorial_01.rst`
- `docs/tutorials/tutorial_02.rst`
- `docs/tutorials/tutorial_05.rst`
- `docs/tutorials/tutorial_06.rst`
- `docs/api/geom/index.rst`
- `docs/api/io/index.rst`
- `docs/api/io/siesta.rst`
- `docs/api/io/tbtrans.rst`
- `docs/scripts/sgeom.rst`
- `docs/scripts/sdata.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `docs/tutorials/tutorial_01.rst`
- `docs/tutorials/tutorial_02.rst`
- `docs/tutorials/tutorial_05.rst`
- `docs/tutorials/tutorial_06.rst`
- `examples`

## Test references
- `src/sisl/geom/tests`
- `src/sisl/io/siesta/tests`
- `src/sisl/io/tbtrans/tests`
- `src/sisl/tests`

## Optional deeper inspection
- `src`
- `tools`

## Source entry points for unresolved issues
- `src/sisl/_core/geometry.py` and `src/sisl/_core/sparse_geometry.py` (geometry + sparse model behavior)
- `src/sisl/_core/_ufuncs_geometry.py` and `src/sisl/_core/_ufuncs_sparse_geometry.py` (functional dispatch paths)
- `src/sisl/io/sile.py` (format resolution and sile registry)
- `src/sisl/io/siesta/fdf.py` (Siesta input parsing and include semantics)
- `src/sisl/io/tbtrans/tbt.py` (TBtrans data extraction argument model)
- `src/sisl/io/fhiaims/_geometry.py` (FHI-aims geometry mapping)
- `src/sisl_toolbox/siesta/minimizer/_yaml_reader.py` and `_minimize.py` (toolbox-driven model optimization inputs)
- `src/sisl_toolbox/siesta/atom/_atom.py` (atom toolbox input/output helper)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src tools`).
