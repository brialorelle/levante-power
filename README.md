# Vocabulary ICC and power for a LEVANTE border cohort

How stable are LEVANTE task scores between children of the same age, and what
does that imply for the power of a three-wave study relating an environmental
index to children's learning?

Everything is in one R Markdown document, [`vocab_icc.Rmd`](vocab_icc.Rmd),
which downloads the data, estimates the ICC, runs the power simulations, and
writes out the proposal text. Render it to get `vocab_icc.html`.

## What it finds

**Vocabulary, Leipzig pilot** (455 scores from 293 children aged 5–13, 162
retested a median of 3.6 months apart):

| Quantity | Value |
|---|---|
| ICC among children of the same age | **.46** (95% CI .34–.57) |
| Test–retest correlation | .75 raw, .41 once age is adjusted for |
| Within-age SD of scores | 0.65 logits |
| Growth with age | 0.27 logits/year |

**Power for three annual waves, 20% attrition, α = .05**, from 1,000 `simr`
simulations per setting:

| Enrolled N | Smallest detectable growth effect | In months of vocabulary growth by wave 3 |
|---|---|---|
| 300 | .098 SD/year | 5.7 |
| 320 | .090 | 5.2 |
| 360 | .086 | 5.0 |

The effect is the association between a standardised environmental index and
each child's growth rate. The analytic formula gives slightly smaller values
(.094 at N = 300); the two agree to within simulation error, and both are
reported.

**Across the battery**, Math is the most sensitive outcome (ICC .66, smallest
detectable effect .078 at N = 300, worth about 3.6 months of growth), and the
executive function tasks are the weakest (ICC .31–.37).

## Three things that change the answer

- **Retest gap.** These ICCs come from retests 3.6 months apart; the study
  plans annual waves. Across tasks, sites with 15.6-month gaps run about .35
  against .46 for short gaps, so the headline ICC is a little optimistic.
- **Test length.** The ICC is mostly a measurement-precision story. The adaptive
  Vocabulary test as administered (35 items) implies an ICC near .40; the
  130-item form implies .55. A longer test buys more than extra children do.
- **Changing task version between waves.** It did not add measurable noise:
  children's score changes were smaller than their own measurement errors
  predict.

## Reproducing

The data are not in this repository. They are the public LEVANTE pilot release
on [Redivis](https://stanford.redivis.com/datasets/68kn-csrddrz5x) and are
covered by the LEVANTE data use agreement, so `data/` is git-ignored.

1. Create a Redivis account and sign the
   [LEVANTE data use agreement](https://researcher.levante-network.org/data).
2. Create an API token with the `data.data` scope (Redivis: Workspace →
   Settings → API tokens) and put it in `~/.Renviron` as
   `REDIVIS_API_TOKEN=...`.
3. Render the document, which downloads `data/scores.csv` on the first run:

```r
rmarkdown::render("vocab_icc.Rmd")
```

Needs R with `dplyr`, `readr`, `ggplot2`, `lme4`, `simr`, `httr2` and
`rmarkdown`. The first render takes about 15 minutes, almost all of it the
power simulations; they are cached, so later renders are quick. The document
checks its own reproducibility: it re-runs a bootstrap and a simulation with
the same seed and confirms the results match.

## Caveats

- One site (Leipzig, German-language scores), ages 5–13. The border cohort is
  4–8 and Spanish/English.
- The between-child SD of growth rates (.15 SD/year) and the target effect
  (.08 SD/year) are assumptions, not estimates from these data.
- Bogotá has too few retested children on Vocabulary (29) to estimate its ICC
  reliably.

## Data source and citation

Analyses use the public LEVANTE pilot data release
(<https://stanford.redivis.com/datasets/68kn-csrddrz5x>), licensed
[CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). No
participant data are redistributed here: the figures and tables are derived
summaries, and `data/` is git-ignored.

- Frank, M. C., Baumgartner, H. A., Braginsky, M., Kachergis, G., et al. (2025).
  Learning Variability Network Exchange (LEVANTE): A global framework for
  measuring children's learning variability through collaborative data sharing.
  *Child Development, 96*(6), 1867–1884. <https://doi.org/10.1111/cdev.70011>
- Kachergis, G., O'Reilly, et al. (2025). Creation and validation of the LEVANTE
  core tasks: Internationalized measures of learning and development for
  children ages 5–12 years. *PsyArXiv*.
  <https://doi.org/10.31234/osf.io/r4dhw_v1>

Claude Code was used to help write and check these analyses.
