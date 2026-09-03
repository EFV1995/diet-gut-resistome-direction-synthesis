# Diet–gut resistome direction-of-evidence synthesis

Reproducible R Markdown workflow and curated study-level metadata for the quantitative direction-of-evidence synthesis used in the systematic review:

**Diet and the human gut resistome: A systematic review of metagenomic evidence on antibiotic resistance genes and microbial signatures**

This analysis was designed for heterogeneous metagenomic evidence for which a conventional pooled random-effects meta-analysis was not considered interpretable across studies because populations, dietary exposures, ARG quantification methods, normalization strategies, reference databases, and reported effect measures differed substantially.

## Repository files

```text
.
├── README.md
├── analysis/
│   └── diet_ARG_direction_of_evidence_synthesis.Rmd
└── data/
    └── meta_data_quantitative_synthesis.xlsx
```

The R Markdown workflow creates the output folders automatically when it is run.

## What the workflow does

The analysis:

1. reads the curated Excel workbook;
2. harmonizes study-level evidence into the three prespecified primary manuscript domains;
3. assigns one study-level direction-of-evidence judgement per study/domain as **supportive**, **null/mixed**, or **opposite**;
4. performs exact one-sided binomial sign tests as a secondary directional check;
5. estimates Bayesian posterior probabilities of supportive evidence using a neutral **Beta(1,1)** prior;
6. generates study-level harvest plots and Bayesian posterior-density plots; and
7. exports manuscript-ready tables and figures, including PNG, TIFF, and PDF files.

The analysis quantifies **consistency of evidence direction**, not biological effect magnitude. Posterior probabilities must therefore not be interpreted as percentage changes in ARG abundance.

## Primary synthesis domains

The final manuscript synthesis contains **14 studies** across three primary domains:

| Manuscript domain | Supportive / studies | Null/mixed | Opposite | Posterior mean support | 95% CrI | Exact one-sided sign-test p |
|---|---:|---:|---:|---:|---|---:|
| A) Breastfeeding, formula feeding and infant ARG dynamics | 6/6 | 0 | 0 | 0.88 | 0.59–1.00 | 0.015625 |
| B) Dietary interventions and probiotic/synbiotic modulation of ARGs | 4/4 | 0 | 0 | 0.83 | 0.48–0.99 | 0.062500 |
| C) Dietary behavioural patterns | 3/4 | 1 | 0 | 0.67 | 0.28–0.95 | 0.125000 |
| D) Primary diet-related domains combined, descriptive | 13/14 | 1 | 0 | 0.88 | 0.68–0.98 | 0.000122 |

The combined analysis is **secondary and descriptive only**. It summarizes consistency across the three primary manuscript domains and is not a pooled effect-size estimate.

### Why geographical studies are not included in the primary synthesis

The workbook retains three geographical/urbanization studies (ST15–ST17) for transparency and narrative/supplementary interpretation. They are not included in the primary direction-of-evidence synthesis because they assess broader geographical or ecological contrasts rather than a prespecified directional diet–ARG hypothesis.

### Intervention-domain harmonization

The extraction workbook deliberately retains the original distinction between `Intervention / probiotic` and `Intervention / diet` so the study coding remains auditable. In the R workflow these are harmonized into one manuscript domain, **Dietary interventions and probiotic/synbiotic modulation of ARGs**, which includes ST07–ST10. Thus, **Wu et al. 2016 (ST10)** contributes to the final 4/4 intervention-domain result.

## Statistical interpretation

For a domain with `k` supportive studies among `n` total studies, the conservative Bayesian synthesis uses:

```text
Posterior = Beta(1 + k, 1 + n - k)
```

Under this Bayesian specification, **null/mixed findings are retained as non-supportive evidence**. The reported posterior mean and 95% credible interval therefore reflect uncertainty in the probability that a study-level finding is supportive.

The exact binomial sign test is intentionally different: it compares **supportive versus opposite directional findings only**. Null/mixed findings are excluded from the sign-test denominator. This is why, for example, the behavioural domain has 3/4 supportive studies but a sign-test based on 3 directional findings.

Because study counts are small, sign-test p-values are interpreted descriptively and are not used as the sole basis for inference.

## Input workbook

`data/meta_data_quantitative_synthesis.xlsx`

The workbook contains the structured study-level extraction and analysis metadata used by the R workflow, including:

- study identifiers and citations;
- population and sample size;
- dietary exposure and study design;
- ARG outcomes and methods/databases;
- study-level direction-of-evidence coding;
- evidence directness and working risk-of-bias fields;
- outcome-level sensitivity data;
- effect-size candidates;
- pathway evidence; and
- the prespecified analysis plan and codebook.

Raw study-domain labels are preserved in the workbook for auditability and are harmonized programmatically in the R Markdown file.

## Running the analysis

### RStudio

1. Place `diet_ARG_direction_of_evidence_synthesis.Rmd` in `analysis/`.
2. Place `meta_data_quantitative_synthesis.xlsx` in `data/`.
3. Open the repository root as an RStudio project or working directory.
4. Open the R Markdown file and click **Knit**.

### R console

From the repository root:

```r
rmarkdown::render("analysis/diet_ARG_direction_of_evidence_synthesis.Rmd")
```

The default YAML parameters are:

```yaml
params:
  workbook: "../data/meta_data_quantitative_synthesis.xlsx"
  output_dir: "../outputs/generated"
  dpi_export: 1000
  install_missing_packages: true
```

## R package requirements

The workflow uses:

```text
readxl, dplyr, tidyr, stringr, ggplot2, forcats, scales,
purrr, tibble, readr, openxlsx, knitr, grid, tools, patchwork
```

If `install_missing_packages: true`, missing packages are installed from CRAN when the document is rendered.

## Main figure conventions

In the Bayesian posterior-density panel:

- **solid vertical line** = posterior mean;
- **dashed vertical lines** = 95% credible interval;
- **dotted vertical line** = reference value of 0.50.

In the study-level harvest plot:

- point colour represents direction of evidence;
- point shape represents evidence directness; and
- point size is proportional to study sample size, using capped square-root scaling for display.

## Transparency and scope

- No overall conventional random-effects pooled diet effect is reported.
- Bayesian posterior summaries quantify directional consistency, not effect magnitude.
- Outcome-level analyses are sensitivity analyses because multiple outcomes from one study are statistically non-independent.
- Extractable effect sizes are retained as descriptive quantitative anchors and are not pooled unless a sufficiently comparable set of estimates with uncertainty becomes available.
- Geographical/urbanization studies remain in the workbook and supplementary audit outputs but are excluded from the three primary directional domains for the reason stated above.

## Suggested Methods wording

Reproducible analyses were conducted in R using an R Markdown workflow. The script reads the structured extraction and coding workbook, harmonizes studies into the prespecified manuscript domains, applies study-level direction-of-evidence rules, calculates exact binomial and Bayesian beta-binomial summaries, and exports the tables and figures used in the manuscript. The R Markdown script and curated extraction workbook are provided in this repository to support transparency and reproducibility.

## Citation

Please cite the associated systematic review when using this repository. Final DOI and journal citation not available yet.

## License

## License

Code in this repository is released under the MIT License.

The curated extraction workbook, tables, figures, and manuscript-supporting content are released under CC BY 4.0, unless otherwise stated and only where permitted by the authors, journal, and institution.
