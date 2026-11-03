# A Charging-Data Digital Twin for IoT-Connected Lithium-Ion Batteries

Code for the paper **"A Charging-Data Digital Twin for IoT-Connected Lithium-Ion Batteries: Health, Charge and Remaining-Life Estimation with Calibrated Uncertainty and Tolerance to Sensor Faults"**.

The digital twin estimates the **state of health (SOH)**, **remaining useful life (RUL)** and **state of charge (SOC)** of lithium-ion cells from four or five numbers per charging step, the kind of summary a low-power IoT sensor node can send once per cycle. It compares five models (ridge regression, random forest, Tuned XGBoost, LSTM and CNN-LSTM), gives calibrated 90% conformal bands for RUL, tests tolerance to sensor faults, and includes a replay dashboard of the twin.

Every number, table and figure in the paper is produced by the single notebook `Battery_Digital_Twin.ipynb`.

## Contents of the notebook

| Part | What it does |
|---|---|
| A | NASA cells: charging features, end-of-life rule, leave-one-battery-out comparison of five models |
| B | NASA cells: self-updating twin for RUL with 90% conformal bands |
| C | LFP cells (MIT–Stanford–Toyota): download, feature extraction, cleaning |
| D | LFP cells: grouped 5-fold comparison of five models |
| E | LFP cells: self-updating twin and confidence bands |
| F | NASA cells: SOC estimation with SOH-corrected charge counting |
| G | Monitoring dashboard (replay of the digital twin) and its pictures |
| H | Final tables of the paper |
| I | Model cost, sensor-fault study, statistical tests, input importance, fault-tolerance remedies |
| J | Paper figures |
| K | Additional analyses: data size per charge, software and hardware, effect sizes, unseen charging policies |
| L | NASA cells: single fixed split (LSTM vs CNN-LSTM on B0018) and charge and energy balance checks |

## Data

1. **NASA Battery Data Set** (cells B0005, B0006, B0007, B0018), from the NASA Prognostics Center of Excellence Data Repository. B. Saha and K. Goebel, "Battery Data Set", NASA Ames Research Center, 2007. Download the four `.mat` files yourself and place them in your Google Drive (in Colab) or in the data folder (on your own computer).
2. **MIT–Stanford–Toyota LFP data set** (124 cells). K. A. Severson, P. M. Attia et al., "Data-driven prediction of battery cycle life before capacity degradation", *Nature Energy* 4, 383–391 (2019). Available at https://data.matr.io/1/. Part C downloads the three batch files (several GB) automatically.

The data sets are not included in this repository. Please cite their original sources if you use them.

## How to run

### Google Colab (recommended)
1. Open `Battery_Digital_Twin.ipynb` in Google Colab.
2. Choose *Runtime → Change runtime type → T4 GPU*.
3. Put the four NASA `.mat` files anywhere inside your Google Drive.
4. Choose *Runtime → Run all* and allow access to Drive when asked.

Results are written to `MyDrive/FYP_NASA_Battery/stage2_results` (NASA results, paper tables and figures, dashboard) and `MyDrive/FYP_MATR` (LFP results).

### On your own computer
```bash
pip install -r requirements.txt
export DATA_ROOT=/path/to/your/data     # folder that contains the NASA .mat files (default: ./data)
jupyter notebook Battery_Digital_Twin.ipynb
```
A GPU is recommended for the LSTM and CNN-LSTM models.

### Run time and saved results
A full run from scratch takes a few hours on a Colab T4 GPU; most of the time goes to the deep models and the hyperparameter searches. Each slow step saves its result in the cache folder `FYP_cache` and loads it on the next run. Set `RERUN_ALL = True` in the setup cells (Parts A and C) to ignore the saved results and recompute everything.

### Optional steps
- **Part G2** installs Playwright and a headless browser to save pictures of the dashboard.
- **Part K2** downloads one LFP batch file (2–3 GB) to count raw samples per charge. Set `COUNT_LFP = False` to skip it.

## Software used for the paper
Python 3.13, scikit-learn 1.6.1, XGBoost 3.4.1, TensorFlow 2.21 (Google Colab, T4 GPU). See `requirements.txt`.

## Outputs
- `paper_tables/`: every table of the paper as CSV and LaTeX.
- `paper_figures/`: every figure of the paper as PDF and PNG.
- `twin_dashboard.html`: the monitoring dashboard. It is a single self-contained web page that opens in any browser without internet or Python.
