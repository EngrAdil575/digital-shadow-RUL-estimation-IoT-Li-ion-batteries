# AI-Driven Digital Shadow for Health Monitoring and Remaining Useful Life Estimation of IoT-Connected Li-Ion Batteries

Code, output files, and the replay dashboard for the manuscript:

> **AI-Driven Digital Shadow for Health Monitoring & Remaining Useful Life Estimation of IoT-Connected Li-Ion Batteries**
> Muhammad Waiz Wais, Roya Mangal, Muhammad Sadiq Orakzai, Adil Zohaib, Haris Sheh Zad, Sohail Khalid.

**Status:** This repository accompanies a manuscript submitted to ACM Transactions on Internet of Things (ACM TIoT). The manuscript is not yet published. This repository is the supporting material for the submitted version.

This repository is the reproducibility record for the manuscript. It holds the full analysis notebook, every result file and figure, and the self-contained monitoring dashboard. The two source datasets are public and are linked (not redistributed); see `data/DATASET_README.txt`.

## What the paper does

The work evaluates a charging-based digital-shadow architecture for IoT battery monitoring that uses four or five features per charging cycle, the kind of summary a low-power sensor node can send once per cycle. From those features it estimates the state of health (SOH), remaining useful life (RUL), and state of charge (SOC) of lithium-ion cells. Five models (ridge regression, random forest, tuned XGBoost, LSTM, and a CNN-LSTM) are compared on four NASA cells and 124 lithium iron phosphate (LFP) cells, with each test cell excluded from fitting and tuning. RUL prediction intervals are calibrated with conformal bands, tolerance to sensor faults is tested with feature noise and glitches, and a replay dashboard presents the estimates. The evaluation uses retrospective preprocessing and does not demonstrate a hardware or closed-loop deployment.

## Repository layout

```
digital-shadow-RUL-estimation-IoT-Li-ion-batteries/
├── data/
│   └── DATASET_README.txt     How to obtain the two public datasets (NASA and MIT–Stanford–Toyota LFP)
├── code/
│   ├── Battery_Digital_Twin.ipynb   Single notebook (Parts A–L) that produces every number, table, and figure
│   ├── requirements.txt
│   └── CODE_README.md               Part-by-part description and run instructions
├── digital_twin/
│   ├── twin_dashboard.html          Self-contained replay dashboard (open in any browser; no internet or Python)
│   └── DASHBOARD_README.txt
├── results/
│   ├── dashboard_figures/           Rendered dashboard views (NASA and LFP)
│   └── other_results/
│       ├── NASA/                    Cycle dataset, LOBO SOH/RUL, SOC, twin streams, balance checks
│       └── LFP/                     Cell/cycle tables, model comparison, robustness, twin examples
├── CITATION.cff
└── LICENSE                          MIT (code). The datasets are public and linked, not redistributed.
```

## Datasets

The two datasets are public and are **not** redistributed here. Obtain them from their original sources:

1. **NASA Battery Data Set** (cells B0005, B0006, B0007, B0018) — B. Saha and K. Goebel, "Battery Data Set," NASA Ames Prognostics Center of Excellence (PCoE) Data Repository, NASA Ames Research Center, 2007.
2. **MIT–Stanford–Toyota LFP dataset** (124 cells) — K. A. Severson, P. M. Attia, et al., "Data-driven prediction of battery cycle life before capacity degradation," *Nature Energy* 4, 383–391 (2019). Available at https://data.matr.io/1/.

See `data/DATASET_README.txt` for placement and the automatic download used by the notebook.

## Reproducing the results

Open `code/Battery_Digital_Twin.ipynb` in Google Colab (Runtime → Change runtime type → T4 GPU), place the four NASA `.mat` files in your Google Drive, and run all cells. Part C downloads the LFP batch files automatically. Each slow step caches its result and is reloaded on the next run; set `RERUN_ALL = True` to recompute everything. The notebook writes the paper tables, figures, and the dashboard. See `code/CODE_README.md` for the full part list and for running on your own computer.

Dependencies and exact versions are in `code/requirements.txt` (Python 3.13, scikit-learn 1.6.1, XGBoost 3.4.1, TensorFlow 2.21).

## Monitoring dashboard

`digital_twin/twin_dashboard.html` is a single self-contained web page — open it in any browser, no internet or Python needed. It replays the recorded tests cycle by cycle and shows, for each cell, the estimated SOH, the RUL with its calibrated 90% band, the SOC, alerts, and the latest readings, in two views (the four NASA cells and a selection of LFP cells). Use the play button or the slider to move through cycles, and click a cell to see its charts.

## Citation

If you use this code, please cite the manuscript (see `CITATION.cff`) and the two datasets above (Saha and Goebel, 2007; Severson, Attia, et al., 2019).

## License

The code in this repository is released under the MIT License (see `LICENSE`). The datasets are public and are linked to their original sources, not redistributed here.
