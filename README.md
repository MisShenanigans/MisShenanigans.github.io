# MisShenanigans.github.io

Personal website built with Quarto. Rendered pages are written to `docs/`.
View the website on: https://misshenanigans.github.io/

## Set up both environments

The shared Python environment lives at the repository root, alongside
`_quarto.yml`. R uses a separate `renv` project at the same root. The setup uses
uv 0.12.5, Python 3.13.2, R 4.6.1, and Quarto 1.10.18. Install uv, R, and Quarto
before running these commands. Python is selected by `.python-version`; install
the R version recorded in `renv.lock` manually.

Clone the repository:

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
. The full render writes the website into `docs/`, including the blog listing.
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
the repository root. Commit `pyproject.toml`, `uv.lock`, and `.python-version`; After adding an R
dependency, run `Rscript -e 'renv::snapshot()'` and commit the updated lockfile.
You can also check the R environment with `Rscript -e 'renv::status()'`.
