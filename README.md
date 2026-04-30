# Predictors of Infusion Visit Adherence in the AMP Trials

Reproducible code for the manuscript:

**Predictors of Infusion Visit Adherence in the AMP Trials: A Secondary Analysis of HVTN 704/HPTN 085 and HVTN 703/HPTN 081**

## Repository
GitHub: `https://github.com/Joshua-M-Data/BRIDGE-Project-`

## Analysis Files

This project is split into two R Markdown workflows:

1. `Retro_AMP_Project_Partition.Rmd`  
   Manuscript-only analysis (primary figures/tables + manuscript-referenced results).

2. `Retro_AMP_Project_Exploratory.Rmd`  
   Full exploratory workflow (legacy checks, alternate models, additional diagnostics/plots).

## Recommended Repository Layout

```text
repo-root/
  README.md
  Retro_AMP_Project_Partition.Rmd
  Retro_AMP_Project_Exploratory.Rmd
  subject_master.csv
  retention.csv
  reacto.csv
  AMP Primary Data Dictionary.xlsx          (optional, reference)
  protocol PDFs / manuscript drafts         (optional, reference)
```

## Data Inputs

Place these files in the same directory as the Rmd files:

- `subject_master.csv`
- `retention.csv`
- `reacto.csv`

Optional reference files:

- `AMP Primary Data Dictionary.xlsx`
- AMP protocol/supporting PDFs

## Software

- R 4.5.x

## How To Run

From R:

```r
rmarkdown::render("Retro_AMP_Project_Partition.Rmd", output_format = "html_document")
rmarkdown::render("Retro_AMP_Project_Exploratory.Rmd", output_format = "html_document")
```

Optional PDF:

```r
rmarkdown::render("Retro_AMP_Project_Partition.Rmd", output_format = "pdf_document")
```

From command line:

```bash
Rscript -e "rmarkdown::render('Retro_AMP_Project_Partition.Rmd', output_format='html_document')"
Rscript -e "rmarkdown::render('Retro_AMP_Project_Exploratory.Rmd', output_format='html_document')"
```

## Workflow

### Manuscript partition (`Retro_AMP_Project_Partition.Rmd`)
- Main manuscript items:
  - Table 1 (visit-level adjusted OR table)
  - Figure 1 (stacked infusion outcomes by infusion number)
  - Figure 2 (missed-infusion trends by age group)
  - Figure 3 (secondary participant-level predicted probability by age)
  - Figure 4 (missed proportion by treatment arm and age group)
- Supplementary items:
  - Supplementary Table S1 (early termination reasons)
  - Supplementary Table S2 (pooled active vs control by age band)
  - Supplementary Figure S1/S2/S3 style outputs (pooled arm chart, missed rate by infusion number, AE heatmap)
- Manuscript-linked numeric blocks:
  - protocol-level descriptive missed rates
  - secondary participant-level age and infusion-count terms
  - pooled arm x age interaction test
  - age 26-33 subgroup tests
  - AE-prior adjusted and unadjusted summaries

### Exploratory workflow (`Retro_AMP_Project_Exploratory.Rmd`)
- Full history, alternative models, and extra analyses.

## Core Definitions Used in Manuscript Workflow

- **Primary unit of analysis:** scheduled infusion visit.
- **Missed infusion (`missed_infusion_core`):** scheduled infusion visit not classified as `Arrived + Infused`.
- **Primary analytic cohort:** expected scheduled infusion visits after excluding post-discontinuation rows (`visitday > discday`).
- **Primary model:** visit-level mixed-effects logistic regression with participant random intercept and adjustment for infusion number.
- **Secondary models:** participant-level `>=2` missed model and AE-lag mixed model (miss at infusion `t+1` predicted by AE at infusion `t`).

## Key Modeling Specifications

- Visit-level mixed model:
  - Outcome: `missed_infusion_core` (binary)
  - Predictors: `age_bin`, `sex`, `country`, `rx_code`, `infusion_num_f`
  - Random effect: `(1 | participant_id)`
- Participant-level secondary model:
  - Outcome: `missed2plus` (>=2 missed infusions)
  - Predictors: `age`, `sex`, `country`, `rx_code`, `total_infusions`
- AE-lag model:
  - AE mapped to infusion sequence within participant (`protocol + uid`)
  - `AE_prior` = AE at infusion `t-1`
  - Outcome = missed at infusion `t`

## Reproducibility Notes

- Manuscript denominator structure is explicitly reported in the partition file:
  - Raw scheduled infusion rows
  - Primary analytic infusion rows
  - Primary attended+infused and missed counts
- Country is modeled without protocol in adjusted primary models to avoid nested collinearity.

## Expected Cohort Checkpoints (Current Public Extract)

If your run is aligned with the current manuscript partition, you should see:
- Raw scheduled infusion rows: `42,802`
- Primary analytic infusion rows: `42,796`
- Primary missed infusions: `2,128`
- Primary attended + infused: `40,668`
- Participant distribution:
  - `0 missed`: `3,780`
  - `1 missed`: `386`
  - `2+ missed`: `457`

## Suggested Citation Text (Code Availability)

Reproducible analysis code for this study is publicly available at:  
`https://github.com/Joshua-M-Data/BRIDGE-Project-`  
The analyses use publicly available AMP datasets (`subject_master.csv`, `retention.csv`, `reacto.csv`).
