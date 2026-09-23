# Behtash JavidSharifi — Research Portfolio

This repository contains my personal Quarto website and computational
posts about my earthquake engineering PhD research and RECoVER workflow.

Website: https://bjavidsh.github.io/
Repository: https://github.com/bjavidsh/bjavidsh.github.io

## Software prerequisites

The development environment used:

- Windows 11 with Git Bash
- Quarto 1.10.18
- uv 0.12.7
- R 4.6.1
- Python 3.14.7

Install Git, Quarto, uv, and R before building. Ensure that `git`,
`quarto`, `uv`, `R`, and `Rscript` are available in your terminal.

The `.python-version` file requests Python 3.14. uv can download a
compatible interpreter when necessary. R must be installed separately;
renv restores R packages but does not install R itself.

Python package versions are recorded in `uv.lock`. R package versions
are recorded in `renv.lock`.

## Build from a fresh clone

The following commands are for Git Bash on Windows. Run the first
command from a folder where you want to create the repository.

### 1. Clone the repository

```bash
git clone https://github.com/bjavidsh/bjavidsh.github.io.git
cd bjavidsh.github.io
```

Run all remaining build commands from this repository root, where
`_quarto.yml` is located.

### 2. Restore the Python environment

```bash
uv sync --locked
```

This creates the local `.venv` and installs the locked Python packages,
including Jupyter, ipykernel, pandas, and matplotlib.

### 3. Restore the R environment

```bash
Rscript -e 'renv::restore(prompt = FALSE)'
```

The project's `.Rprofile` loads `renv/activate.R`, which bootstraps
renv when needed. The restore command installs the packages recorded
in `renv.lock`.

### 4. Render the complete website

```bash
uv run quarto render
touch docs/.nojekyll
```

Run Quarto through `uv run` so the project's Python environment is
available for executing Python chunks. R chunks use the project's
renv environment.

The generated website is written to `docs/`. Its home page is
`docs/index.html`. The `.nojekyll` file supports GitHub Pages hosting
of the generated site.

### 5. Open the website locally

In Git Bash on Windows:

```bash
start docs/index.html
```

Check the Blog page, both computational posts, their figures and
CSV links, and the other navigation links.

## Computational posts

### R: School buildings in my earthquake field research

Source: `posts/field-study-buildings/index.qmd`

This post describes 14 building cases, checks completeness, summarizes
recorded damage categories, and plots reported storey counts.

Input: `posts/field-study-buildings/field-av-buildings.csv`

### Python: Candidate vibration-frequency recurrence

Source: `posts/ambient-vibration-repeatability/index.qmd`

This post examines 87 candidate frequency families across 13 building
cases, summarizes recurrence across recording windows, and compares
recorded frequency spreads.

Input:
`posts/ambient-vibration-repeatability/field-av-frequency-families.csv`

## Data provenance and interpretation

The CSV files are selected-column extracts from the `CASE_REVIEW`
and `FAMILIES` sheets of
`RECoVER_FieldAV_AdjudicationWorkbook_AR38D.xlsx`.

They preserve the records used in this workbook snapshot. The posts
read the CSV files without modifying them. The original Excel workbook
and raw sensor recordings are not required to rebuild the website.

The building analysis is descriptive and does not establish a causal
relationship between storey count and damage. The frequency families
are candidate evidence for review, not confirmed structural modes.

## Network requirements

Cloning the repository and initially downloading Python, renv, and
packages normally require internet access.

The two computational posts read their included CSV files locally.
They do not download research data during rendering.

## Environment and generated files

Commit the Quarto sources, input CSV files, Python environment files,
R lockfile and activation files, and the generated `docs/` website.

Local package libraries and temporary files, including `.venv/`,
`renv/library/`, `.quarto/`, and `_site/`, are not version-controlled.

After changing dependencies, update the appropriate lockfile:
use `uv add` for Python packages, or install R packages and then run
`renv::snapshot()` from R in the project.

## Build verification

The restoration and rendering commands were successfully tested in a
separate local Git clone on Windows 11. Python used the clone's own
`.venv`, renv reported a consistent project, and all seven website
pages rendered successfully.


## Mixed R and Python bonus post

The source is `posts/r-and-python/index.qmd`. It uses the knitr engine
and reticulate to pass an R summary to Python and return Python-calculated
percentages to R.

The post selects the Python interpreter in the current project's `.venv`
using `QUARTO_PROJECT_DIR` and `RETICULATE_PYTHON`. No personal absolute
path is required.

From the repository root, restore both environments before rendering:

```bash
uv sync --locked
Rscript -e 'renv::restore(prompt = FALSE)'
uv run quarto render
```

The input CSV is already included at
`posts/ambient-vibration-repeatability/field-av-frequency-families.csv`.
No additional research-data download is needed.