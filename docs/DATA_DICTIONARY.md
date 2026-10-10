# Data dictionary

## Source
cBioPortal public API, `https://www.cbioportal.org/api`. No credentials.

| Item | Value |
|---|---|
| GBM study | `gbm_tcga_pan_can_atlas_2018` |
| LGG study | `lgg_tcga_pan_can_atlas_2018` |
| Expression profile / sample list | `<study>_rna_seq_v2_mrna` (RSEM, mRNA) |
| Clinical (patient level) | OS_STATUS, OS_MONTHS, AGE |

## Derived fields
| Name | Meaning | Unit |
|---|---|---|
| patientId | first 12 chars of TCGA sample barcode | text |
| OS_MONTHS | overall survival time | months |
| event | 1 if OS_STATUS starts with "1" (deceased), else 0 | binary |
| AGE | age at diagnosis (as in cBioPortal) | years |
| scale | normalized expression ratio per gene | unitless, 0.2–5 |
| vmem / x | predicted membrane potential | mV |
| z | Cox Wald z of Vmem | unitless |

## Gene roles
| Role | Genes (panel) |
|---|---|
| Na (inward leak/channels) | SCN9A, SCN1A, SCN2A, SCN3A, SCN8A |
| K (outward, hyperpolarizing) | KCNJ10, KCNJ2, KCNJ4, KCNJ11, KCNK1, KCNK3, KCNMA1, KCNN4, KCNH2, KCNQ1 |
| Cl | CLIC1, CLIC4, CLCN2, CLCN3, ANO1, CFTR |
| Not in GHK (first notebook only) | GJA1, GJB2 (gap junctions) |

Note: role labels are a simplification; several of these genes (e.g. CLIC1, KCNN4, KCNMA1) have
non-canonical or context-dependent functions.

## Data quality notes
- Of ~581 GBM patients with clinical data, only ~155 have RNA-seq; ~106 also have age.
- Genes with mean RSEM ≤ 20 or > 5% missing are excluded from the panel and the random pool.
- Outputs written by notebooks: `gbm_predicted_vmem.csv`, `km_vmem.png`, `null_distribution.png`,
  `null_panel.png`.
