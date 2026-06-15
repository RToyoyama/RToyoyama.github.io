# Dashboard de E-commerce

![Python](https://img.shields.io/badge/Python-3.13-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?logo=sqlite&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![Chart.js](https://img.shields.io/badge/Chart.js-4.4.0-FF6384?logo=chartdotjs&logoColor=white)

---

## Visão Geral

Projeto final do módulo de Aprofundamento de Analytics (Profissão Ciência de Dados).
Consolida dados de transações e clientes de um e-commerce via SQL e exibe os
resultados em um dashboard HTML interativo com filtros dinâmicos.

!!! success "Destaques"
    - Pipeline SQL completo: leitura, normalização, INNER JOIN e exportação
    - Dashboard HTML autocontido — 296 registros embutidos, sem dependência de servidor
    - 7 métricas e visualizações interativas com filtros por gênero, estado e categoria
    - Dados brutos normalizados (formato europeu `72,93` → decimal `72.93`)

---

## Estrutura do Projeto

```
ecommerce-dashboard/
├── data/
│   ├── raw/                 # Dados de origem (transações e clientes)
│   └── processed/           # Dataset consolidado (resultado do JOIN)
├── notebooks/
│   └── Profissao Cientista de Dados M26 Projeto.ipynb
├── src/
│   └── dashboard.html       # Dashboard interativo (Chart.js)
├── requirements.txt
└── README.md
```

---

## Stack Utilizada

| Tecnologia | Versão | Função |
|---|---|---|
| Python | 3.13 | Linguagem principal |
| pandas | latest | Manipulação e exportação dos dados |
| SQLite (`sqlite3`) | built-in | Banco de dados em memória para o JOIN |
| Jupyter Notebook | latest | Pipeline de dados interativo |
| Chart.js | 4.4.0 | Visualizações no dashboard HTML |

---

## Pipeline de Dados

```
TB_TRANSACOES_PROJETO_ECOMM.csv   TB_CLIENTES_PROJETO_ECOMM.csv
              │                                │
              ▼                                ▼
     ┌────────────────┐             ┌────────────────────┐
     │  Leitura raw   │             │   Leitura raw      │
     │  Normalização  │             │                    │
     │  Price: ,→.    │             │                    │
     └────────┬───────┘             └─────────┬──────────┘
              │                               │
              ▼                               ▼
     ┌─────────────────────────────────────────────────┐
     │              SQLite em memória                  │
     │  INNER JOIN via id_client                       │
     └─────────────────────────┬───────────────────────┘
                               │
                               ▼
              dados_ecommerce_final.csv (296 registros)
                               │
                               ▼
                      dashboard.html (autocontido)
```

### Justificativa do JOIN

**INNER JOIN** foi escolhido porque o objetivo é analisar transações vinculadas
a clientes identificados. Transações sem correspondência na tabela de clientes
(registros órfãos) não agregam valor às métricas demográficas e geográficas —
resultando em 296 registros consolidados.

---

## Dashboard

O arquivo `src/dashboard.html` é autocontido: os 296 registros estão embutidos
diretamente no HTML, sem necessidade de servidor ou banco de dados em execução.

**Métricas exibidas:**

| Métrica | Tipo |
|---|---|
| Total de vendas e ticket médio | KPI cards |
| Número de transações e clientes únicos | KPI cards |
| Distribuição geográfica por estado | Gráfico de barras |
| Perfil demográfico por gênero | Gráfico de rosca |
| Receita por categoria de produto | Barras horizontais |
| Distribuição de preços | Histograma (10 faixas) |
| Top cargos por número de transações | Barras horizontais |

**Filtros interativos:** gênero, estado e categoria — todos os KPIs e gráficos
atualizam ao selecionar um filtro.

---

## Como Executar

### Pipeline de dados (opcional)

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Abra o notebook e execute todas as células. O arquivo
`data/processed/dados_ecommerce_final.csv` será gerado.

### Dashboard

**Opção A — servidor local (recomendado):**

```bash
python3 -m http.server 8000
```

Acesse `http://localhost:8000/src/dashboard.html`.

**Opção B — abrir diretamente no navegador:**

Abra `src/dashboard.html` com duplo clique ou `File > Open`.
