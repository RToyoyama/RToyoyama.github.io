# Titanic — Final Model Duel

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-F7931E?logo=scikitlearn&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-189FDD?logoColor=white)
![Pytest](https://img.shields.io/badge/Pytest-65%20tests-0A9EDC?logo=pytest&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Kaggle](https://img.shields.io/badge/Kaggle-20BEFF?logo=kaggle&logoColor=white)

[:fontawesome-brands-github: Repository](https://github.com/RToyoyama/titanic-duelo-final){ .md-button .md-button--primary }

---

## Overview

Module 41 project (Profissão Cientista de Dados). A solution to Kaggle's
[Titanic: Machine Learning from Disaster](https://www.kaggle.com/competitions/titanic)
competition, built as an engineering project: tested Python package, leak-free
pipelines, reproducible experiments and a Docker environment.

| Final model | CV accuracy (5×3) | Kaggle score |
|---|---|---|
| Hard voting (LogReg + SVM + Random Forest + XGBoost) | **0.8503** | **0.77751** |

!!! success "Highlights"
    - **Leak-free group survival feature:** a scikit-learn transformer using leave-one-out, recomputed inside each fold. On its own, it took logistic regression from 0.8294 to 0.8440
    - **Rigorous validation:** `RepeatedStratifiedKFold` (5×3), paired fold-by-fold comparison and separate seeds for search and evaluation
    - **Decision criteria set before seeing results:** class balancing and the woman-child-group rule were tested and discarded based on evidence
    - **End-to-end reproducible:** locked dependencies, fixed seeds and Docker — the container reproduces local metrics to the 4th decimal place
    - **Code quality:** 65 tests, ruff, pre-commit and Conventional Commits

---

## Project Structure

```
titanic-duelo-final/
├── data/                  # raw, processed and submissions
├── models/                # trained .joblib models
├── notebooks/
│   ├── 01_eda.ipynb                                   # exploratory analysis
│   └── MOD41_EXERCICIO_DUELOFINAL_Renan_Toyoyama.ipynb  # delivery notebook
├── reports/               # results of each experiment + figures
├── src/titanic/           # features, transformers, models, evaluate,
│                          # tuning, ensemble, predict...
├── tests/                 # 65 tests (pytest)
├── Dockerfile / Makefile
├── pyproject.toml
└── requirements.lock
```

The core rule of the project: **every statistic learned from the data lives
inside the pipeline**. Row-wise features (Title, FamilySize, Deck) are computed
upfront; imputation, scaling and group survival are fitted within each fold.

---

## Tech Stack

| Technology | Role |
|---|---|
| Python 3.11 | Main language |
| pandas | Data manipulation and feature engineering |
| scikit-learn | Pipelines, custom transformers, CV, tuning and ensembles |
| XGBoost | Gradient boosting model |
| imbalanced-learn | Class balancing experiment (SMOTE) |
| Matplotlib / Seaborn | Visualizations |
| pytest / ruff / pre-commit | Testing and code quality |
| Docker / Make / Kaggle CLI | Reproducible execution, data download and submission |

---

## Methodology

1. **Decision-driven EDA:** sex and class dominate; the title summarizes sex, age and status; ticket groups tend to survive or die together; a missing cabin is itself information.
2. **Features:** `Title`, `FamilySize`/`FamilyBand`, `Deck`/`HasCabin`, `TicketGroupSize`, `FarePerPerson`, `Age` imputed by title median and `group_survival` (leave-one-out).
3. **Preprocessing:** `ColumnTransformer` with imputation, `log1p` on fare, `StandardScaler` (only for linear models/SVM) and one-hot encoding.
4. **Validation:** `RepeatedStratifiedKFold` 5×3 — a single 5-fold split varied by 0.7 p.p.
5. **Class balancing:** none × `class_weight` × SMOTE. No balancing won in 3 of 4 models.
6. **Tuning:** `GridSearchCV` (LogReg, SVM) and `RandomizedSearchCV` (RF, XGBoost), re-evaluated with a different seed — revealing 0.77 p.p. of optimism in XGBoost.
7. **Ensembles:** out-of-fold error correlation showed two families (linear and tree-based); hard voting won.

![Baseline duel](https://raw.githubusercontent.com/RToyoyama/titanic-duelo-final/main/reports/figures/baseline_duel.png)

---

## Results

### Project evolution

| Stage | Model | CV accuracy | Kaggle |
|---|---|---|---|
| Reference | "Only women survive" | — | ~0.766 |
| Baseline | Logistic regression, default parameters | 0.8373 | — |
| Tuning | Tuned Random Forest | 0.8485 | 0.77751 |
| Ensemble | **Hard voting** | **0.8503** | **0.77751** |

### Final duel

| Model | Accuracy | Std. dev. | ROC AUC | Win rate vs RF |
|---|---|---|---|---|
| **Hard voting** | **0.8503** | **0.0156** | — | 67% |
| Tuned Random Forest | 0.8485 | 0.0183 | 0.8894 | — |
| Stacking | 0.8444 | 0.0188 | 0.8930 | 33% |
| Soft voting | 0.8440 | 0.0167 | **0.8940** | 33% |

### Experiments that didn't work

| Experiment | Result | Why |
|---|---|---|
| SMOTE and `class_weight` | Lower accuracy in 3 of 4 models | They shift the threshold toward the minority class; with a 62/38 split this causes more errors than hits |
| Stacking and soft voting | Best AUC, worst accuracy | Averaging probabilities pulls values toward the middle and borderline passengers flip sides |
| Woman-child-group rule | 7–33% win rate | Redundant: `group_survival` + `Title` + `Sex` already captured the pattern |

!!! info "Lessons learned"
    - Every statistic learned from the data goes **inside the pipeline**.
    - A difference in the mean is not a real difference: XGBoost "led" the baseline while winning only 40% of folds.
    - The best score of a search is **optimistic**; re-evaluating with another seed measures that bias.
    - The best model **depends on the metric**: the AUC champion was last in accuracy.
    - CV is an estimate, not a promise: 0.85 in CV became 0.78 on Kaggle.

---

## How to Run

!!! warning "Prerequisites"
    Kaggle token saved at `~/.kaggle/access_token` and competition rules
    accepted via **Join Competition** on the Titanic page.

=== "Docker"

    ```bash
    git clone https://github.com/RToyoyama/titanic-duelo-final.git
    cd titanic-duelo-final
    make docker-build      # build the image
    make docker-data       # download data from Kaggle
    make docker-test       # run the 65 tests
    make docker-baseline   # any Makefile target works as docker-<target>
    ```

=== "Local"

    ```bash
    git clone https://github.com/RToyoyama/titanic-duelo-final.git
    cd titanic-duelo-final
    uv venv --python 3.11 --seed .venv
    source .venv/bin/activate
    pip install -r requirements.lock
    make install
    make data
    make test
    ```

Main targets: `make baseline`, `make balance`, `make tune`, `make ensemble`,
`make wcg`, `make predict` and `make submit`.

!!! note
    The notebooks and documentation are written in Portuguese.
