# Classificação com Árvore de Decisão

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-F7931E?logo=scikitlearn&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)

[:fontawesome-brands-github: Repositório](https://github.com/RToyoyama/modulo-21-arvore-decisao){ .md-button .md-button--primary }

---

## Visão Geral

Aplicação do algoritmo de **Árvore de Decisão** para classificação supervisionada,
com avaliação de desempenho e comparação com **Naive Bayes**.

!!! success "Resultados"
    | Métrica | Valor |
    |---------|-------|
    | Acurácia — Treino | 100% |
    | Acurácia — Teste | 94% |
    | Comparativo | Árvore superou Naive Bayes neste dataset |

---

## Fluxo do Projeto

=== "1. Preparação"
    - Leitura das bases de treino balanceado e teste
    - Separação entre atributos (X) e variável alvo (y)

=== "2. Treinamento"
    - `DecisionTreeClassifier(criterion='gini', random_state=0)`
    - Base de treino balanceada para evitar viés de classe

=== "3. Avaliação"
    - Acurácia em treino e teste
    - `classification_report` e matriz de confusão

=== "4. Feature Importance"
    - Ranking das variáveis mais relevantes
    - Novo modelo treinado apenas com as 2 principais features
    - Comparativo de desempenho

=== "5. Comparação"
    - Árvore de Decisão vs. Naive Bayes
    - Análise de adequação ao tipo de dado

---

## Implementação

```python
from sklearn.tree import DecisionTreeClassifier, plot_tree
from sklearn.metrics import accuracy_score, classification_report
import matplotlib.pyplot as plt

# Treinamento
clf = DecisionTreeClassifier(criterion='gini', random_state=0)
clf.fit(X_train, y_train)

# Avaliação
print(f"Acurácia treino : {accuracy_score(y_train, clf.predict(X_train)):.2%}")
print(f"Acurácia teste  : {accuracy_score(y_test,  clf.predict(X_test)):.2%}")

# Visualização da árvore
plt.figure(figsize=(20, 10))
plot_tree(clf, filled=True, feature_names=X_train.columns)
plt.show()

# Feature importance
importances = pd.Series(clf.feature_importances_, index=X_train.columns)
importances.sort_values().plot(kind='barh')
```

---

## Stack Utilizada

- **Python 3.x** · **Pandas** · **Scikit-learn**
- **Matplotlib** — visualização da árvore e feature importance
- **Jupyter Notebook** — ambiente de experimentação

---

## Evoluções Futuras

- [ ] Tuning de hiperparâmetros com GridSearchCV
- [ ] Comparativo com Random Forest e Gradient Boosting
- [ ] Deploy do modelo com FastAPI