# Changelog (chronological, including claims that did not hold)

## Notebook 4 — LGG replication
Same 20-gene panel, same criteria, n = 513. Age-adjusted p = 0.353; random-set p = 0.797;
worst leave-one-out p = 0.806. Verdict: red. No replication.

## Notebook 3 — expanded panel (GBM)
20 expressed genes of 21 pre-declared. Age-adjusted p = 0.718 (n = 106); random-set p = 0.715.
Verdict: red.

## Notebook 2 — robustness of the 4-gene model
Age adjustment: p = 0.142 (n = 106; same patients unadjusted p = 0.040). SCN9A alone HR 1.26 per SD,
p = 0.017; Vmem without SCN9A p = 0.21. Random sets: empirical p = 0.026. Verdict: yellow.
Lesson: the 4-gene Vmem was mainly SCN9A.

## Notebook 1 — first look (GBM, 4 channel genes)
n = 154, 122 deaths. Cox HR 1.081 per mV, p = 0.007; KM log-rank p = 0.0425.
Interpreted as preliminary only, before any robustness test.

## Earlier fixes
- Linear expression ratio replaced by log2 scaling with clipping (extreme-value artefact).
- Early project modules (GHK engine, API fetch, pipeline) consolidated into self-contained notebooks.
- Review of earlier simulation papers found inconsistencies (e.g. direction of ivermectin's effect);
  these were corrected in the review report, not in this repository.

## Possible next steps (not done)
GTEx healthy reference; external cohort (CGGA); IDH-stratified LGG analysis; SCN9A as a stand-alone
prognostic candidate with proper pre-specification; wet-lab validation (DiBAC4(3) dye).
