# HiveIQ: Smart Honey Hive Tracking & Production Analytics

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/alitrose/HiveIQ/blob/main/HiveIQ_Course_Project_Colab.ipynb)
[![GitHub Pages](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-success?style=flat&logo=github)](https://alitrose.github.io/HiveIQ/)
[![Course](https://img.shields.io/badge/Course-23CS2024%20Data%20Science%20Ecosystem-amber)](https://github.com)
[![Status](https://img.shields.io/badge/Status-Complete%20%26%20Tested-brightgreen)](https://github.com)

> **HiveIQ** is an end-to-end data science application combining global macro honey production analytics from the United Nations FAOSTAT dataset (9,601 records, 1961–2024) with local hive health tracking, sensor screening heuristics, and browser-executed machine learning models.

---

## 🍯 Live Application & Quick Links

- **Live Web Application (GitHub Pages):** [https://alitrose.github.io/HiveIQ/](https://alitrose.github.io/HiveIQ/)  
  *(Replace `alitrose` with your GitHub handle)*
- **Interactive Google Colab Notebook:** [Open in Google Colab](https://colab.research.google.com/github/alitrose/HiveIQ/blob/main/HiveIQ_Course_Project_Colab.ipynb)
- **Local Standalone Run:** Download this repository and double-click `index.html` (zero dependencies, works 100% offline).

---

## 📌 Problem Statement & Objectives

Beekeepers and commercial honey producers face unpredictable yield variations caused by climate anomalies, seasonal nectar flow shifts, and hive diseases. Existing tools are either purely administrative hive logs or disconnected academic research papers.

**HiveIQ bridges this gap** by:
1. Providing a clean, offline-first apiary management tracker for hive inspections, sensor observations, and harvested yields.
2. Training predictive machine learning models in Google Colab on 64 years of historical agricultural records.
3. Exporting trained parameters to a client-side JavaScript engine to deliver real-time yield forecasts, growth classifications, and peer country comparisons without requiring cloud servers.

---

## 🏗️ System Architecture & Workflow

```text
[ UN FAOSTAT Raw Dataset ] (9,601 rows, 1961-2024)
          │
          ▼
[ Exploratory Data Analysis & Cleaning ]
  ├── Filter 23 Regional Aggregates (World, Europe, etc.)
  └── Extract 157 Sovereign Nation Records (8,129 rows)
          │
          ▼
[ Feature Engineering ]
  ├── 1-Year Lag (prod_lag1)
  ├── 2-Year Lag (prod_lag2)
  ├── 3-Year Lag (prod_lag3)
  ├── 3-Year Rolling Mean (prod_roll_mean3)
  ├── 1-Year Trend Momentum (prod_diff_1)
  └── Classification Target: Growth vs Decline (growth_target)
          │
          ▼
[ Time-Aware Chronological Train/Test Split ]
  ├── Training Set (1964 - 2014): 6,263 samples (81.8%)
  └── Testing Set  (2015 - 2024): 1,395 samples (18.2%)
          │
          ▼
[ Model Training & Benchmarking in Google Colab ]
  ├── Task 1: Linear Regression vs. KNN Regressor (Continuous Volume)
  ├── Task 2: Logistic Regression vs. KNN Classifier (Growth Classification)
  └── Task 3: K-Means Clustering (k=3 Country Segmentation)
          │
          ▼
[ Parameter Export (model.json) ]
  └── Scaling vectors, weights, intercepts, cluster centroids & 156 country prototypes
          │
          ▼
[ Standalone Web Application (index.html) ]
  ├── Dashboard with Interactive Canvas Trend Charts
  ├── Hive Tracker (Local Logging + Sensor Screening Alerts)
  ├── ML Predictor (Pure JavaScript matrix multiplication)
  ├── Global Country Profile Explorer (3 K-Means Tiers)
  └── My Records (Persistent localStorage History + Ground Truth Evaluation)
```

---

## 📊 Model Evaluation & Genuine Benchmark Results

All models were evaluated on the held-out test set (years 2015 to 2024) using a chronological split to eliminate future data leakage.

### 1. Continuous Production Forecast (Regression Task)
| Evaluation Metric | Linear Regression *(Selected)* | KNN Regressor ($k=5$) | Best Model |
| :--- | :---: | :---: | :---: |
| **Mean Absolute Error (MAE)** | **2,350.33 tonnes** | 2,896.79 tonnes | **Linear Regression** |
| **Root Mean Squared Error (RMSE)** | **9,296.05 tonnes** | 12,460.96 tonnes | **Linear Regression** |
| **Coefficient of Determination ($R^2$)** | **0.9801** | 0.9643 | **Linear Regression** |

### 2. Growth vs. Decline Classification Task
| Evaluation Metric | Logistic Regression *(Selected)* | KNN Classifier ($k=5$) | Best Model |
| :--- | :---: | :---: | :---: |
| **Accuracy** | **61.22%** | 56.99% | **Logistic Regression** |
| **Precision (Growth Class)** | 60.77% | **63.75%** | Mixed |
| **Recall (Growth Sensitivity)** | **98.92%** | 64.83% | **Logistic Regression** |
| **F1-Score** | **0.7529** | 0.6429 | **Logistic Regression** |

### 3. Country Profile Segmentation (K-Means Clustering)
- **k=2:** Silhouette $= 0.8971$ *(Too coarse; only isolates China)*
- **k=3:** Silhouette $= \mathbf{0.8651}$ *(**Selected:** Optimal interpretability for apiculture)*
  - **Tier 1 (Global Mega-Producers):** China ($>400,000$ tonnes/yr).
  - **Tier 2 (Commercial / Regional Majors):** India, Turkey, USA, Argentina, Mexico, Ukraine ($20,000$–$100,000$ tonnes/yr).
  - **Tier 3 (Small / Artisanal / Emerging):** $130+$ nations ($<15,000$ tonnes/yr).
- **k=4:** Silhouette $= 0.8243$
- **k=5:** Silhouette $= 0.8247$

---

## 🧪 Systematic Verification Suite

Every mathematical operation in the JavaScript frontend was systematically cross-verified against Python scikit-learn outputs:

```text
[PASS] Case 1: India Preset         -> JS Forecast: 75569.8 t (diff: 0.022 t)
[PASS] Case 2: Turkey Preset        -> JS Forecast: 106907.6 t (diff: 0.015 t)
[PASS] Case 3: USA Preset           -> JS Forecast: 71428.9 t (diff: 0.013 t)
[PASS] Case 4: Declining Producer   -> JS Forecast: 23314.5 t (diff: 0.010 t)
[PASS] Case 5: Small Artisanal      -> JS Forecast: 436.1 t (diff: 0.038 t)
[PASS] Case 6: China Mega-Producer  -> JS Forecast: 462063.7 t (diff: 0.014 t)

VERIFICATION COMPLETE: 6 PASSED, 0 FAILED.
```

---

## 🚀 How to Run Locally

No Node.js, Python server, or build tools are required:

1. Clone or download this repository:
   ```bash
   git clone https://github.com/alitrose/HiveIQ.git
   cd HiveIQ
   ```
2. Double-click `index.html` to open it in any modern browser (Chrome, Edge, Firefox, Brave, Safari).
3. Everything runs locally on your computer. Inspection records and prediction histories are stored securely in browser `localStorage`.

---

## 🌐 How to Host on GitHub Pages (Free)

1. Create a new public repository on GitHub named `HiveIQ`.
2. Push this folder to GitHub:
   ```bash
   git init
   git add .
   git commit -m "Initial commit of HiveIQ course project"
   git branch -M main
   git remote add origin https://github.com/alitrose/HiveIQ.git
   git push -u origin main
   ```
3. On GitHub, navigate to **Settings** $\rightarrow$ **Pages** (on the left menu).
4. Under **Branch**, select `main` and folder `/ (root)`, then click **Save**.
5. Your web application will be live at `https://alitrose.github.io/HiveIQ/` in 1–2 minutes!

---

## 🐝 Beekeeper Sensor Screening Rules

HiveIQ includes automated heuristic screening rules for individual hive telemetry:
- **Optimal Brood Core Temperature:** $33.0^\circ\text{C}$ to $36.0^\circ\text{C}$.
  - Alert below $32.0^\circ\text{C}$: Risk of brood hypothermic chilling.
  - Alert above $36.5^\circ\text{C}$: Risk of comb sagging and thermal stress.
- **Optimal Relative Humidity:** $50\%$ to $65\%$.
  - Alert above $68\%$: Risk of chalkbrood fungal spore propagation.
- **Pest Observation:**
  - Varroa destructor observation triggers non-chemical monitoring or seasonal oxalic acid screening protocol.

---

## ⚠️ Academic Disclaimer & Limitations

1. **Screening & Advisory Only:** HiveIQ is an academic decision-support tool developed for the Data Science Ecosystem (23CS2024) course. It is not an agronomic diagnosis tool or financial guarantee.
2. **Autoregression Bounds:** High regression $R^2$ ($0.9801$) reflects strong historical momentum in national honey harvests. The model cannot forecast sudden localized weather anomalies, droughts, or colony collapse disorder without real-time microclimate sensors.
3. **Simulated Hive Logs:** The 10 starter hive inspection logs are synthetic demo records used to demonstrate client-side persistence and rule evaluation.

---

## 👨‍💻 Student Author & Acknowledgments

- **Course:** Data Science Ecosystem (23CS2024)
- **Primary Data Source:** Food and Agriculture Organization of the United Nations (FAOSTAT)
- **Built With:** Python, pandas, NumPy, scikit-learn, Matplotlib, Vanilla HTML5, CSS3, JavaScript.
