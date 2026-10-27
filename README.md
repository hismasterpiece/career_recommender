# Explainable career recommender (dissertation project)

Weeks 1-8 code: data acquisition, tidying and first exploratory analysis in R.

## How to open

Double-click `career-recommender.Rproj` (or `setwd()` to this folder). The Rproj file marks the project
root so `here::here()` always points to this folder, not to `R/`. Check with `here::here()`.

## Run order

1. `R/00_setup.R` -- installs packages, creates folders
2. `R/01_download_data.R` -- downloads O*NET and BLS OEWS into `data/raw/`
3. `R/02_tidy_onet.R` -- builds tidy tables in `data/tidy/`
4. `R/03_eda_pilot.R` -- pilot plots in `output/figures/`
5. `R/04_candidate_options.R` -- data-driven list of many career options per qualification, with
   optional preference filters (edit the SETTINGS block at the top); writes `output/candidate_options_all.csv`
6. `R/06_logistic_model.R` -- logistic-regression recommender plus a simple test with 120 simulated users
   (no participants needed); writes `output/model_comparison.csv`, `model_coefficients.csv` and a figure

## Career Options Finder (web page)

Open `app/index.html` in any browser. It is one self-contained file with the O*NET and BLS data built in,
and uses the same scoring as script 04.

## Status

Scripts 01 and 02's inputs were checked against the downloaded O*NET 29.3 and BLS OEWS May 2024 files
(column names, scale IDs, work-context element names, pilot SOC codes). They have not been executed in R
by the author, so read the console output and report any error.

Script 06 was written after its logic was replicated in Python on the same O*NET/BLS files (expected
precision@5: similarity only about 0.53, filter about 0.65, logistic regression about 0.65; AUC about 0.65,
0.71, 0.72). R uses a different random generator, so expect similar but not identical numbers.
The simulated users answer by rule, so the test checks the pipeline, not what real people think.

## Notes

- O*NET 29.3 has no "Structured versus Unstructured Work" work-context element; autonomy is captured by
  "Determine Tasks, Priorities and Goals" and "Freedom to Make Decisions" instead.
- BLS top-codes some wages as "#" (e.g. pathologists' annual mean); these become NA.
- Clinical laboratory technologists (29-2011) are reported by BLS only at the broad level (29-2010);
  the tidy script falls back to the broad group wage and records `wage_source`.

## Not in this repository

`data/raw/` (O*NET and BLS downloads), `data/tidy/` and `output/` are ignored; script 01 and 02 re-create them.

## Data sources

- O*NET database: https://www.onetcenter.org/database.html
- BLS OEWS national tables: https://www.bls.gov/oes/tables.htm
