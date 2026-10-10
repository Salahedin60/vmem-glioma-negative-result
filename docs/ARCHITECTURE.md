# Architecture

No machine learning is used. The pipeline is analytic and deterministic except for resampling steps
(seeded).

```
cBioPortal REST API ──► expression (RSEM, per sample) + clinical (OS, AGE, per patient)
          │
          ▼
 Normalization: log2(x+1) / log2(ref+1), clipped to [1/5, 5]; ref = cohort mean per gene
          │
          ▼
 Role aggregation: mean scale per ion role (Na, K, Cl)
          │
          ▼
 GHK engine: scaled permeabilities → Vmem (mV) per patient
          │
          ▼
 Statistics: Cox PH (± age), leave-one-gene/ion-out, bootstrap CI, random-gene-set null
          │
          ▼
 Pre-declared verdict (4 criteria) → ✅/⚠️ and 🟢/🟡/🔴
```

## Components
| Component | Where | Notes |
|---|---|---|
| GHK engine | notebook cell "engine" (`ghk_vmem`, `scale_matrix`, `vmem_from_roles`) | vectorized over patients |
| Data access | cell "data" (`entrez_map`, `fetch_molecular`, `to_patient_index`) | API only, no files needed |
| Statistics | `lifelines` CoxPHFitter | standard errors from the partial likelihood |
| Null model | 1000 random expressed gene sets with the same Na/K/Cl structure | gives empirical p |

## Design decisions
1. **Self-contained notebooks** instead of a package: the user runs on Colab with one click.
2. **Pre-declared panel and criteria** before looking at outcomes (see METHODS §6).
3. **Random-gene-set null** because Vmem is a nonlinear function of a few genes, and a parametric
   p-value alone can mislead.
4. **Optional future layers** (not built): inverse problem (drug combinations to shift Vmem) and a UI.
   Neither is justified until a forward signal exists.

## Not implemented
Healthy-tissue reference (GTEx), tissue-specific ion concentrations, gap-junction coupling,
BETSE-style spatial simulation.
