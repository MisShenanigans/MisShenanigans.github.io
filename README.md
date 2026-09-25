# MisShenanigans.github.io

Personal website built with Quarto. Rendered pages are written to `docs/`.

## Python environment and rendering

The shared Python environment lives at the repository root, alongside
`_quarto.yml`. The setup has been checked with uv 0.12.5, Python 3.13.2,
and Quarto 1.10.18. Install uv and Quarto before running these commands.

From the repository root:

```bash
uv sync --locked
uv run quarto render posts/Optimal_Rivalry_Python_Demo/index.qmd
```

Open `docs/posts/Optimal_Rivalry_Python_Demo/index.html` to view the rendered
Python post. To preview it locally:

```bash
uv run quarto preview posts/Optimal_Rivalry_Python_Demo/index.qmd
```

Select the repository root's `.venv/bin/python` as your IDE's Python interpreter.
To check which interpreter Quarto uses, run `uv run quarto check jupyter` from
the repository root. Commit `pyproject.toml`, `uv.lock`, and `.python-version`;
the `.venv/` directory is recreated by `uv sync` and is ignored by Git.

## Interactive Python playground

The Python post includes editable browser code cells powered by
[Quarto Live](https://github.com/r-wasm/quarto-live). Its extension files are
included under `_extensions/r-wasm/live/`; commit these files along with the
rendered `docs/` assets. No separate Python server is needed on GitHub Pages.

Use the preview command above and open **Try it yourself**. Run **Load the matchup
data** first, then edit and run the counter-team, KDE, or maximin examples.
**Start Over** restores a cell's original code; reloading the page resets Python
variables. Browser edits are not saved to the repository.

The browser uses Pyodide 0.28.1, loaded from jsDelivr, with that release's NumPy,
pandas, SciPy, Matplotlib, and CVXPY packages. The browser optimizer uses SciPy's
HiGHS interface through CVXPY. This environment is separate
from the local uv environment. Browser package versions are selected by the
pinned Pyodide release, not by `uv.lock`. The maximin playground caps the solver
at 20 seconds by default and explicitly labels any unproven result as a candidate.

Both the article build and the browser playground fetch win-rate and match-count
CSVs from the `main` branch of
[Optimal_Rivalry](https://github.com/MisShenanigans/Optimal_Rivalry), originally
derived from Rivals Meta. Internet access and availability of GitHub are required;
the playground also needs access to the package CDN. Results may change when the
source CSVs change. Serve the rendered site over HTTP (for example with Quarto
preview), rather than opening its HTML using a `file://` URL, to use the playground.
