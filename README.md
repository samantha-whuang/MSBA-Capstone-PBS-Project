# PBS Utah Major-Donor Prospecting

## Project objective

This project explores donor, giving, engagement, campaign, and cultivation data
to prepare a time-aware donor--fiscal-year dataset for major-donor prospecting.
The EDA documents data structure, quality checks, cleaning decisions, giving and
engagement patterns, conversion targets, and candidate predictors before model
testing.

## Datasets used

All source files are in [`Data/`](Data/):

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

The data dictionaries in `Data/` describe fields, grain, relationships, and
synthetic-data conventions.

## Run or render the EDA

The analysis is in [`eda_pbs_utah.qmd`](eda_pbs_utah.qmd). It uses R packages
including `tidyverse`, `janitor`, `skimr`, `naniar`, `lubridate`, and `here`.

1. Install [Quarto](https://quarto.org/docs/get-started/) and R.
2. In R, install any missing packages:

   ```r
   install.packages(c("tidyverse", "janitor", "skimr", "naniar", "lubridate", "here"))
   ```

3. From the project root, render the document:

   ```bash
   quarto render eda_pbs_utah.qmd
   ```

The document constructs 12-month and longer-term conversion targets, time-aware
giving-history, campaign, and Passport features, and a joined donor-year
candidate feature table. It does not fit or validate a predictive model.

## Key data-cleaning rules

- Preserve source files unchanged; create separate cleaned objects in R.
- Standardize identifiers as character values and parse dates explicitly.
- Use a July--June fiscal year: FY2026 runs from July 2025 through June 2026.
- Remove only confirmed exact duplicate records and retain an audit trail.
- Retain `SYN-ORPHAN` records for audit, but exclude them from the individual
  modeling population because they do not link to a constituent.
- Retain zero-dollar records in raw audit tables while excluding them from
  annual giving-dollar totals.
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
