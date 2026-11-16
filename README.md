# Wheat yield forecasting with historical residual correction

Code for the study **"Evaluating historical residual correction and learned reliability for cost-sensitive early regional wheat yield forecasting"** (manuscript submitted to *Scientific Reports*).

The notebook `wheat-yield-forecasting-2026.ipynb` reproduces all results, tables and figures of the study. It is stored with its outputs, so the results can be read without running it.

## Overview

- **Data:** CY-Bench subnational wheat records with weather, vegetation and soil predictors.
- **Countries:** seven data-rich countries (AR, BR, DE, FR, IN, IT, US), selected from 29 by an independent K-means screening.
- **Forecast cutoffs:** λ = 0.25, 0.50 and 0.75 of the growing season (larger λ gives an earlier forecast).
- **Evaluation:** chronological; every outer test season uses only earlier seasons for fitting, tuning and historical residuals (252 folds, 114,548 held-out forecasts).
- **Metrics:** MAE, RMSE, 90% prediction intervals, cost-sensitive decision loss at cost ratios 1:1 to 8:1, and country–year clustered paired tests.

| Identifier | Configuration |
|---|---|
| C1 | Primary XGBoost forecast, no correction |
| C2 | Historical residual correction |
| C3 | Reliability-conditioned forecast |
| C4 | Reliability model without stress features |
| C5 | Full reliability model with cost-sensitive decision |
| B1 | Quantile XGBoost |
| B2 | Conformalized quantile regression |
| B3 | Quantile regression forest (tuned) |
| B4a | Yield persistence |
| B4b | Yield persistence with residual correction |

## Data

The data are **not** included in this repository. Download CY-Bench from its official archive:

> Kallenberg, M. et al. CY-Bench: a comprehensive benchmark dataset for sub-national crop yield forecasting. Zenodo (2025). https://doi.org/10.5281/zenodo.11502142

The notebook expects the wheat folders in this layout:

```
wheat/
  AR/
    yield_wheat_AR.csv
    crop_calendar_wheat_AR.csv
    meteo_wheat_AR.csv
    ndvi_wheat_AR.csv
    fpar_wheat_AR.csv
    soil_moisture_wheat_AR.csv
    soil_wheat_AR.csv
  BR/
  ...
```

Section 0 reads all 29 wheat country folders; Sections 1–9 use the seven selected countries. Use the data under the licence stated in the CY-Bench record.

## How to run

**On Kaggle (recommended):**

1. Open the notebook on Kaggle: https://www.kaggle.com/code/wakeupsahitya/wheat-yield-forecasting-2026
2. Attach the CY-Bench wheat folders as input (dataset `wakeupsahitya/cybench-wheat`).
3. Select a GPU accelerator under **Settings → Accelerator**.
4. Choose **Run All**.

The main experiment (Section 4) took about 27 minutes on Kaggle Tesla T4 GPUs; the full notebook takes longer because of the baselines and sensitivity analyses.

**On another machine:** install the packages in `requirements.txt` on a system with an NVIDIA GPU and CUDA, then set `CANDIDATE_ROOTS` in the first cells of Sections 0 and 1 to the folder that contains `wheat/`. The output folders are set to `/kaggle/working/` and fall back to a local folder where the code provides one.

The comparison cell in Section 7 is optional. It compares the run with an earlier independent run attached as a separate Kaggle dataset and can be skipped.

## Outputs

The notebook writes all tables (CSV), figures (PNG), manifests and a run report (`RUN_REPORT.md`, `pip_freeze.txt`) and bundles them into a downloadable archive in Section 8.

## Reproducibility

- Random seed: 42
- Recorded environment: Python 3.12, NumPy 2.0.2, pandas 2.3.3, SciPy 1.16.3, scikit-learn 1.6.1, XGBoost 3.2.0
- Two consecutive executions produce identical results; the notebook checks source-file checksums, temporal eligibility and an independent reconstruction of sampled features.

## Citation

If you use this code, please cite the article (details will be added after publication) and the archived code release (see `CITATION.cff`).

## Licence

The code is released under the MIT Licence (see `LICENSE`). The CY-Bench data remain under their own licence.
