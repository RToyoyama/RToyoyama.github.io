# プロジェクト

パイプラインエンジニアリングから予測モデリングまで、
データの完全なサイクルをカバーする個人プロジェクトリポジトリ。

---

## データエンジニアリング

### [通貨ETLパイプライン](currency-etl-pipeline.md)

![Python](https://img.shields.io/badge/Python-3.13-blue?logo=python&logoColor=white)
![Airflow](https://img.shields.io/badge/Apache%20Airflow-2.9-017CEE?logo=apacheairflow&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-336791?logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)

USD、EUR、BTCの為替レートをリアルタイムで収集し、Pandasで変換して
PostgreSQLに保存する自動ETLパイプライン。Airflowで毎時オーケストレーション、
DockerでコンテナKA化、GitHub ActionsでCI/CD。

[:octicons-arrow-right-24: プロジェクトを見る](currency-etl-pipeline.md)

---

## データサイエンス — 機械学習

### [決定木による分類](arvore-decisao.md)

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-F7931E?logo=scikitlearn&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)

ジニ基準による決定木で教師あり分類を実装、テストセットで94%の精度を達成。
特徴量重要度、ツリー可視化、ナイーブベイズとの比較を含む。

[:octicons-arrow-right-24: プロジェクトを見る](arvore-decisao.md)

---

### [ナイーブベイズによる信用スコア](credit-score-naivebayes.md)

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-F7931E?logo=scikitlearn&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white)

ガウスナイーブベイズによる信用リスク分類 — テストセットで98%の精度、
マクロリコール99%、優れた汎化能力。

[:octicons-arrow-right-24: プロジェクトを見る](credit-score-naivebayes.md)

---

### [不動産市場の線形回帰](regressao-imobiliaria.md)

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-F7931E?logo=scikitlearn&logoColor=white)
![Statsmodels](https://img.shields.io/badge/Statsmodels-3d5a80?logoColor=white)

家賃予測のための単純・重回帰分析。IQRウィンザライゼーションによる
堅牢な外れ値処理、多重共線性分析、過学習診断。

[:octicons-arrow-right-24: プロジェクトを見る](regressao-imobiliaria.md)

---

## データサイエンス — 探索的分析

### [スーパーマーケット売上分析](supermarket-sales.md)

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-4C72B0?logoColor=white)

スーパーマーケット売上データの完全なEDA — 消費パターン、
季節性、戦略的インサイトの特定。プロフェッショナルな可視化による記述統計分析。

[:octicons-arrow-right-24: プロジェクトを見る](supermarket-sales.md)

---

## ソフトウェアエンジニアリング

### [EduConnect — 教育管理システム](educonnect-sge.md)

![Java](https://img.shields.io/badge/Java-11%2B-ED8B00?logo=openjdk&logoColor=white)
![OOP](https://img.shields.io/badge/OOP-SOLID-4caf50)

3層アーキテクチャ、SOLID原則、継承、ポリモーフィズム、インターフェースを備えた
完全なJavaシステム。学生、教師、コース、クラス、評価を管理。

[:octicons-arrow-right-24: プロジェクトを見る](educonnect-sge.md)

---

### [ハノイの塔](torre-hanoi.md)

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python&logoColor=white)
![OOP](https://img.shields.io/badge/OOP-SOLID-4caf50)

ソフトウェアエンジニアリングに焦点を当てたPythonでの古典的問題実装：
オブジェクト指向、SRP、YAML外部設定、argparseによる堅牢なCLI。

[:octicons-arrow-right-24: プロジェクトを見る](torre-hanoi.md)

---

!!! tip "近日公開予定の新プロジェクト"
    このポートフォリオは継続的に更新されています。
    最新リリースは[GitHub](https://github.com/RToyoyama)をフォローしてください。