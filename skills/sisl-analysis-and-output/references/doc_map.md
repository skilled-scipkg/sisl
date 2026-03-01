# sisl documentation map: Analysis and Output

Use this map to quickly route analysis/output requests before checking source internals.

## Primary start points
- `docs/visualization/index.rst` | visualization overview and plot families
- `docs/api/viz/index.rst` | plot/data API entry point
- `docs/api/physics.electron.rst` | DOS/PDOS and electron analysis helpers
- `docs/api/physics.brillouinzone.rst` | k-point averaging/apply workflows
- `docs/api/io/index.rst` | file IO and sile dispatch basics for analysis inputs

## Output and conversion docs
- `docs/scripts/sgrid.rst` | grid transforms and CUBE export patterns
- `docs/scripts/sdata.rst` | content-driven CLI inspection by file type
- `docs/visualization/ase/index.rst` | geometry to ASE visualization handoff
- `docs/visualization/blender/First animation.rst` | blender animation path

## Executable walkthrough assets
- `docs/visualization/basic-tutorials/Demo.ipynb`
- `docs/visualization/combining-plots/Intro to combining plots.ipynb`
- `docs/visualization/showcase/BandsPlot.ipynb`
- `docs/visualization/showcase/PdosPlot.ipynb`
- `docs/visualization/showcase/GridPlot.ipynb`
- `docs/visualization/showcase/GeometryPlot.ipynb`

## Validation references
- `docs/visualization/tests/test_tutorials.py` | notebook-level regression coverage in docs tree
