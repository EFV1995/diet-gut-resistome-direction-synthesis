# diet-gut-resistome-direction-synthesis
Reproducible R Markdown workflow for a SWiM-compatible Bayesian/binomial direction-of-evidence synthesis of diet, gut microbiota, and antibiotic resistance genes.

# Diet and the human gut resistome: reproducible quantitative direction-of-evidence synthesis

This repository contains the reproducible analysis workflow for the quantitative direction-of-evidence synthesis used in the systematic review:

**Diet and the human gut resistome: A systematic review of metagenomic evidence on antibiotic resistance genes and microbial signatures**

The workflow was designed for heterogeneous metagenomic evidence where a conventional random-effects meta-analysis was not interpretable because the included studies differed in life stage, dietary exposure definition, ARG quantification method, normalization strategy, reference database, and reported effect measure.

## What this repository reproduces

The R Markdown workflow:

1. reads the structured extraction and coding workbook;
2. applies the prespecified study-level scoring rules;
3. classifies one primary finding per study/domain as **supportive**, **null/mixed**, or **opposite**;
4. calculates exact one-sided binomial sign tests;
5. calculates Bayesian beta-binomial direction-of-evidence summaries using a neutral Beta(1,1) prior;
6. exports manuscript-ready tables, figures, captions, and interpretation text.

The analysis estimates **evidence consistency**, not biological effect magnitude. Posterior probabilities should not be interpreted as percentage reductions/increases in ARG abundance.

## Repository structure

```text
.
├── README.md
├── run_analysis.R
├── requirements.R
├── analysis/
│   └── diet_ARG_direction_of_evidence_synthesis.Rmd
├── data/
│   ├── diet_ARG_quantitative_synthesis_extraction.xlsx
│   └── README.md
└── outputs/
    ├── tables/
    ├── figures/
    ├── manuscript_text/
    └── generated/
```

## Main input file

`data/diet_ARG_quantitative_synthesis_extraction.xlsx`

This workbook contains the study-level extraction, coding decisions, prespecified domains, direction-of-evidence labels, effect-size candidates, risk-of-bias/directness fields, and codebook used by the R Markdown workflow.

## Main script

`analysis/diet_ARG_direction_of_evidence_synthesis.Rmd`

The R Markdown file is the full reproducible script. It was developed and rendered in R through RStudio. The default parameters in the YAML header assume the repository layout shown above:

```yaml
params:
  workbook: "../data/diet_ARG_quantitative_synthesis_extraction.xlsx"
  output_dir: "../outputs/generated"
  dpi_export: 1000
  install_missing_packages: true
```

## How to run the analysis

### Option 1: RStudio

1. Open the repository folder in RStudio.
2. Open `analysis/diet_ARG_direction_of_evidence_synthesis.Rmd`.
3. Click **Knit**.

### Option 2: R console

From the repository root, run:

```r
source("run_analysis.R")
```

### Option 3: Manual render

```r
rmarkdown::render(
  input = "analysis/diet_ARG_direction_of_evidence_synthesis.Rmd",
  output_dir = "outputs/rendered_report"
)
```

## R package requirements

The workflow uses:

```r
readxl, dplyr, tidyr, stringr, ggplot2, forcats, scales,
purrr, tibble, readr, openxlsx, knitr, grid, tools, patchwork, rmarkdown
```

To install them manually:

```r
source("requirements.R")
```

## Expected primary results

| Manuscript section | Supportive / studies | Null/mixed | Opposite | Posterior mean support | 95% CrI | Exact sign-test p |
|---|---:|---:|---:|---:|---|---:|
| A) Breastfeeding, formula feeding and infant ARG dynamics | 6/6 | 0 | 0 | 0.88 | 0.59–1.00 | 0.016 |
| B) Dietary interventions and probiotic/synbiotic modulation of ARGs | 3/3 | 0 | 0 | 0.80 | 0.40–0.99 | 0.125 |
| C) Dietary behavioural patterns | 3/4 | 1 | 0 | 0.67 | 0.28–0.95 | 0.125 |
| D) All domains combined, descriptive | 12/13 | 1 | 0 | 0.87 | 0.66–0.98 | <0.001 |

The all-domains combined analysis is **secondary and descriptive only**. It combines the three primary manuscript sections A–C and is not a pooled diet effect.

## Interpretation

Overall, the evidence shows strong and consistent study-level support that diet-related exposures are linked to gut resistome modulation, particularly in early-life feeding and probiotic/synbiotic interventions, while adult dietary behavioural evidence is supportive but more heterogeneous. These findings indicate consistency of direction, not effect magnitude.

## Main outputs

After running the workflow, outputs are written to:

```text
outputs/generated/
├── tables/
├── figures/main_article/
├── figures/supplement/
└── manuscript_text/
```

A copy of the latest generated/curated outputs is also included in:

```text
outputs/tables/
outputs/figures/
outputs/manuscript_text/
```

## Notes on transparency and scope

- No conventional pooled random-effects estimate is reported as a main result.
- Exact binomial sign tests are interpreted descriptively because the number of studies per domain is small.
- The Bayesian credible intervals quantify uncertainty around the posterior probability of supportive evidence.
- Outcome-level analyses are sensitivity analyses only because multiple outcomes from the same study are statistically non-independent.
- Extractable effect sizes are shown descriptively as quantitative anchors and are not pooled.

## Suggested Methods wording

Reproducible analyses were conducted in R through RStudio using an R Markdown workflow. The script reads the structured extraction and coding workbook, applies the prespecified study-level scoring rules, calculates the exact binomial and Bayesian beta-binomial direction-of-evidence summaries, and exports all tables, figures, and interpretation text used in the manuscript. The R Markdown script, extraction workbook, and derived outputs have been made available in this repository to support transparency and reproducibility.

## Citation

Please cite the associated manuscript when using this repository. Add the final DOI or journal citation here once available.

## License

Add the preferred license before public release, according to the journal and institutional requirements.

