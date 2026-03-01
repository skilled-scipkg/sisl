---
name: sisl-api-and-scripting
description: This skill should be used when users ask about api and scripting in sisl; it prioritizes documentation references and then source inspection only for unresolved details.
---

# sisl: API and Scripting

## High-Signal Playbook
### Route conditions
- Use this skill for Python API navigation, module/symbol lookup, and programmatic IO/physics/viz usage.
- Route command-order and CLI flag behavior to `sisl-scripts` (`docs/scripts/*.rst`).
- Route model-building recipes (geometry/Hamiltonian setup) to `sisl-inputs-and-modeling` (`docs/tutorials/tutorial_05.rst`, `docs/tutorials/tutorial_06.rst`).
- Route first-install issues to `sisl-getting-started` (`docs/quickstart/install.rst`).

### Triage questions
- Which exact symbol/class is needed, and in which submodule? (`docs/api/index.rst`)
- Is the workflow basic objects, physics functions, IO, or visualization? (`docs/api/basic.rst`, `docs/api/physics.rst`, `docs/api/io/index.rst`, `docs/api/viz/index.rst`)
- Is conversion/dispatch behavior involved (`ret_sisl`, external classes)? (`docs/api/core.rst`)
- Is file type detection failing due to ambiguous suffixes? (`docs/api/io/index.rst`)
- Is the request CLI-backed (`sgeom`/`sgrid`/`sdata`/`stoolbox`) or pure Python? (`docs/scripts/index.rst`, `pyproject.toml`)

### Canonical workflow
1. Start at API router (`docs/api/index.rst`) and pick the correct sub-tree.
2. Confirm base class/object semantics in `docs/api/basic.rst`.
3. For functional helpers and dispatch behavior, use `docs/api/core.rst`.
4. For file IO, apply `get_sile` rules/specifiers from `docs/api/io/index.rst`.
5. For domain methods (electron/phonon/matrix), use `docs/api/physics.*.rst`.
6. For plotting APIs, use `docs/api/viz/index.rst` and `docs/visualization/index.rst`.
7. If the user needs shell tools, map from `pyproject.toml [project.scripts]` to command docs.
8. Escalate to source maps only when docs leave symbol behavior ambiguous.

### Minimal working example
```python
import sisl as si
from ase.build import bulk

rot = si.rotate(bulk("Au"), 30, [1, 1, 1])
sile = si.get_sile("run/RUN.fdf")
H = sile.read_hamiltonian()
```

```bash
sgeom --help
sgrid --help
sdata RUN.fdf --help
stoolbox --help
```

### Pitfalls and fixes
- Ambiguous file extensions: use filename specifiers like `{contains=...}` (`docs/api/io/index.rst`).
- Dispatch return type surprises: `ret_sisl=True` forces sisl object return (`docs/api/core.rst`).
- Neighbor list type confusion (`Full` vs `Unique`): pick class by whether `I < J` symmetry reduction is desired (`docs/api/geom/neighbors.rst`).
- PDOS/electronic helpers may require overlap at matching k-point; use supporting electron classes (`docs/api/physics.electron.rst`).
- CLI behavior differs from API object calls when operation ordering is involved; route to scripts docs (`docs/scripts/sgeom.rst`, `docs/scripts/sgrid.rst`).

### Convergence and validation checks
- Confirm module/symbol exists in API autosummary path (`docs/api/*.rst`).
- For dispatch calls, validate returned object type explicitly.
- For IO, verify selected sile class and successful `read_*` operation.
- For CLI-linked behavior, reproduce with `--help` and one minimal command before scaling up.

## Scope
- Handle questions about language bindings, APIs, and programmatic interfaces.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/api/index.rst`
- `docs/api/basic.rst`
- `docs/api/core.rst`
- `docs/api/io/index.rst`
- `docs/api/geom/neighbors.rst`
- `docs/api/physics.rst`
- `docs/api/physics.electron.rst`
- `docs/api/physics.phonon.rst`
- `docs/api/viz/index.rst`
- `docs/visualization/index.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `docs/tutorials`
- `docs/visualization`
- `examples`

## Test references
- `src/sisl/tests`
- `src/sisl/geom/_neighbors/tests`
- `src/sisl/io/tests`
- `src/sisl/physics/tests`
- `src/sisl/viz/plots/tests`

## Optional deeper inspection
- `src`
- `tools`

## Source entry points for unresolved issues
- `src/sisl/__init__.py` (top-level API, lazy submodule access, dispatch exposure)
- `src/sisl/io/sile.py` (`add_sile`, `get_sile`, file-type resolution)
- `src/sisl/_core/geometry.py` and `src/sisl/_core/grid.py` (`sgeom`/`sgrid` entry functions)
- `src/sisl/utils/_sisl_cmd.py` (`sdata` command dispatcher)
- `src/sisl_toolbox/cli/_cli.py` (`stoolbox` command registration)
- `src/sisl/geom/_neighbors/_finder.py` and `_neighborlists.py` (neighbor finder/list internals)
- `src/sisl/physics/electron.py`, `src/sisl/physics/phonon.py`, `src/sisl/physics/_ufuncs_matrix.py` (physics API behavior)
- `src/sisl/viz/plots/matrix.py`, `src/sisl/viz/plotters/matrix.py`, `src/sisl/viz/processors/matrix.py` (viz pipeline)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src tools`).
