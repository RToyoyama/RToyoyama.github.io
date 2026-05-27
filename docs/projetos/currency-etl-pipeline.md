# Currency ETL Pipeline

[![CI](https://github.com/RToyoyama/Currency-ETL-Pipeline/actions/workflows/ci.yml/badge.svg)](https://github.com/RToyoyama/Currency-ETL-Pipeline/actions/workflows/ci.yml)
![Python](https://img.shields.io/badge/Python-3.13-blue?logo=python&logoColor=white)
![Airflow](https://img.shields.io/badge/Apache%20Airflow-2.9-017CEE?logo=apacheairflow&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-336791?logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)

[:fontawesome-brands-github: Repositório](https://github.com/RToyoyama/Currency-ETL-Pipeline){ .md-button .md-button--primary }

---

## Visão Geral

Pipeline ETL automatizado que coleta cotações de moedas em tempo real,
transforma os dados e os persiste em PostgreSQL — orquestrado pelo Apache
Airflow e containerizado com Docker.

!!! success "Destaques"
    - Pipeline completo Extract → Transform → Load em produção
    - Execução horária automática com retries e monitoramento via Airflow UI
    - CI/CD com GitHub Actions: lint, formatação e testes a cada push
    - Testes sem dependências externas (API mockada, SQLite em memória)

---

## Arquitetura

```
API (AwesomeAPI)
      │
      ▼
┌─────────────┐     ┌─────────────────┐     ┌──────────────┐
│   Extract   │────▶│    Transform    │────▶│     Load     │
│  Requests   │     │  Pandas         │     │  SQLAlchemy  │
│  Validação  │     │  Tipagem        │     │  PostgreSQL  │
└─────────────┘     └─────────────────┘     └──────────────┘
                            │
                            ▼
              ┌─────────────────────────┐
              │     Apache Airflow      │
              │  Orquestração @hourly   │
              │  Retries automáticos    │
              └─────────────────────────┘
                            │
                            ▼
              ┌─────────────────────────┐
              │      Docker Compose     │
              │  postgres               │
              │  airflow-webserver      │
              │  airflow-scheduler      │
              └─────────────────────────┘
```

---

## Stack Utilizada

| Tecnologia | Versão | Função |
|---|---|---|
| Python | 3.13 | Linguagem principal |
| uv | latest | Gerenciamento de dependências |
| Apache Airflow | 2.9.2 | Orquestração do pipeline |
| PostgreSQL | 15 | Armazenamento dos dados |
| Pandas | 2.2+ | Transformação dos dados |
| SQLAlchemy | 2.0+ | Abstração do banco de dados |
| Docker / Compose | latest | Containerização |
| Pytest | 8.0+ | Testes automatizados |
| Ruff | 0.4+ | Lint e formatação |
| GitHub Actions | — | CI/CD |

---

## Implementação

### DAG do Airflow

```python
# dags/currency_dag.py
@dag(
    dag_id="currency_etl_pipeline",
    schedule="@hourly",
    start_date=datetime(2024, 1, 1),
    catchup=False,
    tags=["etl", "currency"],
)
def currency_etl_pipeline():
    extract   = PythonOperator(task_id="extract",   python_callable=extract_currencies)
    transform = PythonOperator(task_id="transform", python_callable=transform_data)
    load      = PythonOperator(task_id="load",      python_callable=load_to_postgres)

    extract >> transform >> load
```

### Schema do Banco de Dados

```sql
CREATE TABLE currency_rates (
    id           SERIAL PRIMARY KEY,
    currency     VARCHAR(20)    NOT NULL,
    value        NUMERIC(10, 2) NOT NULL,
    collected_at TIMESTAMP      NOT NULL
);
```

---

## Dados Coletados

| Par | Descrição | Frequência |
|-----|-----------|-----------|
| USD-BRL | Dólar Americano → Real | A cada hora |
| EUR-BRL | Euro → Real | A cada hora |
| BTC-BRL | Bitcoin → Real | A cada hora |

Fonte: [AwesomeAPI](https://economia.awesomeapi.com.br/)

---

## Resultados

!!! success "CI/CD funcionando"
    Pipeline de CI executa automaticamente a cada push:

    1. **Lint** — `ruff check .`
    2. **Formatação** — `ruff format --check .`
    3. **Testes** — `pytest -v`

---

## Evoluções Futuras

- [ ] Camada raw — preservar dados brutos em Parquet
- [ ] Idempotência — evitar inserção de duplicatas
- [ ] Great Expectations — validação de qualidade dos dados
- [ ] MinIO — Data Lake local para arquivos intermediários
- [ ] Dashboard — visualização das cotações históricas
- [ ] Alertas — notificações por e-mail em caso de falha