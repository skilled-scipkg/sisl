---
name: sisl-changelog
description: This skill should be used when users ask about changelog in sisl; it prioritizes documentation references and then source inspection only for unresolved details.
---

# sisl: Changelog

## High-Signal Playbook
### Route conditions
- Use this skill for historical change tracking and migration notes for legacy releases.
- Route >=0.16 release-note questions to `sisl-release` (`docs/release.rst`).
- Route symbol-level API usage to `sisl-api-and-scripting`.
- Route build/install regressions to `sisl-getting-started` or `sisl-advanced-topics`.

### Triage questions
- Which exact version range is being compared?
- Is the request about `Added`, `Fixed`, or `Changed` items?
- Are there warning/deprecation signals that imply migration work?
- Which subsystem is affected (IO, geometry, viz, physics, CLI)?
- Do we need PR-level provenance for a claim? (`docs/changelog/v*.rst`)

### Canonical workflow
1. Start at `docs/changelog/index.rst` and pick target release files.
2. Read target files newest-to-oldest in the range.
3. Extract `Added`/`Fixed`/`Changed` bullets and associated PR references.
4. Map each change to impacted subsystem for user action.
5. Cross-check with `docs/release.rst` when range crosses 0.16 boundary.
6. If details are still unclear, inspect source entry points from `references/source_map.md`.

### Minimal working example
```bash
# Generate contributor/PR list between two tags
./tools/changelog.py "$GITHUB" v0.15.1..v0.15.2 > 0.15.2-changelog.rst
```

```python
import sisl

# use changelog-derived migration branching when supporting old releases
legacy = sisl.__version_tuple__[:3] <= (0, 15, 2)
```

### Pitfalls and fixes
- Old changelog and new release-note systems coexist; use the right tree by version (`docs/release.rst`, `docs/changelog/index.rst`).
- Changelog bullets can include both behavior changes and documentation-only updates; separate impact during triage (`docs/changelog/v0.15.0.rst`, `docs/changelog/v0.15.2.rst`).
- Some fixes intentionally raise warnings for compatibility transitions (for example integer handling in `Eindex`) (`docs/changelog/v0.15.2.rst`).
- PR totals include maintenance PRs; do not treat all listed PRs as user-facing API changes (`docs/changelog/v0.15.0.rst`).

### Convergence and validation checks
- Output cites exact changelog file(s) and version numbers.
- Each migration statement links to a concrete `Added`/`Fixed`/`Changed` entry.
- Version boundary with release notes is explicit when crossing 0.16.
- Any proposed workaround is validated against at least one minimal reproduction path.

## Scope
- Handle questions about changelog history and migration deltas for legacy releases.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/changelog/index.rst`
- `docs/changelog/v0.15.2.rst`
- `docs/changelog/v0.15.1.rst`
- `docs/changelog/v0.15.0.rst`
- `docs/changelog/v0.14.3.rst`
- `docs/changelog/v0.14.2.rst`
- `docs/changelog/v0.14.0.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `tools/changelog.py` usage examples in module docstring

## Test references
- `src/sisl/tests`
- `src/sisl/io/tests`
- `src/sisl/viz/plots/tests`

## Optional deeper inspection
- `src`
- `tools`

## Source entry points for unresolved issues
- `tools/changelog.py` (revision-range parsing, PR extraction, output formatting)
- `src/sisl/__init__.py` (`__version__`, `__version_tuple__` for migration branching)
- `pyproject.toml` (dependency/version floors relevant to release transitions)
- `src/sisl/io/siesta/fdf.py`, `src/sisl/physics/electron.py`, and `src/sisl/viz/plots/matrix.py` (common subsystems referenced by changelog items)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src tools`).
