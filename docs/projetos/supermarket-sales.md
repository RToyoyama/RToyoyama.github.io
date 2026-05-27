# Análise de Vendas de Supermercado

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-4C72B0?logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557c?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)

[:fontawesome-brands-github: Repositório](https://github.com/RToyoyama/supermarket-sales-data-analysis){ .md-button .md-button--primary }

---

## Visão Geral

Análise Exploratória de Dados (EDA) completa sobre vendas de supermercado,
identificando padrões de consumo, sazonalidade e gerando insights estratégicos.

!!! info "Contexto"
    Projeto do Módulo 13 — Estatística do curso Profissão: Cientista de Dados (EBAC),
    com foco em estatística descritiva e visualização profissional de dados.

---

## Perguntas Respondidas

- Quais categorias de produto têm maior volume de vendas?
- Existe sazonalidade nos padrões de compra?
- Qual o perfil dos clientes por filial?
- Quais horários concentram mais transações?

---

## Fluxo de Análise

```
Carga dos dados
      │
      ▼
Inspeção inicial
(shape, dtypes, nulos, duplicatas)
      │
      ▼
Estatísticas descritivas
(média, mediana, desvio padrão, quartis)
      │
      ▼
Análise univariada
(distribuições, histogramas, boxplots)
      │
      ▼
Análise bivariada
(correlações, vendas por categoria/filial/horário)
      │
      ▼
Insights & Conclusões
```

---

## Implementação

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("data/supermarket_sales.csv")

# Estatísticas descritivas
print(df.describe())

# Vendas por categoria
df.groupby("Product line")["Total"].sum() \
  .sort_values() \
  .plot(kind="barh", figsize=(10, 6), color="teal")

plt.title("Receita Total por Categoria de Produto")
plt.tight_layout()
plt.show()

# Correlação entre variáveis numéricas
sns.heatmap(df.select_dtypes("number").corr(), annot=True, cmap="coolwarm")
plt.show()
```

---

## Stack Utilizada

- **Python 3.x** — linguagem principal
- **Pandas** — manipulação e análise dos dados
- **Matplotlib** — gráficos e visualizações
- **Seaborn** — visualizações estatísticas
- **Jupyter Notebook** — ambiente de análise interativa

---

## Evoluções Futuras

- [ ] Dashboard interativo com Streamlit
- [ ] Análise de séries temporais de vendas
- [ ] Segmentação de clientes com K-Means