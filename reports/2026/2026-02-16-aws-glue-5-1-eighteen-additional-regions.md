# AWS Glue 5.1 - 18 リージョンへの拡大

**リリース日**: 2026 年 2 月 16 日
**サービス**: AWS Glue
**機能**: AWS Glue 5.1 の 18 追加リージョンでの提供開始

📊 [このアップデートのインフォグラフィックを見る](https://takech9203.github.io/aws-news-summary/20260216-aws-glue-5-1-eighteen-additional-regions.html)

## 概要

AWS Glue 5.1 が 18 の追加 AWS リージョンで利用可能になった。今回の拡大により、AWS Glue 5.1 は合計 33 リージョンで提供される。Glue 5.1 はコアエンジンを Apache Spark 3.5.6、Python 3.11、Scala 2.12.18 にアップグレードし、パフォーマンスとセキュリティの強化をもたらす。

特筆すべき機能として、Apache Iceberg フォーマットバージョン 3.0 のサポートが追加され、デフォルトカラム値、マージオンリードテーブルの削除ベクトル、マルチ引数変換、行リネージ追跡が可能になった。また、AWS Lake Formation のきめ細かなアクセス制御が書き込み操作 (DML/DDL) にも拡張された。

**アップデート前の課題**

- Glue 5.1 が利用可能なリージョンが 15 リージョンに限定されていた
- アジア太平洋、カナダ、中東などの一部リージョンでは Glue 5.1 の新機能を活用できなかった
- リージョン制約により、データレジデンシー要件を満たしながら最新機能を使用することが困難だった

**アップデート後の改善**

- 33 リージョンで Glue 5.1 が利用可能になり、グローバル展開が大幅に拡大
- 大阪、ソウル、台北などのアジア太平洋リージョンでも Iceberg 3.0 や Lake Formation 書き込み制御が利用可能
- データレジデンシー要件を満たしながら最新のデータ統合機能を活用可能

## サービスアップデートの詳細

### 主要機能

1. **コアエンジンのアップグレード**
   - Apache Spark 3.5.6
   - Python 3.11
   - Scala 2.12.18
   - パフォーマンスとセキュリティの強化

2. **オープンテーブルフォーマットのアップデート**
   - Apache Hudi 1.0.2
   - Apache Iceberg 1.10.0
   - Delta Lake 3.3.2

3. **Apache Iceberg フォーマットバージョン 3.0 サポート**
   - デフォルトカラム値
   - マージオンリードテーブルの削除ベクトル
   - マルチ引数変換
   - 行リネージ追跡

4. **Lake Formation 書き込みアクセス制御**
   - Spark DataFrames および Spark SQL の書き込み操作に対するきめ細かなアクセス制御
   - 以前は読み取り操作のみに限定されていた
   - Apache Hudi および Delta Lake テーブルのフルテーブルアクセス制御も追加

## 技術仕様

### 新規追加リージョン

| リージョン | コード |
|-----------|--------|
| Africa (Cape Town) | af-south-1 |
| Asia Pacific (Hyderabad) | ap-south-2 |
| Asia Pacific (Jakarta) | ap-southeast-3 |
| Asia Pacific (Melbourne) | ap-southeast-4 |
| Asia Pacific (Osaka) | ap-northeast-3 |
| Asia Pacific (Seoul) | ap-northeast-2 |
| Asia Pacific (Taipei) | ap-east-2 |
| Canada (Calgary) | ca-west-1 |
| Canada (Central) | ca-central-1 |
| Europe (London) | eu-west-2 |
| Europe (Milan) | eu-south-1 |
| Europe (Paris) | eu-west-3 |
| Europe (Zurich) | eu-central-2 |
| Israel (Tel Aviv) | il-central-1 |
| Mexico (Central) | mx-central-1 |
| Middle East (Bahrain) | me-south-1 |
| Middle East (UAE) | me-central-1 |
| US West (N. California) | us-west-1 |

### エンジンバージョン比較

| コンポーネント | Glue 5.1 | Glue 5.0 |
|---------------|----------|----------|
| Apache Spark | 3.5.6 | 3.5.4 |
| Python | 3.11 | 3.11 |
| Scala | 2.12.18 | 2.12.18 |
| Java | 17 | 17 |
| Apache Hudi | 1.0.2 | 0.15.0 |
| Apache Iceberg | 1.10.0 | 1.7.1 |
| Delta Lake | 3.3.2 | 3.3.0 |

## 設定方法

### 前提条件

1. 対象リージョンに AWS アカウントが設定されていること
2. AWS Glue へのアクセス権限があること
3. 必要に応じて Lake Formation の設定が完了していること

### 手順

#### ステップ 1: Glue 5.1 ジョブの作成

```bash
aws glue create-job \
  --name "my-glue-job" \
  --role "arn:aws:iam::role/GlueServiceRole" \
  --command '{"Name":"glueetl","ScriptLocation":"s3://my-bucket/scripts/my-script.py","PythonVersion":"3"}' \
  --glue-version "5.1" \
  --region ap-northeast-3
```

Glue 5.1 バージョンを指定してジョブを作成する。`--glue-version "5.1"` で明示的にバージョンを指定する。

#### ステップ 2: Iceberg テーブルの操作

```python
# Iceberg フォーマット v3 テーブルの作成例
spark.sql("""
    CREATE TABLE my_catalog.my_db.my_table (
        id BIGINT,
        name STRING,
        created_at TIMESTAMP
    )
    USING iceberg
    TBLPROPERTIES ('format-version'='3')
""")
```

Glue 5.1 で Apache Iceberg フォーマットバージョン 3.0 のテーブルを作成する例。

## メリット

### ビジネス面

- **グローバル展開**: 33 リージョンでの提供により、世界中のデータレジデンシー要件に対応
- **最新技術の活用**: すべてのリージョンで Iceberg 3.0 や Lake Formation 書き込み制御を利用可能
- **コンプライアンス対応**: データを特定リージョンに保持しながら最新のデータ統合機能を活用

### 技術面

- **Iceberg 3.0**: 削除ベクトルによるマージオンリードの効率化と行リネージ追跡
- **書き込みアクセス制御**: Lake Formation によるきめ細かなデータ保護が読み取りと書き込みの両方に対応
- **パフォーマンス向上**: Spark 3.5.6 および最新ライブラリによる処理速度の改善

## デメリット・制約事項

### 制限事項

- Glue 2.0 および 1.0 は 2026 年 4 月 1 日にサポート終了予定
- 既存ジョブの Glue 5.1 への移行にはスクリプトの互換性確認が必要

### 考慮すべき点

- Python 3.11 への移行に伴い、一部のサードパーティライブラリの互換性を確認する必要がある
- Iceberg フォーマット v3 はまだすべてのツールでサポートされていない可能性がある

## ユースケース

### ユースケース 1: アジア太平洋でのデータレイク構築

**シナリオ**: 日本企業が大阪リージョンでデータレイクを構築し、Iceberg テーブルを活用したい

**効果**: 大阪リージョンで Glue 5.1 が利用可能になり、Iceberg 3.0 の機能を活用したデータレイクを国内リージョンに構築できる

### ユースケース 2: Lake Formation によるデータガバナンス強化

**シナリオ**: 金融機関が複数リージョンのデータに対してきめ細かな書き込みアクセス制御を適用したい

**効果**: Lake Formation の書き込み操作に対するアクセス制御が全 33 リージョンで利用可能になり、統一的なデータガバナンスを実現

## 料金

AWS Glue の標準料金が適用される。リージョンによって料金が異なる場合がある。詳細は [AWS Glue 料金ページ](https://aws.amazon.com/glue/pricing/) を参照。

## 利用可能リージョン

合計 33 リージョンで利用可能。完全なリストは [AWS Glue ドキュメント](https://docs.aws.amazon.com/glue/latest/dg/release-notes.html) を参照。

## 関連サービス・機能

- **AWS Lake Formation**: データレイクのアクセス制御とガバナンス
- **Amazon SageMaker Unified Studio**: Glue ジョブの統合開発環境
- **Apache Iceberg**: オープンテーブルフォーマットによるデータレイク管理

## 参考リンク

- 📊 [インフォグラフィック](https://takech9203.github.io/aws-news-summary/20260216-aws-glue-5-1-eighteen-additional-regions.html)
- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/aws-glue-5-1-eighteen-additional-regions)
- [AWS Blog - Introducing AWS Glue 5.1 for Apache Spark](https://aws.amazon.com/blogs/big-data/introducing-aws-glue-5-1-for-apache-spark/)
- [AWS Glue プロダクトページ](https://aws.amazon.com/glue/)
- [AWS Glue ドキュメント](https://docs.aws.amazon.com/glue/latest/dg/release-notes.html)

## まとめ

AWS Glue 5.1 の 18 リージョン拡大により、Apache Iceberg 3.0 や Lake Formation 書き込みアクセス制御などの最新機能がグローバルに利用可能になった。大阪やソウルなどアジア太平洋リージョンのユーザーは、データレジデンシー要件を満たしながら最新のデータ統合機能を活用できるようになるため、Glue 5.1 へのアップグレードを検討すべきである。
