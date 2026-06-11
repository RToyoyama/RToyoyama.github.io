# Currency ETL Pipeline

[![CI](https://github.com/RToyoyama/Currency-ETL-Pipeline/actions/workflows/ci.yml/badge.svg)](https://github.com/RToyoyama/Currency-ETL-Pipeline/actions/workflows/ci.yml)
![Python](https://img.shields.io/badge/Python-3.13-blue?logo=python&logoColor=white)
![Airflow](https://img.shields.io/badge/Apache%20Airflow-2.9-017CEE?logo=apacheairflow&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-336791?logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)

[:fontawesome-brands-github: Repository](https://github.com/RToyoyama/Currency-ETL-Pipeline){ .md-button .md-button--primary }

---

## Overview

Automated ETL pipeline that collects real-time currency exchange rates,
transforms the data and persists it in PostgreSQL — orchestrated by Apache
Airflow and containerized with Docker.

!!! success "Highlights"
    - Complete Extract → Transform → Load pipeline in production
    - Hourly automatic execution with retries and monitoring via Airflow UI
    - CI/CD with GitHub Actions: lint, formatting and tests on every push
    - Tests without external dependencies (mocked API, in-memory SQLite)

---

## Architecture

```
API (AwesomeAPI)
      │
      ▼
┌─────────────┐     ┌─────────────────┐     ┌──────────────┐
│   Extract   │────▶│    Transform    │────▶│     Load     │
│  Requests   │     │  Pandas         │     │  SQLAlchemy  │
│  Validation │     │  Typing         │     │  PostgreSQL  │
└─────────────┘     └─────────────────┘     └──────────────┘
                            │
                            ▼
              ┌─────────────────────────┐
              │     Apache Airflow      │
              │  Orchestration @hourly  │
              │  Automatic retries      │
              └─────────────────────────┘
```

---

## Tech Stack

| Technology | Version | Role |
|---|---|---|
| Python | 3.13 | Main language |
| Apache Airflow | 2.9.2 | Pipeline orchestration |
| PostgreSQL | 15 | Data storage |
| Pandas | 2.2+ | Data transformation |
| SQLAlchemy | 2.0+ | Database abstraction |
| Docker / Compose | latest | Containerization |
| Pytest | 8.0+ | Automated tests |
| Ruff | 0.4+ | Lint and formatting |
| GitHub Actions | — | CI/CD |

---

## Implementation

```python
@dag(
    dag_id="currency_etl_pipeline",
    schedule="@hourly",
    start_date=datetime(2024, 1, 1),
    catchup=False,
)
def currency_etl_pipeline():
    extract   = PythonOperator(task_id="extract",   python_callable=extract_currencies)
    transform = PythonOperator(task_id="transform", python_callable=transform_data)
    load      = PythonOperator(task_id="load",      python_callable=load_to_postgres)
    extract >> transform >> load
```

---

## Future Improvements

- [ ] Raw layer — preserve raw API data in Parquet
- [ ] Idempotency — avoid duplicate insertions
- [ ] Great Expectations — data quality validation
- [ ] Dashboard — historical exchange rate visualization
