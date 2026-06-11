# 通貨ETLパイプライン

[![CI](https://github.com/RToyoyama/Currency-ETL-Pipeline/actions/workflows/ci.yml/badge.svg)](https://github.com/RToyoyama/Currency-ETL-Pipeline/actions/workflows/ci.yml)
![Python](https://img.shields.io/badge/Python-3.13-blue?logo=python&logoColor=white)
![Airflow](https://img.shields.io/badge/Apache%20Airflow-2.9-017CEE?logo=apacheairflow&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-336791?logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)

[:fontawesome-brands-github: リポジトリ](https://github.com/RToyoyama/Currency-ETL-Pipeline){ .md-button .md-button--primary }

---

## 概要

リアルタイムで為替レートを収集し、データを変換してPostgreSQLに保存する
自動ETLパイプライン — Apache Airflowでオーケストレーション、Dockerでコンテナ化。

!!! success "ハイライト"
    - 本番環境での完全なExtract → Transform → Loadパイプライン
    - Airflow UIでの監視とリトライによる毎時自動実行
    - GitHub ActionsによるCI/CD：プッシュごとにlint、フォーマット、テスト
    - 外部依存なしのテスト（APIモック、インメモリSQLite）

---

## アーキテクチャ

```
API (AwesomeAPI)
      │
      ▼
┌─────────────┐     ┌─────────────────┐     ┌──────────────┐
│   抽出      │────▶│    変換         │────▶│   ロード     │
│  Requests   │     │  Pandas         │     │  SQLAlchemy  │
│  バリデーション│   │  型付け         │     │  PostgreSQL  │
└─────────────┘     └─────────────────┘     └──────────────┘
                            │
                            ▼
              ┌─────────────────────────┐
              │     Apache Airflow      │
              │  毎時オーケストレーション │
              │  自動リトライ           │
              └─────────────────────────┘
```

---

## 技術スタック

| 技術 | バージョン | 役割 |
|---|---|---|
| Python | 3.13 | メイン言語 |
| Apache Airflow | 2.9.2 | パイプラインオーケストレーション |
| PostgreSQL | 15 | データストレージ |
| Pandas | 2.2+ | データ変換 |
| SQLAlchemy | 2.0+ | データベース抽象化 |
| Docker / Compose | latest | コンテナ化 |
| Pytest | 8.0+ | 自動テスト |
| Ruff | 0.4+ | リントとフォーマット |

---

## 今後の改善

- [ ] Rawレイヤー — ParquetでAPIの生データを保存
- [ ] 冪等性 — 重複挿入の防止
- [ ] Great Expectations — データ品質検証
- [ ] ダッシュボード — 為替レート履歴の可視化
