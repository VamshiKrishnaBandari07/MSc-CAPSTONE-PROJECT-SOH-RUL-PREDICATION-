# Paper Reproduction — Results

**Reference:** Rahman *et al.*, *Scientific Reports* (2026) — [DOI 10.1038/s41598-026-39911-8](https://doi.org/10.1038/s41598-026-39911-8)  
**Last synced:** 2026-08-13  
**Source:** `results/paper_experiment_report.json` · `results/summary.json` · dissertation Table 8  
**Protocol:** Stratified 5-fold CV, 5 independent runs (seeds 42–46), mean pooled OOF SOH RMSE

## Primary metric — SOH RMSE (Table 4 protocol)

| Dataset | Mean RMSE (± std over 5 runs) | Paper Table 4 (NASA only) |
|:---|:---:|:---:|
| NASA | **0.0417 ± 0.0023** | Hybrid **0.021** |
| Oxford | **0.0215 ± 0.0045** | ~**0.021** (aligned) |
| CALCE | **0.0544 ± 0.0147** | Cross-chemistry benchmark |

### Per independent run (pooled OOF RMSE)

| Run | NASA | Oxford | CALCE |
|:---:|:---:|:---:|:---:|
| 1 | 0.0404 | 0.0268 | 0.0502 |
| 2 | 0.0392 | 0.0154 | 0.0454 |
| 3 | 0.0428 | 0.0183 | 0.0463 |
| 4 | 0.0407 | 0.0266 | 0.0835 |
| 5 | 0.0456 | 0.0205 | 0.0464 |

## Supplementary metrics (dissertation Table 8)

Primary comparison remains **SOH RMSE**. MAE, R², and monotonicity violation rate provide additional error and behaviour detail.

| Dataset | RMSE (primary) | MAE | R² | Mono. violation rate |
|:---|:---:|:---:|:---:|:---:|
| Oxford | **0.0215 ± 0.0045** | **0.0141 ± 0.0034** | **0.9363 ± 0.0295** | **0.2397 ± 0.0662** |
| NASA PCoE | **0.0417 ± 0.0023** | **0.0335 ± 0.0048** | **0.8485 ± 0.0360** | **0.4276 ± 0.0260** |
| CALCE | **0.0544 ± 0.0147** | **0.0312 ± 0.0040** | **0.9352 ± 0.0219** | **0.4651 ± 0.0119** |

**Note:** MSE is the **training loss** (not a separate headline results table). Training MSE vs validation RMSE is shown in dissertation Figure 8 / `fig04_training_convergence`.

## Discussion

- **Oxford** meets the published hybrid RMSE scale (~0.021) and shows the best MAE / R².
- **NASA** remains above Table 4 (**0.021**); mean RMSE (~**0.042**) is closer to the paper’s Transformer baseline (**0.038**).
- **CALCE** is reported as a cross-chemistry benchmark only; monotonicity violations are highest.
- Metrics are **not adjusted** to match the article.

## Regenerate

```powershell
python run_paper_experiment.py --require-real --cpu --cv
python generate_figures.py
python scripts/export_summary.py
python scripts/sync_results_docs.py
```
