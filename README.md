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
