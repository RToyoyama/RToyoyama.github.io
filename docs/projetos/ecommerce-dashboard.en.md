# E-commerce Dashboard

![Python](https://img.shields.io/badge/Python-3.13-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?logo=sqlite&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![Chart.js](https://img.shields.io/badge/Chart.js-4.4.0-FF6384?logo=chartdotjs&logoColor=white)

---

## Overview

Final project for the Analytics Deep Dive module (Profissão Ciência de Dados).
Consolidates e-commerce transaction and customer data via SQL and displays the
results in an interactive HTML dashboard with dynamic filters.

!!! success "Highlights"
    - Complete SQL pipeline: ingestion, normalization, INNER JOIN and export
    - Self-contained HTML dashboard — 296 records embedded, no server required
    - 7 metrics and interactive visualizations with filters by gender, state and category
    - Raw data normalized (European format `72,93` → decimal `72.93`)

---

## Project Structure

```
ecommerce-dashboard/
├── data/
│   ├── raw/                 # Source data (transactions and customers)
│   └── processed/           # Consolidated dataset (JOIN result)
├── notebooks/
│   └── Profissao Cientista de Dados M26 Projeto.ipynb
├── src/
│   └── dashboard.html       # Interactive dashboard (Chart.js)
├── requirements.txt
└── README.md
```

---

## Tech Stack

| Technology | Version | Role |
|---|---|---|
| Python | 3.13 | Main language |
| pandas | latest | Data manipulation and export |
| SQLite (`sqlite3`) | built-in | In-memory database for the JOIN |
| Jupyter Notebook | latest | Interactive data pipeline |
| Chart.js | 4.4.0 | Dashboard visualizations |

---

## Data Pipeline

```
TB_TRANSACOES_PROJETO_ECOMM.csv   TB_CLIENTES_PROJETO_ECOMM.csv
              │                                │
              ▼                                ▼
     ┌────────────────┐             ┌────────────────────┐
     │  Raw ingestion │             │   Raw ingestion    │
     │  Normalization │             │                    │
     │  Price: ,→.    │             │                    │
     └────────┬───────┘             └─────────┬──────────┘
              │                               │
              ▼                               ▼
     ┌─────────────────────────────────────────────────┐
     │              In-memory SQLite                   │
     │  INNER JOIN on id_client                        │
     └─────────────────────────┬───────────────────────┘
                               │
                               ▼
              dados_ecommerce_final.csv (296 records)
                               │
                               ▼
                      dashboard.html (self-contained)
```

### JOIN Rationale

**INNER JOIN** was chosen because the goal is to analyze transactions linked
to identified customers. Transactions without a matching customer record
(orphaned rows) add no value to demographic or geographic metrics —
resulting in 296 consolidated records.

---

## Dashboard

The `src/dashboard.html` file is self-contained: all 296 records are embedded
directly in the HTML, with no server or live database connection required.

**Displayed metrics:**

| Metric | Type |
|---|---|
| Total sales and average ticket | KPI cards |
| Transactions and unique customers | KPI cards |
| Geographic distribution by state | Bar chart |
| Demographic profile by gender | Doughnut chart |
| Revenue by product category | Horizontal bars |
| Price distribution | Histogram (10 bins) |
| Top roles by transaction count | Horizontal bars |

**Interactive filters:** gender, state and category — all KPIs and charts
update when a filter is selected.

---

## How to Run

### Data pipeline (optional)

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Open the notebook and run all cells. The file
`data/processed/dados_ecommerce_final.csv` will be generated.

### Dashboard

**Option A — local server (recommended):**

```bash
python3 -m http.server 8000
```

Open `http://localhost:8000/src/dashboard.html`.

**Option B — open directly in browser:**

Double-click `src/dashboard.html` or use `File > Open`.
