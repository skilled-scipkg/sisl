---
name: sisl-scripts
description: This skill should be used when users ask about scripts in sisl; it prioritizes documentation references and then source inspection only for unresolved details.
---

# sisl: Scripts

## High-Signal Playbook
### Route conditions
- Use this skill for `sgeom`, `sgrid`, `sdata`, and `stoolbox` command-line usage.
- Route API-level (Python) equivalents to `sisl-api-and-scripting`.
- Route model-definition details (Hamiltonian setup, physics parameters) to `sisl-inputs-and-modeling`.
- Route install/packaging issues for missing commands to `sisl-getting-started`.

### Triage questions
- Which command is being used (`sgeom`, `sgrid`, `sdata`, `stoolbox`)? (`docs/scripts/index.rst`)
- What is the input file type and required output format? (`docs/scripts/sgeom.rst`, `docs/scripts/sgrid.rst`)
- Are operations chained, and in what order? (`docs/scripts/sgeom.rst`, `docs/scripts/sgrid.rst`)
- Is extra geometry metadata needed for grid export? (`docs/scripts/sgrid.rst`)
- Is the file polymorphic, making `sdata <in> --help` preferable? (`docs/scripts/sdata.rst`)

### Canonical workflow
1. Start with command help (`<cmd> --help`) to expose supported actions (`docs/scripts/*.rst`).
2. Run the minimal conversion first (`sgeom in out` or `sgrid in out`) (`docs/scripts/sgeom.rst`, `docs/scripts/sgrid.rst`).
3. Add one transformation at a time and keep operation order explicit.
4. For grid export, add `--geometry` when CUBE metadata is needed (`docs/scripts/sgrid.rst`).
5. For mixed file types, switch to `sdata` and inspect dynamic help (`docs/scripts/sdata.rst`).
6. For contributed tools, use `stoolbox` subcommands (`docs/scripts/stoolbox.rst`).
7. Escalate to source only for parser/action edge cases.

### Minimal working example
```bash
sgeom RUN.fdf RUN.xyz
sgeom RUN.fdf --repeat 4 x RUN4x.fdf

sgrid Rho.grid.nc --geometry RUN.fdf Rho.cube
sgrid "Rho.grid.nc{1.,-1.}" -G RUN.fdf diff_up-down.cube

sdata RUN.fdf --help
stoolbox --help
```

### Pitfalls and fixes
- Input file must be first positional argument for `sgeom`/`sgrid` parser flow (`docs/scripts/sgeom.rst`, `docs/scripts/sgrid.rst`, `src/sisl/_core/geometry.py`, `src/sisl/_core/grid.py`).
- Operation order changes results; commands with same flags but different order are not equivalent (`docs/scripts/sgeom.rst`, `docs/scripts/sgrid.rst`).
- Shell parsing can break bracketed spin specs; quote expressions like `"Rho.grid.nc{1.,-1.}"` (`docs/scripts/sgrid.rst`).
- Large CUBE outputs are expensive; reduce/subselect grids before writing (`docs/scripts/sgrid.rst`).
- `sdata` options depend on input file content; always inspect file-specific help (`docs/scripts/sdata.rst`).

### Convergence and validation checks
- `--help` output shows expected option family for the chosen command.
- One-step conversion runs before adding chained transforms.
- Intermediate outputs are saved when chaining complex operations (order audit).
- For grid differences/reductions, confirm resulting shape/extent matches command order intent.

## Scope
- Handle questions about command-line workflows and script entry points.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/scripts/index.rst`
- `docs/scripts/sgeom.rst`
- `docs/scripts/sgrid.rst`
- `docs/scripts/sdata.rst`
- `docs/scripts/stoolbox.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `docs/scripts/*.rst`
- `examples`

## Test references
- `src/sisl/_core/tests/test_sgeom.py`
- `src/sisl/_core/tests/test_sgrid.py`
- `src/sisl/utils/tests/test_cmd.py`

## Optional deeper inspection
- `src`
- `tools`

## Source entry points for unresolved issues
- `pyproject.toml` (`[project.scripts]` command-to-function mapping)
- `src/sisl/_core/geometry.py` (`sgeom`, parser actions for geometry transforms)
- `src/sisl/_core/grid.py` (`sgrid`, parser actions for grid transforms)
- `src/sisl/utils/_sisl_cmd.py` (`sdata` adaptive command dispatcher)
- `src/sisl/utils/cmd.py` (shared argparse collection/output actions)
- `src/sisl_toolbox/cli/_cli.py` (`stoolbox` subcommand registration model)
- `src/sisl_toolbox/siesta/atom/_atom.py` (example toolbox CLI registration)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src tools`).
