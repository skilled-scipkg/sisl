---
name: sisl-parallel-hpc
description: This skill should be used when users ask about parallel and hpc in sisl; it prioritizes documentation references and then source inspection only for unresolved details.
---

# sisl: Parallel and HPC

## High-Signal Playbook
### Route conditions
- Use this skill for k-point parallelism, processor/thread tuning, and TranSiesta FFT-Poisson correction workflows.
- Route model construction to `sisl-inputs-and-modeling`.
- Route install issues (`pathos`, compiled deps) to `sisl-getting-started`.
- Route generic API semantics to `sisl-api-and-scripting`.

### Triage questions
- Is the workload Brillouin-zone looping (`MonkhorstPack.apply`) or toolbox CLI (`stoolbox ts-fft`)?
- What are current values for `SISL_NUM_PROCS`, `OMP_NUM_THREADS`, and cluster core allocation?
- Is performance bound by Python process pool overhead or linear-algebra threading?
- Do we need correctness parity first (serial vs parallel) before scaling?

### Canonical workflow
1. Establish serial baseline on a reduced k-grid.
2. Enable pool parallelism with explicit processor count.
3. Tune `SISL_NUM_PROCS * OMP_NUM_THREADS` to stay within allocated cores.
4. For TranSiesta correction, validate boundary-condition choices on a smaller grid first.
5. Escalate to source only for dispatch/chunksize/runtime anomalies.

### Minimal working example
```bash
export SISL_NUM_PROCS=2
export OMP_NUM_THREADS=1
```

```python
import sisl as si

g = si.geom.graphene(1.42)
H = si.Hamiltonian(g)
for ia in g:
    near = g.close(ia, [0.142, 1.43])
    H[ia, near[0]] = 0.0
    H[ia, near[1]] = -2.7

mp = si.MonkhorstPack(H, [6, 6, 1])
with mp.apply.renew(pool=2) as par:
    eig = par.array.eigh()
print(eig.shape)
```

```bash
stoolbox ts-fft --help
```

### Pitfalls and fixes
- Oversubscription (`SISL_NUM_PROCS * OMP_NUM_THREADS` too high) can make parallel slower than serial.
- Pool startup overhead dominates tiny k-point workloads; batch work or stay serial for small jobs.
- Wrong boundary conditions in `ts-fft` produce physically inconsistent corrections; validate direction-by-direction.
- `pathos` may be missing on lean installs; install optional dependencies before HPC tuning.

### Convergence and validation checks
- Parallel result matches serial within expected numerical tolerance.
- Walltime decreases with moderate pool size on representative workload.
- CPU utilization aligns with requested process/thread limits.
- `ts-fft` output file shape and electrode boundary behavior match intended setup.

## Scope
- Handle questions about MPI/OpenMP/process-pool execution, scaling, and HPC-oriented workflows in sisl.
- Keep responses practical for launching and validating real parallel runs.

## Primary documentation references
- `docs/api/physics.brillouinzone.rst`
- `docs/environment.rst`
- `docs/toolbox/transiesta/ts_fft.rst`
- `docs/scripts/stoolbox.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with ranked function-level entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `docs/tutorials/tutorial_05.rst`
- `docs/tutorials/tutorial_06.rst`
- `docs/toolbox/transiesta/ts_fft.rst`

## Test references
- `src/sisl/physics/tests/test_brillouinzone.py`
- `src/sisl/physics/tests/test_hamiltonian.py`
- `src/sisl/tests/test_package.py`

## Optional deeper inspection
- `src`
- `tools`

## Source entry points for unresolved issues
- `src/sisl/physics/brillouinzone.py` (`BrillouinZone`, `MonkhorstPack`, dispatch integration)
- `src/sisl/physics/_brillouinzone_apply.py` (apply-loop execution and pool orchestration)
- `src/sisl/_environ.py` (`SISL_NUM_PROCS`, `SISL_PAR_CHUNKSIZE`, progress/env controls)
- `src/sisl/physics/tests/test_brillouinzone.py` (parallel/aggregation behavior checks)
- `src/sisl_toolbox/transiesta/poisson/fftpoisson_fix.py` (`stoolbox ts-fft` logic)
- `src/sisl_toolbox/transiesta/poisson/__init__.py` (toolbox command registration)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src tools`).
