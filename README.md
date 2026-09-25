# MisShenanigans.github.io

Personal website built with Quarto. Rendered pages are written to `docs/`.

## Set up both environments

The shared Python environment lives at the repository root, alongside
`_quarto.yml`. R uses a separate `renv` project at the same root. The setup uses
uv 0.12.5, Python 3.13.2, R 4.6.1, and Quarto 1.10.18. Install uv, R, and Quarto
before running these commands. Python is selected by `.python-version`; install
the R version recorded in `renv.lock` yourself (`renv` manages packages, not R).

For a fresh checkout:

```bash
git clone https://github.com/MisShenanigans/MisShenanigans.github.io.git
cd MisShenanigans.github.io
uv sync --locked
Rscript -e 'renv::restore(prompt = FALSE)'
uv run quarto render
```

The committed `.Rprofile` bootstraps `renv` on first use. The root environment
includes `knitr`, `rmarkdown`, and `lpSolve`, with all package versions recorded
in `renv.lock`. Run R and Quarto commands from the repository root so this
profile is loaded. The R post uses base R for its KDE and plot; no system LaTeX
installation or separate optimization software is required for HTML output.

The full render writes the website into `docs/`, including the blog listing.
To render or preview just one post:

```bash
uv run quarto render posts/optimal_rivalry_python_demo/index.qmd
uv run quarto render posts/optimal_rivalry_r_demo/index.qmd

uv run quarto preview posts/optimal_rivalry_python_demo/index.qmd
# Or preview the R version:
uv run quarto preview posts/optimal_rivalry_r_demo/index.qmd
```

Select the repository root's `.venv/bin/python` as your IDE's Python interpreter.
To check which interpreter Quarto uses, run `uv run quarto check jupyter` from
the repository root. Commit `pyproject.toml`, `uv.lock`, and `.python-version`;
the `.venv/` directory is recreated by `uv sync` and is ignored by Git. For R,
commit `.Rprofile`, `renv.lock`, and the setup files in `renv/`; its local library,
staging, and cache directories are ignored. After deliberately adding an R
dependency, run `Rscript -e 'renv::snapshot()'` and commit the updated lockfile.
Check the R environment with `Rscript -e 'renv::status()'`.

## Interactive playgrounds

Both posts include editable browser code cells powered by
[Quarto Live](https://github.com/r-wasm/quarto-live). Its extension files are
included under `_extensions/r-wasm/live/`; commit these files along with the
rendered `docs/` assets. No separate Python server is needed on GitHub Pages.

Use the appropriate preview command above and open **Try it yourself**. Run
**Load the matchup data** first, then edit and run the counter-team example.
**Start Over** restores a cell's original code; reloading the page resets the
variables. Browser edits are not saved to the repository.

The browser uses Pyodide 0.28.1, loaded from jsDelivr, with that release's NumPy,
pandas, SciPy, Matplotlib, and CVXPY packages. The R playground uses webR 0.6.0
and base R. These environments are separate from the local `uv` and `renv`
environments: browser versions come from the pinned runtimes, not the local
lockfiles. The counter-team playgrounds rank additive contributions; the full
maximin optimizations run locally during rendering. The R article's bandwidth
calculation matches SciPy's weighted Scott rule so its payoff matrix can be
compared directly with Python's.

Both the article build and the browser playground fetch win-rate and match-count
CSVs from the `main` branch of
[Optimal_Rivalry](https://github.com/MisShenanigans/Optimal_Rivalry), originally
derived from Rivals Meta. Internet access and availability of GitHub are required;
the playgrounds also need access to jsDelivr and/or `webr.r-wasm.org` and
`repo.r-wasm.org`. Results may change when the
source CSVs change. Serve the rendered site over HTTP (for example with Quarto
preview), rather than opening its HTML using a `file://` URL, to use the playground.

## Verification

On September 24, 2026, the full site rendered with `uv run quarto render`.
A temporary copy without an R library successfully bootstrapped `renv`, restored
the lockfile (using the local package cache), and rendered the R post. All 3,136
R payoff values matched Python, and the maximin formulation matched exhaustive
search on eight small test matrices. The R data-loading and counter-team cells
also ran successfully in Firefox, producing the same counter-team score (23.86).
