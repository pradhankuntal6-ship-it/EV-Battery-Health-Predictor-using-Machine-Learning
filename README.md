# 🔋 EV Battery Health Predictor using Machine Learning

A machine learning project that predicts Li-ion battery health status — **Healthy / Degrading / Critical** — using NASA's public Battery Dataset. Built as part of Module 1 of the project series.

---

## 📌 Objective

Train a classification model to predict battery health based on real charge/discharge/impedance test data, using capacity fade as the ground-truth indicator of degradation.

---

## 📂 Dataset

**Source:** [NASA Prognostics Center of Excellence — Battery Data Set](https://www.nasa.gov/intelligent-systems-division/discovery-and-systems-health/pcoe/pcoe-data-set-repository/)

- 34 Li-ion batteries (B0005–B0056), each run through repeated charge, discharge, and electrochemical impedance spectroscopy (EIS) cycles at room temperature until end-of-life (30% capacity fade from a 2 Ah rating).
- **`metadata.csv`** — one row per test (charge/discharge/impedance) with battery ID, cycle order, measured `Capacity`, `Re` (electrolyte resistance), and `Rct` (charge transfer resistance).
- **`data/`** — ~7,500 individual CSV files, one per test, containing the raw time-series (`Voltage_measured`, `Current_measured`, `Temperature_measured`, `Time`, etc.) for that specific cycle.
- **`extra_infos/`** — README files (per battery group) describing experimental setup and data field definitions.

> ⚠️ The dataset is not included in this repo due to size (~575 MB unzipped). Download it from the link above or from the original archive, and place it locally as described below.

---

## 🗂️ Repository Structure

```
├── README.md
├── notebook/
│   └── battery_health_predictor.ipynb   # Full pipeline (Colab-ready)
├── data/                                  # (not included — see Dataset section)
│   ├── metadata.csv
│   ├── data/            *.csv per cycle
│   └── extra_infos/     README_*.txt
└── outputs/
    ├── cleaned_battery_dataset.csv
    ├── dataset_statistics.csv
    ├── confusion_matrix.png
    ├── chart1_voltage_vs_cycle.png
    ├── chart2_health_distribution.png
    ├── chart3_capacity_fade.png
    └── feature_importance.png
```

---

## ⚙️ Tools & Libraries

- **Language:** Python 3
- **Libraries:** Pandas, NumPy, Scikit-learn, Matplotlib, tqdm
- **Environment:** Google Colab (no local setup required)

---

## 🚀 How to Run

1. Open the notebook in [Google Colab](https://colab.research.google.com/).
2. Zip the dataset folder (containing `metadata.csv`, `data/`, `extra_infos/`) if not already zipped.
3. Run the notebook cells top to bottom:
   - Upload the dataset zip when prompted.
   - The notebook extracts per-cycle voltage/temperature features from `data/`, merges impedance readings from `metadata.csv`, and labels each cycle's health status from capacity retention.
   - Trains a `RandomForestClassifier` and evaluates it.
4. Outputs (cleaned CSV, stats, charts, confusion matrix) are saved and downloadable from the final cell.

**Estimated runtime:** ~2–4 minutes (feature extraction from ~2,800 files is the slowest step).

---

## 🧠 Methodology

| Step | Description |
|---|---|
| **Feature extraction** | Per discharge cycle: mean/min voltage, mean/max temperature, discharge duration, plus nearest impedance readings (Re, Rct) |
| **Labeling** | Capacity ratio (current capacity ÷ battery's initial capacity): ≥90% → Healthy, 70–90% → Degrading, <70% → Critical (matches NASA's own 30% fade end-of-life criterion) |
| **Model** | Random Forest Classifier (200 trees, max depth 8) |
| **Split** | 80/20 train-test, stratified by class |
| **Evaluation** | Accuracy, classification report, confusion matrix |

**Limitation:** the train/test split is row-level, so cycles from the same battery can appear in both sets. A stricter evaluation would split by `battery_id` — noted here as a possible future improvement.

---

## 📊 Deliverables

1. ✅ Cleaned dataset (`cleaned_battery_dataset.csv`) with summary statistics
2. ✅ Trained classification model (Healthy / Degrading / Critical)
3. ✅ Accuracy score + confusion matrix
4. ✅ Charts: Voltage vs Cycle, Health Status Distribution, Capacity Fade over Cycles
5. ✅ Short report (3–4 pages)
6. ✅ Presentation slides (5–6 slides)

---

## 📈 Results

*(Fill in after running the notebook)*

- **Accuracy:** `__%`
- **Key insight:** *(e.g., which feature was most predictive — cycle number, voltage, or resistance)*

---

## 📄 License

This project uses the NASA Battery Dataset, which is publicly available for research use. Code in this repository is provided for educational purposes.

---

## 🙋 Author

*(Add your name / course / module details here)*
