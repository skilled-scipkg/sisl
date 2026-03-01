---
name: sisl-advanced-topics
description: This skill should be used for low-frequency, single-document topics (cite, conventions, environment, toolbox internals, references, templates, and related advanced pages) after core workflow skills are ruled out.
---

# sisl: Advanced Topics

## High-Signal Playbook
### Route conditions
- Use this skill only after ruling out core workflow skills (`sisl-getting-started`, `sisl-api-and-scripting`, `sisl-inputs-and-modeling`, `sisl-scripts`, `sisl-release`, `sisl-changelog`).
- Focus on citation, units/conventions, environment controls, toolbox internals, references, and doc-template maintenance.

### Triage questions
- Is the user asking policy/attribution (`cite`) or runtime behavior (`environment`, units, toolbox internals)?
- Is the request documentation-process/developer oriented versus simulation API behavior?
- Do we need reproducible command output (`--cite`, env echo, CLI help) before source inspection?

### Canonical workflow
1. Route to the single most relevant advanced doc.
2. Reproduce behavior with one minimal command/snippet.
3. Confirm exact env/unit/toolbox setting used in the answer.
4. Escalate to source entry points only when docs do not explain runtime behavior.

### Minimal working example
```bash
sgeom --cite
sgrid --cite
sdata --cite
stoolbox --help
```

```python
import sisl as si
from sisl.unit import unit_convert

print(si.__version__)
print(unit_convert("Ry", "eV"))
```

```bash
# Environment introspection for parallel/runtime behavior
env | rg '^SISL_|^OMP_'
```

### Pitfalls and fixes
- Advanced docs are often single-page and easy to over-generalize; quote exact file path and section.
- Unit/codata expectations can differ by environment; verify `SISL_CODATA` assumptions.
- Citation output is command-generated and version-dependent; prefer runtime `--cite` over hand-edited BibTeX.
- Toolbox internals span multiple packages (`sisl_toolbox` and core `sisl`); keep boundaries explicit.

### Convergence and validation checks
- Claims about citation/version are reproduced from command output.
- Unit conversion claims are backed by a minimal call.
- Environment advice states explicit variable names and expected impact.
- If source-level explanation is needed, at least one function/class entry point is identified.

## Route the request
- Citation and attribution: `docs/cite.rst`.
- Units and notation conventions: `docs/conventions.rst`.
- Environment variables and runtime knobs: `docs/environment.rst`.
- Contribution/developer process: `docs/dev/index.rst`.
- Toolbox architecture and BTD theory pages: `docs/toolbox/index.rst`, `docs/toolbox/btd/btd.rst`.
- Bibliography and references list: `docs/references.rst`.
- Docs-template and spelling maintenance pages: `docs/_templates/autosummary/module.rst`, `docs/spelling_wordlist.txt`.
- Misc related software index pages: `docs/other.rst`.
- Generated geometry-building API reference page: `docs/api/geom/building.rst`.

## Workflow
- Start from the matching advanced doc above.
- If details are missing, inspect `references/doc_map.md` for combined inventory.
- If doc coverage is insufficient, use `references/source_map.md` for targeted source entry points.
- Keep responses docs-first and cite exact file paths.

## Optional deeper inspection
- `src`
- `tools`

## Source entry points for unresolved issues
- `src/CMakeLists.txt` and `src/sisl/geom/CMakeLists.txt` (build/config-related advanced questions)
- `src/sisl/__init__.py` and `src/sisl/_environ.py` (citation/version/env behavior)
- `src/sisl/unit/__init__.py`, `src/sisl/unit/base.py`, `src/sisl/unit/siesta.py` (units/conventions)
- `src/sisl_toolbox/README`, `src/sisl_toolbox/cli/_cli.py`, `src/sisl_toolbox/models/_base.py` (toolbox structure)
- `src/sisl_toolbox/btd/_btd.py`, `_green.py`, `_electrode.py`, `_help.py` (BTD implementation entry points)
- `tools/changelog.py` (historical release/changelog generation utility)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src tools`).
