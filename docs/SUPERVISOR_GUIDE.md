# Supervisor guide — paper reproduction only

**Student:** Vamshi Krishna Bandari · MSc AI, University of Roehampton  
**Paper:** [Scientific Reports 16, 9871 (2026)](https://www.nature.com/articles/s41598-026-39911-8) · DOI [10.1038/s41598-026-39911-8](https://doi.org/10.1038/s41598-026-39911-8)

## Scope

GitHub holds **only** Rahman et al. (2026) **SOH** reproduction. MSc joint SOH+RUL work is in `local_archive/` (not pushed).

## Point-by-point checklist

1. Three datasets loaded — NASA, Oxford, CALCE  
2. SOH = Q_k / Q_BoL per cell  
3. ICA, DV, DC + SG(15,3) + 300-point grid  
4. Hybrid CNN–TCN–LSTM–attention  
5. Stratified **5-fold CV**, **five independent runs** (seeds 42–46)  
6. Report JSON lists all three datasets with pooled OOF RMSE  
7. Figures **fig01–fig04** present (no RUL / legacy fig05–07 on GitHub)  
8. No MSc scripts in repo root  

Run: `python scripts/verify_repo.py`

## Results vs Table 4 (NASA PCoE)

| Metric | Paper (hybrid) | Reproduction (5 runs, mean ± std) |
|:---|:---:|:---:|
| SOH RMSE | **0.021** | **0.0417 ± 0.0023** (does not match Table 4; near Transformer baseline **0.038**) |
| Oxford SOH RMSE | ~0.021 | **0.0215 ± 0.0045** (aligns with paper scale) |
| CALCE SOH RMSE | — | **0.0544 ± 0.0147** (cross-chemistry benchmark only) |

Metrics are taken from `results/paper_experiment_report.json` / `results/summary.json` and are **not adjusted** to match the article.

### Per independent run (pooled OOF SOH RMSE)

| Run | NASA | Oxford | CALCE |
|:---:|:---:|:---:|:---:|
| 1 | 0.0404 | 0.0268 | 0.0502 |
| 2 | 0.0392 | 0.0154 | 0.0454 |
| 3 | 0.0428 | 0.0183 | 0.0463 |
| 4 | 0.0407 | 0.0266 | 0.0835 |
| 5 | 0.0456 | 0.0205 | 0.0464 |

### Supplementary metrics (dissertation Table 8)

| Dataset | MAE | R² | Mono. violation rate |
|:---|:---:|:---:|:---:|
| Oxford | 0.0141 ± 0.0034 | 0.9363 ± 0.0295 | 0.2397 ± 0.0662 |
| NASA | 0.0335 ± 0.0048 | 0.8485 ± 0.0360 | 0.4276 ± 0.0260 |
| CALCE | 0.0312 ± 0.0040 | 0.9352 ± 0.0219 | 0.4651 ± 0.0119 |

**Primary comparison metric remains SOH RMSE.** MSE is the training objective (not a separate headline results table).

## Known limitations (for viva)

- **NASA:** Cycle-level stratified CV (not GroupKFold by cell); Q-profile construction may differ from the paper’s internal pipeline.  
- **Oxford:** Uses **characterisation (C1ch)** cycles, not discharge profiles.  
- **Monotonicity:** Non-zero violation rates on all datasets (highest on CALCE/NASA); no post-hoc isotonic smoothing applied.

## Reproduce

```powershell
git lfs pull
pip install -r requirements.txt
python run_paper_experiment.py --require-real --cpu --cv
python generate_figures.py
python scripts/export_summary.py
python scripts/sync_results_docs.py
python scripts/verify_repo.py
```

Long run on CPU (~7 h): `python scripts/run_train_and_eval.py`
