# Credit Score com Naive Bayes

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-F7931E?logo=scikitlearn&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?logo=numpy&logoColor=white)

[:fontawesome-brands-github: Repositório](https://github.com/RToyoyama/CreditScoreNaiveBayes){ .md-button .md-button--primary }

---

## Visão Geral

Modelo preditivo de **classificação de risco de crédito** usando Gaussian Naive Bayes,
com 98% de acurácia no teste e excelente capacidade de generalização.

!!! success "Resultados"
    | Métrica | Valor |
    |---------|-------|
    | Acurácia — Treino | ~98,7% |
    | Acurácia — Teste | ~98,0% |
    | Recall Macro — Teste | ~99,0% |

    Métricas próximas entre treino e teste confirmam ausência de overfitting.

---

## Contexto

Continuação do Módulo 17 do curso Profissão: Cientista de Dados (EBAC), onde os
dados foram pré-processados: classes balanceadas e variáveis categóricas transformadas.
Este módulo foca na modelagem e avaliação.

---

## Dados

| Arquivo | Descrição |
|---------|-----------|
| `X_train_balanceado.csv` | Variáveis preditoras — treino balanceado |
| `y_train_balanceado.csv` | Variável alvo — treino |
| `X_test.csv` | Variáveis preditoras — teste |
| `y_test.csv` | Variável alvo — teste |

---

## Implementação

```python
from sklearn.naive_bayes import GaussianNB
from sklearn.metrics import accuracy_score, recall_score, classification_report

# Treinamento
gnb = GaussianNB()
gnb.fit(X_train, y_train)

# Avaliação
y_pred = gnb.predict(X_test)

print(f"Acurácia treino : {accuracy_score(y_train, gnb.predict(X_train)):.2%}")
print(f"Acurácia teste  : {accuracy_score(y_test, y_pred):.2%}")
print(f"Recall macro    : {recall_score(y_test, y_pred, average='macro'):.2%}")

print(classification_report(y_test, y_pred))
```

---

## Por que Gaussian Naive Bayes?

!!! tip "Escolha do modelo"
    - Eficiente com dados numéricos contínuos
    - Treinamento rápido mesmo em grandes volumes
    - Assume independência condicional entre features
    - Excelente baseline para comparação com modelos mais complexos

---

## Stack Utilizada

- **Python 3.x** · **Pandas** · **NumPy**
- **Scikit-learn** — `GaussianNB`, métricas de avaliação
- **Seaborn / Matplotlib** — matriz de confusão e visualizações
- **Jupyter Notebook** — ambiente de experimentação

---

## Evoluções Futuras

- [ ] Comparativo com Bernoulli e Multinomial Naive Bayes
- [ ] Pipeline com `sklearn.pipeline.Pipeline`
- [ ] Deploy com Streamlit para demonstração interativa