# Methods

## 1. Membrane potential (GHK voltage equation)
Vmem = (RT/F) · ln[(P_K[K]o + P_Na[Na]o + P_Cl[Cl]i) / (P_K[K]i + P_Na[Na]i + P_Cl[Cl]o)]

- RT/F = 26.73 mV at 310.15 K (R = 8.314 J/mol/K, F = 96485 C/mol)
- Concentrations (mM): K o/i = 5/140; Na o/i = 145/15; Cl o/i = 110/10
  (generic mammalian values, not glioma-specific)
- Baseline relative permeabilities: P_K : P_Na : P_Cl = 1 : 0.02 : 0.10 → Vmem ≈ −75.7 mV
- Self-check: raising P_Na ×10 gives ≈ −39.6 mV (matches hand calculation)

## 2. Expression to permeability scale
For gene g in patient i with cohort mean m_g:
scale = clip( log2(x_ig + 1) / log2(m_g + 1), 1/5, 5 ). Missing → 1.0.
Reason: raw RSEM is extremely right-skewed; a linear ratio produced implausible scales (fixed early).
Scales are averaged within each role (Na, K, Cl) and multiply the baseline permeability.
**Limitation:** this is an assumption (mRNA ∝ permeability), not a measured relationship.

## 3. Cohorts
TCGA PanCan Atlas via cBioPortal: `gbm_tcga_pan_can_atlas_2018` (155 patients with RNA-seq;
106 with age and survival), `lgg_tcga_pan_can_atlas_2018` (513 analyzed). Patient ID = first
12 characters of the sample barcode; duplicates dropped (first kept).

## 4. Survival statistics
- Endpoint: overall survival (OS_MONTHS, event = OS_STATUS starts with "1").
- Cox proportional hazards (lifelines); reported HR per mV unless stated.
- Age-adjusted model: covariates Vmem + AGE.
- Proportional-hazards assumption was **not** formally tested (limitation).

## 5. Robustness tests
1. Leave-one-gene-out and leave-one-ion-out Vmem, re-fit with age.
2. Bootstrap (1000 patient resamples), percentile 95% CI of HR.
3. Null: 1000 random gene sets from the expressed pool (mean > 20, > 95% non-missing, ~5000
   genes), same Na/K/Cl counts as the panel; empirical p = (1 + #{|z_null| ≥ |z_obs|}) / (1 + N).
   One-sided p is also reported for the hypothesis direction.

## 6. Pre-specification (stated before running notebooks 3 and 4)
- Panel (21 genes defined, 20 expressed): Na: SCN9A, SCN1A, SCN2A, SCN3A, SCN8A;
  K: KCNJ10, KCNJ2, KCNJ4, KCNJ11, KCNK1, KCNK3, KCNMA1, KCNN4, KCNH2, KCNQ1;
  Cl: CLIC1, CLIC4, CLCN2, CLCN3, ANO1, CFTR. Only an expression filter was applied.
- Primary endpoint: age-adjusted Cox p < 0.05.
- Verdict criteria: (1) primary p < 0.05; (2) random-set p < 0.05; (3) bootstrap CI excludes 1;
  (4) worst leave-one-gene-out p < 0.10. Green = 4/4, yellow = 2–3, red = 0–1.
- Notebook 4 reused the same panel and criteria without change.

## 7. Limitations
1. mRNA is not channel abundance, localization, gating or modulation.
2. Reference = cohort mean, not healthy tissue; Vmem is therefore relative, not absolute.
3. Bulk tumor: mixed tumor, glia, immune and vascular cells.
4. Fixed ion concentrations and permeability ratios; no tissue-specific calibration.
5. The first model had one sodium gene; its early signal was an SCN9A effect.
6. Gap junctions (GJA1, GJB2) were excluded from GHK and not modeled.
7. LGG: IDH, 1p/19q and grade were not adjusted; age only.
8. Multiple looks: the first p = 0.007 was found before any correction; later steps were
   confirmatory and are the ones to trust.
9. TCGA is a single source; no external validation (e.g. CGGA) was done.

## 8. Software
Python 3, pandas, numpy, matplotlib, lifelines, requests (all installed by the notebooks).
Random seeds: 7 (null), 11 (bootstrap), 2026 (gene pool).
