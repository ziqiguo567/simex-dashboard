# A SIMEX-like Extrapolation Approach for Handling Left-Censored Exposure Data in Logistic Regression

**Author:** Ziqi Guo · **Advisor:** Prof. Robert Lyles
**Institution:** Rollins School of Public Health, Emory University
**Live dashboard:** <https://ziqiguo567.github.io/simex-dashboard/>
**Personal site:** <https://ziqiguo567.github.io/>

## Research topic

In environmental and occupational epidemiology, laboratory instruments cannot measure exposures below a **Limit of Detection (δ)**. Values that fall below δ are **left-censored non-detects**, and in real-world studies they can make up anywhere from a few percent to over 50% of observations. Failing to handle them properly biases log-odds-ratio estimates, distorts standard errors, and collapses 95% CI coverage.

This capstone compares **eight methods** for handling left-censored exposure data in logistic regression — Complete-case oracle, LOD / LOD/2 / LOD/√2 substitution, Drop (detects-only), MDI (missing-indicator), MLE (discriminant-function), and a **SIMEX-like extrapolation** approach — across:

- **Scenario 1 (correctly specified discriminant-function model):** n = 500, true β = 0.30, censoring 21% / 35% / 50%.
- **Scenario 2 (misspecified — mixture of normals exposure):** n = 1000, true β = 0.50, censoring 20% / 35% / 50%.

500 Monte Carlo replications per cell; evaluation metrics are bias, empirical SD, and 95% CI coverage.

## Why this matters

Left-censored exposure data from limits of detection are ubiquitous in environmental and occupational epidemiology; choosing the wrong correction method (e.g., LOD/2 substitution) can produce biases exceeding 40% of the true effect and invalid confidence intervals. This dashboard helps applied researchers pick a bias-correction method (MLE, MDI, or SIMEX) appropriate to their censoring level and distributional assumptions.

## Repository contents

```
.
├── SIMEX_flexdashboard.Rmd   # Source for the dashboard (this is the file to knit)
├── index.html                # Rendered dashboard served by GitHub Pages
└── README.md
```

The summary statistics shown in the two interactive widgets (Mean β̂, Bias, empirical SD, and 95% CI coverage for each method × censoring level × scenario) are embedded directly in the `.Rmd` as data frames, so the Rmd is self-contained and fully reproducible. The upstream SAS programs that generated those summary statistics via 500-replicate Monte Carlo simulation are not included in this repository.

## Reproduce the dashboard

```r
install.packages(c("flexdashboard", "plotly", "dplyr", "DT", "htmltools"))
rmarkdown::render("SIMEX_flexdashboard.Rmd", output_file = "index.html")
```

## Key findings (TL;DR)

| Scenario | Worst method at 50% censoring | Best method | SIMEX verdict |
|---|---|---|---|
| 1 (DF, correctly specified) | LOD / LOD/2 / LOD/√2 (bias = +0.140) | **MLE** (bias ≤ +0.002) | Near-zero bias; higher SD |
| 2 (mixture, misspecified) | LOD (bias = +0.803) | **MDI / Drop** | Beats naive + MLE when assumptions fail |

- **Avoid substitution methods when censoring exceeds ~20%.**
- **MLE is optimal only when the DF normality assumption holds.**
- **MDI is the most consistently reliable approach in this study.**
- **SIMEX is preferred when distributional assumptions are in doubt.**

## References

Key references (full list in the dashboard's *Conclusions* tab):

- Cook, J.R. & Stefanski, L.A. (1994). *JASA*, **89**(428), 1314–1328.
- Carroll et al. (2006). *Measurement Error in Nonlinear Models*, Chapman & Hall/CRC.
- Lyles, R.H., Guo, Y., & Hill, A.N. (2009). *The American Statistician*, **63**(4), 320–327.
- Chiou, Betensky, & Balasubramanian (2019). *Ann. Epidemiol.*, **38**, 57–64.
- Longford, N.T. (2012). *Stat. in Medicine*, **31**, 3133–3146.
- Gong, Q. (2025). MSPH Capstone, Rollins SPH, Emory University.

## Acknowledgment

Sincere gratitude to Prof. Robert Lyles for his guidance and continued encouragement throughout this project, and to the Rollins School of Public Health for providing a supportive academic environment.
