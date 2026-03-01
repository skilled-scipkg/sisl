# sisl source map: Advanced Topics

Generated from source roots:
- `src`
- `tools`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `build`
- `cmake`
- `cite`
- `conventions`
- `environment`
- `developer`
- `references`
- `toolbox`
- `btd`
- `unit`
- `template`
- `spelling`

## Fast source navigation
- `rg -n "<symbol_or_keyword>" src tools`
- `rg -n "class|def|struct|namespace" src tools`
- If a doc mentions a function/class, search that exact symbol first, then inspect nearby implementation files.

## Suggested source entry points
- `src/CMakeLists.txt` | matched tokens: build, cmake
- `src/sisl/geom/CMakeLists.txt` | matched tokens: build, cmake
- `src/sisl/__init__.py` | matched tokens: cite, environment
- `src/sisl/_environ.py` | matched tokens: environment
- `src/sisl/unit/__init__.py` | matched tokens: conventions, unit
- `src/sisl/unit/base.py` | matched tokens: conventions, unit
- `src/sisl/unit/siesta.py` | matched tokens: unit
- `src/sisl_toolbox/README` | matched tokens: toolbox
- `src/sisl_toolbox/cli/_cli.py` | matched tokens: toolbox
- `src/sisl_toolbox/models/_base.py` | matched tokens: toolbox
- `src/sisl_toolbox/btd/_btd.py` | matched tokens: btd, toolbox
- `src/sisl_toolbox/btd/_green.py` | matched tokens: btd, toolbox
- `src/sisl_toolbox/btd/_electrode.py` | matched tokens: btd, toolbox
- `src/sisl_toolbox/btd/_help.py` | matched tokens: btd, toolbox
- `tools/changelog.py` | matched tokens: references
