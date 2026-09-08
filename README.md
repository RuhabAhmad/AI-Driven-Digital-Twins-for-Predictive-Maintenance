# Resource-Aware Deployment of AI-Driven Digital Twins for Predictive Maintenance

**A Cloud–Edge Cost–Accuracy–Latency Study**

Group 20 · Ayan Asif (22i-1097) · Ruhab Ahmad (23i-0559)
FAST University (NUCES), Islamabad
Supervised by: Basharat Hussain (basharat.hussain.v@isb.nu.edu.pk)

---

## Overview

This repository contains the code, experiments, and reproducibility materials for our 100-day research project on **resource-aware digital twin deployment for predictive maintenance (PdM)**.

We study how the deployment configuration of an AI-driven digital twin — **cloud-only**, **edge-only**, or **hybrid edge-cloud** — affects three things at once:

- **Prediction accuracy** (RMSE / MAE on remaining useful life)
- **Response latency** (end-to-end, sensor reading → RUL output)
- **Operational cost** (compute + transmission per inference cycle)

using the same underlying model and dataset, so the deployment configuration is the only thing that varies.

**Primary Research Question:**
> How does the deployment configuration (cloud-only, edge-only, or hybrid) of an AI-driven digital twin for predictive maintenance affect prediction accuracy, response latency, and operational cost, compared to a static cloud-only baseline?

---

## Repository Structure

```
.
├── data/
│   ├── raw/                  # Original C-MAPSS files (not committed — see Dataset section)
│   └── processed/            # Cleaned/windowed data used for training
├── src/
│   ├── data_prep.py          # Loading, cleaning, and windowing the C-MAPSS data
│   ├── models/
│   │   ├── lstm_rul.py       # LSTM/GRU RUL prediction model
│   │   └── baseline_rf.py    # Random Forest / linear regression baseline
│   ├── train.py               # Model training entry point
│   ├── evaluate.py            # RMSE / MAE computation on test splits
│   └── deployment_sim/
│       ├── cloud_only.py     # Cloud-only deployment simulation
│       ├── edge_only.py      # Edge-only deployment simulation
│       └── hybrid.py         # Hybrid edge-cloud deployment simulation
├── experiments/
│   ├── configs/               # YAML/JSON configs for each experiment run
│   └── run_all.sh             # Runs all three deployment configurations end-to-end
├── figures/                    # Generated plots (accuracy/latency/cost trade-off charts)
├── results/
│   ├── raw/                   # Raw metric logs per run
│   └── summary.csv            # Aggregated accuracy/latency/cost table
├── notebooks/                  # Exploratory analysis (optional, not required to reproduce results)
├── requirements.txt
├── environment.yml             # Conda environment (alternative to requirements.txt)
├── LICENSE
└── README.md
```

*(Structure will be filled in incrementally as the project progresses through Cutoffs 2 and 3 — see [Project Timeline](#project-timeline) below.)*

---

## Dataset

We use the **NASA C-MAPSS Turbofan Engine Degradation Simulation Dataset**, a public benchmark for remaining-useful-life (RUL) prediction.

- **Source:** NASA Prognostics Data Repository
- **Citation:** A. Saxena and K. Goebel, "Turbofan Engine Degradation Simulation Data Set," NASA Ames Research Center, Moffett Field, CA, 2008.
- **Download:** [NASA Prognostics Center of Excellence Data Repository](https://www.nasa.gov/intelligent-systems-division/discovery-and-systems-health/pcoe/pcoe-data-set-repository/)
- **Subsets used:** FD001–FD004 (vary by number of operating conditions and fault modes)

The raw dataset is **not committed to this repository** due to size and licensing. To reproduce:

1. Download the C-MAPSS dataset from the link above.
2. Place the extracted `train_FD00X.txt`, `test_FD00X.txt`, and `RUL_FD00X.txt` files into `data/raw/`.
3. Run `src/data_prep.py` to generate the processed, windowed dataset used for training.

```bash
python src/data_prep.py --input_dir data/raw --output_dir data/processed
```

---

## Environment & Dependencies

**Python version:** 3.10+

**Option A — pip:**
```bash
pip install -r requirements.txt
```

**Option B — conda:**
```bash
conda env create -f environment.yml
conda activate dt-pdm
```

Key dependencies (finalized list will be pinned in `requirements.txt` as implementation progresses):
- `torch` or `tensorflow` — model training
- `scikit-learn` — baseline model (Random Forest / linear regression)
- `numpy`, `pandas` — data handling
- `matplotlib` / `seaborn` — figure generation
- `iFogSim` / `EdgeCloudSim` (Java-based) or a custom Python discrete-event simulator — deployment simulation

---

## Reproducing the Results

1. **Prepare data** (see [Dataset](#dataset) above).
2. **Train models:**
   ```bash
   python src/train.py --model lstm --dataset FD001
   python src/train.py --model baseline_rf --dataset FD001
   ```
3. **Run deployment simulations** for all three configurations:
   ```bash
   bash experiments/run_all.sh
   ```
   This runs the trained model under cloud-only, edge-only, and hybrid deployment scenarios and logs accuracy, latency, and cost metrics to `results/raw/`.
4. **Generate figures and summary table:**
   ```bash
   python src/evaluate.py --results_dir results/raw --output results/summary.csv
   python src/plot_results.py --input results/summary.csv --output_dir figures/
   ```

Random seeds are fixed in `experiments/configs/` for reproducibility; exact values will be documented there once experiments begin (Cutoff 3).

---

## Expected Metrics

| Metric | Description |
|---|---|
| **RMSE / MAE** | Accuracy of predicted vs. true remaining useful life (RUL) |
| **Latency (ms)** | Simulated end-to-end time from sensor reading to RUL output |
| **Cost (est. $/inference)** | Estimated compute + transmission cost per inference cycle |

---

## Project Timeline

| Milestone | Target Day | Status |
|---|---|---|
| Cutoff 1 — Proposal | Day 10 | ✅ Complete |
| Cutoff 2 — Literature synthesis & finalized gap | Day 35 | ⬜ In progress |
| Cutoff 3 — Implementation & preliminary results | Day 65 | ⬜ Pending |
| Draft manuscript | Day 80 | ⬜ Pending |
| Final submission | Day 100 | ⬜ Pending |

---

## Citation

If you use this code or refer to this work, please cite:

```
A. Asif and R. Ahmad, "Resource-Aware Deployment of AI-Driven Digital Twins
for Predictive Maintenance: A Cloud-Edge Cost-Accuracy-Latency Study,"
FAST University (NUCES), 2026.
```

---

## License

This project is released under the [MIT License](LICENSE).

---

## Security & Privacy Notice

No credentials, API keys, cloud tokens, or private data are stored in this repository. All datasets used are publicly available benchmarks. Any cloud-cost estimates are based on public pricing pages, not live billing accounts.

---

## Contact

- Ayan Asif — i221097@nu.edu.pk
- Ruhab Ahmad — i230559@isb.nu.edu.pk
- Supervisor: Basharat Hussain — basharat.hussain.v@isb.nu.edu.pk
