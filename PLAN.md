# India Job Portal DiD — Project Plan

## Purpose

A standalone program evaluation code sample built for the World Bank DECDI Research Assistant application (deadline: April 29, 2026). Demonstrates proficiency in DiD methods, TWFE, event study, and distributed lags using R/fixest.

## Data

- Source: Synthetic panel data from UChicago Harris PPHA 34600 (Program Evaluation), Problem Set 2, Fall 2024.
- File: `data/job_portal_panel.csv` (gitignored)
- Coverage: 2,200 job seekers, 1999-2014 (35,200 obs)
- Treatment variation: cohorts gained portal access in 2006, 2007, 2008, 2009, 2010
- Outcome: annual salary in rupees
- Key variables: `participant_id`, `year`, `salaries`, `jobportal_year`

## Methods

1. 2x2 DiD (difference in means) — baseline check
2. OLS DiD with interaction (`ever_treated * post`) — no FE
3. TWFE (`salaries ~ D | participant_id + year`) — main estimate
4. Event study (`i(eventtime, ever_treated, ref = -1) | FE`) — pre-trend validation + dynamic effects
5. Distributed lag regression — cumulative 5-year effect

All regressions use `feols` from fixest. Standard errors clustered at `participant_id`.

## Key Results

- TWFE ATT: ~60 rupees (~3% relative to pre-treatment mean)
- Pre-trends: flat in event study (supports parallel trends)
- Cumulative 5-year effect: ~63 rupees (~3.2%)
- Pattern: effect grows steadily post-treatment, consistent with gradual match quality improvement

## Status

- [x] Rmd written and knitting
- [x] etable fixed (keep_raw for interaction + D)
- [ ] Final PDF rendered and reviewed
- [ ] Git init + initial commit
- [ ] Add to World Bank application materials

## Files

```
india-job-portal-did/
  india_job_portal_did.Rmd   — main analysis
  india_job_portal_did.pdf   — rendered output (gitignored)
  data/
    job_portal_panel.csv     — gitignored
  PLAN.md                    — this file
  .gitignore
```

## Notes

- Data note in Rmd discloses synthetic/pedagogical origin of dataset.
- Project framed as independent analysis, not as a problem set submission.
- Reuse for other applications where R/program evaluation code sample is requested.
