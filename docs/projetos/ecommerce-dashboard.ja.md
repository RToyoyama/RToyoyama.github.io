# Eコマースダッシュボード

![Python](https://img.shields.io/badge/Python-3.13-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?logo=sqlite&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![Chart.js](https://img.shields.io/badge/Chart.js-4.4.0-FF6384?logo=chartdotjs&logoColor=white)

---

## 概要

Profissão Ciência de Dados コースのアナリティクス深化モジュールの最終プロジェクト。
SQLを使用してEコマースのトランザクションデータと顧客データを統合し、
動的フィルター付きのインタラクティブHTMLダッシュボードで結果を表示する。

!!! success "ハイライト"
    - 完全なSQLパイプライン：読み込み、正規化、INNER JOIN、エクスポート
    - 自己完結型HTMLダッシュボード — 296件のレコードを埋め込み、サーバー不要
    - 性別・州・カテゴリのフィルター付き7種類のインタラクティブ指標
    - 生データの正規化（ヨーロッパ形式 `72,93` → 小数点 `72.93`）

---

## プロジェクト構造

```
ecommerce-dashboard/
├── data/
│   ├── raw/                 # ソースデータ（トランザクションと顧客）
│   └── processed/           # 統合データセット（JOINの結果）
├── notebooks/
│   └── Profissao Cientista de Dados M26 Projeto.ipynb
├── src/
│   └── dashboard.html       # インタラクティブダッシュボード（Chart.js）
├── requirements.txt
└── README.md
```

---

## 技術スタック

| 技術 | バージョン | 役割 |
|---|---|---|
| Python | 3.13 | メイン言語 |
| pandas | latest | データ操作とエクスポート |
| SQLite（`sqlite3`） | built-in | JOINのためのインメモリDB |
| Jupyter Notebook | latest | インタラクティブなデータパイプライン |
| Chart.js | 4.4.0 | ダッシュボードの可視化 |

---

## データパイプライン

```
TB_TRANSACOES_PROJETO_ECOMM.csv   TB_CLIENTES_PROJETO_ECOMM.csv
              │                                │
              ▼                                ▼
     ┌────────────────┐             ┌────────────────────┐
     │  Raw読み込み   │             │   Raw読み込み      │
     │  正規化        │             │                    │
     │  Price: ,→.   │             │                    │
     └────────┬───────┘             └─────────┬──────────┘
              │                               │
              ▼                               ▼
     ┌─────────────────────────────────────────────────┐
     │          インメモリSQLite                        │
     │  id_client でINNER JOIN                         │
     └─────────────────────────┬───────────────────────┘
                               │
                               ▼
         dados_ecommerce_final.csv（296件のレコード）
                               │
                               ▼
                  dashboard.html（自己完結型）
```

### JOINの選択理由

**INNER JOIN** を採用した理由は、特定された顧客に紐づくトランザクションの分析が
目的だから。顧客テーブルに対応レコードがないトランザクション（孤立レコード）は
人口統計・地理的メトリクスに価値を持たないため除外 — 結果として296件の統合レコード。

---

## ダッシュボード

`src/dashboard.html` は自己完結型：296件のレコードがHTML内に直接埋め込まれており、
サーバーやライブデータベース接続は不要。

**表示メトリクス：**

| メトリクス | タイプ |
|---|---|
| 総売上と平均チケット | KPIカード |
| トランザクション数とユニーク顧客数 | KPIカード |
| 州別地理的分布 | 棒グラフ |
| 性別人口統計プロファイル | ドーナツグラフ |
| 商品カテゴリ別売上 | 横棒グラフ |
| 価格分布 | ヒストグラム（10ビン） |
| トランザクション数上位の職種 | 横棒グラフ |

**インタラクティブフィルター：** 性別、州、カテゴリ — フィルター選択時にすべての
KPIとグラフが更新される。

---

## 実行方法

### データパイプライン（オプション）

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

ノートブックを開いてすべてのセルを実行。
`data/processed/dados_ecommerce_final.csv` が生成される。

### ダッシュボード

**オプションA — ローカルサーバー（推奨）：**

```bash
python3 -m http.server 8000
```

`http://localhost:8000/src/dashboard.html` を開く。

**オプションB — ブラウザで直接開く：**

`src/dashboard.html` をダブルクリックまたは `File > Open`。
