# Regressão Linear no Mercado Imobiliário

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-F7931E?logo=scikitlearn&logoColor=white)
![Statsmodels](https://img.shields.io/badge/Statsmodels-3d5a80?logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)

[:fontawesome-brands-github: Repositório](https://github.com/RToyoyama/linear-regression-real-estate){ .md-button .md-button--primary }

---

## Visão Geral

Regressão linear simples e múltipla para prever o valor de aluguel de imóveis.
O projeto vai além da modelagem — inclui tratamento robusto de outliers,
análise de multicolinearidade e diagnóstico de overfitting.

---

## Variáveis do Dataset

| Variável | Tipo | Descrição |
|----------|------|-----------|
| `ValorAluguel` | Target | Valor mensal do aluguel |
| `Metragem` | Contínua | Área total em m² |
| `NQuartos` | Discreta | Número de quartos |
| `NVagas` | Discreta | Vagas de garagem |
| `NBanheiros` | Discreta | Número de banheiros |
| `NSuites` | Discreta | Número de suítes |
| `ValorCondominio` | Contínua | Taxa de condomínio |

!!! warning "Desafio dos Outliers"
    O dataset contém imóveis de luxo com valores extremos. A estratégia adotada
    foi **winsorização por IQR** para variáveis contínuas e **winsorização por
    percentil** para variáveis discretas — sem exclusão de amostras.

---

## Fluxo do Projeto

=== "1. Tratamento de Outliers"
    Winsorização por IQR para variáveis contínuas (aluguel, metragem).
    Winsorização por percentil para variáveis discretas (banheiros, suítes).

=== "2. Análise Exploratória"
    Estatísticas descritivas, boxplots e matriz de correlação.

=== "3. Modelagem"
    - **Regressão simples** — apenas metragem como preditor
    - **Regressão múltipla** — todas as variáveis disponíveis
    - Extração de coeficientes e equações dos modelos

=== "4. Validação"
    - R² em treino e teste
    - Diagnóstico de overfitting e multicolinearidade
    - Recomendação de regularização (Ridge/Lasso)

---

## Implementação

```python
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import r2_score

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Regressão simples
lr_simple = LinearRegression()
lr_simple.fit(X_train[["Metragem"]], y_train)

# Regressão múltipla
lr_multi = LinearRegression()
lr_multi.fit(X_train, y_train)

print(f"R² simples  — Treino: {r2_score(y_train, lr_simple.predict(X_train[['Metragem']])):.4f}")
print(f"R² simples  — Teste : {r2_score(y_test,  lr_simple.predict(X_test[['Metragem']])):.4f}")
print(f"R² múltiplo — Treino: {r2_score(y_train, lr_multi.predict(X_train)):.4f}")
print(f"R² múltiplo — Teste : {r2_score(y_test,  lr_multi.predict(X_test)):.4f}")
```

---

## Diagnóstico

!!! danger "Overfitting identificado"
    O modelo múltiplo apresentou R² altíssimo no treino, mas não generalizou
    para o teste — causado por **multicolinearidade** entre quartos, banheiros e suítes.

!!! tip "Recomendações"
    - Aplicar **Ridge** ou **Lasso** para regularização
    - Selecionar variáveis com VIF (Variance Inflation Factor)
    - Avaliar transformações logarítmicas em variáveis assimétricas

---

## Stack Utilizada

- **Python 3.x** · **Pandas** · **NumPy**
- **Scikit-learn** — modelagem e métricas
- **Statsmodels** — análise estatística detalhada
- **Matplotlib / Seaborn** — visualizações
- **Jupyter Notebook** — ambiente de experimentação

---

## Evoluções Futuras

- [ ] Implementar Ridge e Lasso com cross-validation
- [ ] Análise de VIF para seleção de variáveis
- [ ] Modelo com variáveis geográficas (bairro, cidade)