# PBS Utah Major-Donor Prospecting

## Project objective

This project documents a time-aware donor--fiscal-year analysis for major-donor
prospecting. It separates exploratory analysis and data preparation from the
individual modeling submission.

## Notebooks

- [`eda_pbs_utah.qmd`](eda_pbs_utah.qmd) — data loading, content-based
  cleaning, data-quality checks, descriptive EDA, cohort/target definitions,
  and feature exploration.
- [`pbs_utah_modeling.qmd`](pbs_utah_modeling.qmd) — the individual modeling
  notebook for submission, including time-based evaluation and the three
  modeling workstreams.

## Datasets used

All source files are in [`tables/`](tables/):

- `constituents_w_memberships.csv` — constituent attributes and membership data
- `unite_payments.csv` — payments from FY2021 through FY2026
- `team_approach_legacy_payments.csv` — legacy payments before FY2021
- `soft_credits.csv` — soft credits associated with constituents
- `campaign_codes.csv` and `campaign_members.csv` — marketing-code and
  solicitation history
- `passport_viewing.csv` — Passport viewing by constituent, fiscal year, and
  genre
- `cultivation.csv` and `officer_portfolios.csv` — cultivation assignments and
  portfolio metadata

The data dictionaries in [`docs/`](docs/) describe fields, grain,
relationships, and synthetic-data conventions.

## Synthetic classroom data

The files in `tables/` are synthetic classroom data created for this project;
they do not contain real PBS donor records. This public repository may include
those project tables, but no real PBS donor data or other sensitive production
data should be added to it.

Several currently tracked large raw files use Git LFS. A clone intended to
render the notebook therefore needs Git LFS installed and enabled before the
tables are retrieved:

```bash
git lfs install
git lfs pull
```

The repository's `.gitignore` prevents new raw CSVs and local artifacts from
being added. It does not remove files already tracked by Git; those remain
tracked until they are deliberately removed from the index and, if appropriate,
repository history.

## Run or render the EDA

The notebooks require the R packages `tidyverse`, `janitor`, `skimr`, `naniar`,
`lubridate`, `here`, `glmnet`, `rpart`, `logistf`, and `knitr`.

1. Install [Quarto](https://quarto.org/docs/get-started/) and R.
2. In R, install any missing packages:

   ```r
   install.packages(c(
     "tidyverse", "janitor", "skimr", "naniar", "lubridate", "here",
     "glmnet", "rpart", "logistf", "knitr"
   ))
   ```

3. From the project root, render the desired notebook:

   ```bash
   quarto render eda_pbs_utah.qmd
   quarto render pbs_utah_modeling.qmd
   ```

The EDA notebook constructs the cleaned household-aware donor-year inputs and
documents descriptive findings. The modeling notebook applies training-only
preprocessing and rolling fiscal-year evaluation to active never-major
prospecting, current-major retention risk, and former-major reactivation.

## Key data-cleaning rules

- Preserve source files unchanged; create separate cleaned objects in R.
- Standardize identifiers as character values and parse dates explicitly.
- Use a July--June fiscal year: FY2026 runs from July 2025 through June 2026.
- Remove transaction copies only through documented content-based valid-twin
  checks and retain an audit trail; synthetic labels are audit-only.
- Retain all campaign-member rows because repeats can be valid solicitations.
- Exclude organization payments from individual direct-payment construction and
  add soft credits to the individual recipient without double counting.
- Allocate legacy household gifts to the primary linked constituent only and
  document this limitation.
- Aggregate giving, campaign, and Passport records to one constituent--fiscal-
  year row before joining; snapshot-by-snapshot processing prevents many-to-many
  row explosions.
- Use only information available by the snapshot fiscal-year end. Exclude
  future outcomes, end-of-data snapshots, and staff-selected cultivation fields
  from standard predictors.

## Synthetic data

Every record in this release is synthetic. Identifiers are fabricated, and no
record represents a real person, donor, gift, campaign, or viewing event. This
repository demonstrates an analytical workflow and must not be used for real
fundraising decisions.
