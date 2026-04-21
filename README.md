# Access to Online Job Platforms and Earnings
## A Difference-in-Differences Evaluation

### 1. Research Question

Does access to an online job portal increase earnings for low-income job seekers? This project estimates the causal effect of job portal access on annual salaries among job seekers in India, using a staggered difference-in-differences design that exploits variation in the timing of platform access across cohorts.

### 2. Data

The analysis uses a synthetic panel dataset reflecting the structure of job seeker records maintained by BOOST (Building Opportunities and Outcomes through Skill Training), an NGO operating in India. The dataset covers 2,200 job seekers over 1999–2014 (35,200 observations), with treated cohorts gaining portal access in different years between 2006 and 2010.

The data were originally generated for pedagogical purposes at the University of Chicago Harris School of Public Policy (PPHA 34600, Fall 2024). They are designed to reflect realistic panel structures and treatment variation but do not represent actual BOOST records.

Key variables:
- `participant_id` — unique job seeker identifier
- `year` — calendar year (1999–2014)
- `salaries` — annual salary in rupees
- `jobportal_year` — year of first portal access (NA for never-treated)

### 3. Methodology

The analysis proceeds in four steps:

**Step 1 — Pre-treatment trends:** Average salary trajectories are plotted for the 2006 treatment cohort and the never-treated group to assess the parallel trends assumption visually.

**Step 2 — Main DiD estimates:** Three specifications are estimated on the 2006 cohort and never-treated group:
- Simple difference in means (2×2 DiD)
- OLS DiD with interaction (`ever_treated × post`)
- Two-way fixed effects (TWFE): `salaries ~ D | participant_id + year`

Standard errors are clustered at the individual level throughout.

**Step 3 — Event study:** An event study regression using `i(eventtime, ever_treated, ref = -1)` validates pre-trend flatness and traces the dynamic pattern of treatment effects relative to the year of portal access.

**Step 4 — Distributed lag model:** A distributed lag regression on the full staggered sample estimates the cumulative salary effect over five post-treatment years.

All regressions use `feols` from the `fixest` package in R.

### 4. Key Results

| Specification | Estimate |
|---|---|
| Simple DiD (difference in means) | ~60 rupees |
| OLS DiD | ~60 rupees |
| TWFE (main estimate) | ~60 rupees |
| Cumulative effect after 5 years | ~170 rupees |

- Pre-treatment event study coefficients are flat and indistinguishable from zero, supporting parallel trends.
- Post-treatment coefficients grow steadily, consistent with gradual improvement in job match quality.
- The cumulative 5-year effect (~170 rupees, ~8.5% of pre-treatment salary) suggests persistent and expanding returns to platform access.

### 5. Repository Structure

```
india-job-portal-did/
  india_job_portal_did.Rmd   — main analysis (R Markdown)
  india_job_portal_did.pdf   — rendered output
  data/
    job_portal_panel.csv     — data file (gitignored)
  PLAN.md                    — project notes
  .gitignore
```

### 6. How to Reproduce

1. Place `job_portal_panel.csv` in the `data/` folder (not tracked in this repo)
2. Open `india_job_portal_did.Rmd` in RStudio
3. Install required packages: `tidyverse`, `fixest`, `kableExtra`, `ggplot2`
4. Knit to PDF

---

*Analysis by Dong Liu. Data source: synthetic panel generated for pedagogical purposes at the University of Chicago Harris School of Public Policy (PPHA 34600, Fall 2024).*
