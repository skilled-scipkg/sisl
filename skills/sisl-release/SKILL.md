---
name: sisl-release
description: This skill should be used when users ask about release in sisl; it prioritizes documentation references and then source inspection only for unresolved details.
---

# sisl: Release

## High-Signal Playbook
### Route conditions
- Use this skill for release-note interpretation, migration planning, and version-conditional guidance for modern releases.
- Route <=0.15.2 change history requests to `sisl-changelog` (`docs/changelog/index.rst`).
- Route symbol-level API behavior to `sisl-api-and-scripting`.
- Route install/build breakages to `sisl-getting-started` or `sisl-advanced-topics`.

### Triage questions
- What is the current runtime version and target version? (`docs/release.rst`)
- Is the issue from new feature adoption or behavior change/regression?
- Is a version guard already used in user scripts? (`docs/release.rst`)
- Does the issue involve IO updates (buffers/zip), spin/config changes, or CLI semantics? (`docs/release/0.16.0-notes.rst`, `docs/release/0.16.1-notes.rst`)
- Is the relevant note in release docs (>=0.16) or old changelog (<=0.15.2)? (`docs/release.rst`)

### Canonical workflow
1. Identify current version using `sisl.__version_tuple__` (`docs/release.rst`).
2. Open `docs/release.rst` and locate the nearest release-note entries.
3. Read notes newest-to-oldest within target range (`0.16.2`, `0.16.1`, `0.16.0`).
4. Extract actionable deltas (new features, changed arguments, bugfixes).
5. Add explicit version guards when supporting multiple versions.
6. Validate with a minimal workflow that exercises the changed feature.
7. Escalate to code entry points only if doc text is insufficient.

### Minimal working example
```python
import sisl

if sisl.__version_tuple__[:3] >= (0, 16, 0):
    # use 0.16+ behavior
    pass
else:
    # legacy path
    pass
```

```python
import sisl

# 0.16.1+ documented path: read from zip container
H = sisl.get_sile("/path/to/data.zip/run/RUN.fdf").read_hamiltonian()
```

### Pitfalls and fixes
- Release notes cover modern releases; older history lives in changelog docs (`docs/release.rst`).
- `0.16.2` note is only a wheel-creation bugfix; avoid over-attributing behavior changes to it (`docs/release/0.16.2-notes.rst`).
- Argument/behavior renames in `0.16.0` can break older scripts (for example transpose `conjugate` keyword) (`docs/release/0.16.0-notes.rst`).
- `0.16.1` introduces full buffer/zip IO support; users on older versions need fallback paths (`docs/release/0.16.1-notes.rst`).

### Convergence and validation checks
- Version-gated branches select the intended path at runtime.
- Minimal reproduction demonstrates old vs new behavior boundary.
- Referenced release note section matches exact target version/date.
- For IO changes, confirm read/write works on one representative file before scaling.

## Scope
- Handle questions about release notes, migration boundaries, and version-aware behavior.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/release.rst`
- `docs/release/0.16.2-notes.rst`
- `docs/release/0.16.1-notes.rst`
- `docs/release/0.16.0-notes.rst`
- `docs/release/template.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `docs/release/0.16.1-notes.rst` (buffer/zip IO examples)
- `docs/release.rst` (version tuple gating snippet)

## Test references
- `src/sisl/tests`
- `src/sisl/io/tests`

## Optional deeper inspection
- `src`
- `tools`

## Source entry points for unresolved issues
- `src/sisl/__init__.py` (`__version__`, `__version_tuple__`, top-level behavior exposure)
- `pyproject.toml` (fallback version, dependency floors, script entry points)
- `tools/changelog.py` (legacy release/changelog generation tooling)
- `src/CMakeLists.txt` (build-system toggles relevant to release packaging)
- `src/sisl/io/sile.py`, `src/sisl/io/siesta/fdf.py`, and `src/sisl/io/siesta/siesta_nc.py` (IO behavior touched by release notes)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src tools`).
