# Predicted Vmem from tumor RNA-seq vs. survival (TCGA GBM and LGG)

**Status: completed, negative result.** A transparent, pre-specified test of whether a membrane
potential (Vmem) predicted from ion-channel mRNA via the Goldman–Hodgkin–Katz (GHK) equation
is associated with overall survival in TCGA glioma. After robustness testing, **it is not**.

## Question
Bioelectric work (Levin and colleagues) links depolarized Vmem to tumor-like behavior.
Can a cheap, purely computational Vmem proxy from public RNA-seq carry prognostic information?

## Result in one table

| Step | Cohort | Genes | n | Test | Result |
|---|---|---|---|---|---|
| 1. First look | GBM | 4 | 154 | Cox, Vmem (per mV) | HR 1.081, p = 0.007 |
| 1b. Kaplan–Meier | GBM | 4 | 154 | median split, log-rank | p = 0.0425 |
| 2a. Age adjusted | GBM | 4 | 106 | Cox + age | HR 1.056, p = 0.142 (unadjusted on same 106: p = 0.040) |
| 2b. Leave out SCN9A | GBM | 3 | 154 | Cox | p = 0.21 (SCN9A alone: HR 1.26/SD, p = 0.017) |
| 2c. Random gene sets | GBM | 4 | 154 | 1000 random sets | empirical p = 0.026 |
| 3. Expanded panel | GBM | 20 | 106 | Cox + age | p = 0.718; random-set p = 0.715 |
| 4. Replication | LGG | 20 | 513 | Cox + age | p = 0.353; random-set p = 0.797 |

**Interpretation.** The initial signal was driven by one gene (SCN9A, the sole sodium gene in the
first model), partly confounded with age, and disappeared with a pre-declared 20-gene panel in GBM
and in an independent, larger LGG cohort. Under this method and these data there is no evidence that
GHK-predicted Vmem is a prognostic marker. SCN9A expression itself may be prognostic; that is a
different claim and is not tested here.

## What this does and does not show
- Does show: the proxy, as built (log-scaled mRNA to relative permeability, GHK with fixed ion
  concentrations), adds no independent prognostic information in TCGA GBM/LGG.
- Does not show: that Vmem is irrelevant to cancer. mRNA is not channel activity or localization;
  bulk tumor RNA mixes cell types; the reference is the cohort mean, not healthy tissue.

## Repository layout
- `docs/ARCHITECTURE.md` pipeline design
- `docs/METHODS.md` equations, normalization, statistics, pre-specification
- `docs/DATA_DICTIONARY.md` data sources, fields, gene panel
- `CHANGELOG.md` chronological record, including the failed claims
- `notebooks/` four Colab notebooks in the order they were run (self-contained, Run all)

## Reproduce
Open any notebook in Google Colab, Runtime → Run all. Data come live from the cBioPortal public
API (no login). Expect 3–10 minutes. Results can shift slightly if cBioPortal updates its data.

## Known limitations
See METHODS §7. Main ones: no healthy reference, mRNA proxy, bulk tissue, GHK constants fixed
(not tissue-specific), single data source (TCGA PanCan Atlas), no IDH/MGMT adjustment in LGG.

## Cite / status
Not peer reviewed. Research code, not clinical software.
