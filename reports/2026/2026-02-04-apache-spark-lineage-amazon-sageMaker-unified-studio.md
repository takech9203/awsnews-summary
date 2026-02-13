# SageMaker Unified Studio - Apache Spark データ系統管理

**リリース日**: 2026 年 2 月 4 日
**サービス**: Amazon SageMaker Unified Studio
**機能**: IDC ベースドメイン向け Apache Spark データ系統の一般利用開始

## 概要

Amazon SageMaker は IDC ベースドメイン向けの SageMaker Unified Studio で、Apache Spark ジョブに対するデータ系統管理の一般利用開始を発表しました。データ系統により、複雑な問題の根本原因を特定し、変更の影響を把握できます。

この機能は EMR-EC2、EMR-Serverless、EMR-EKS、AWS Glue からの Spark 実行に対する スキーマと変換のデータ資産・列単位での系統キャプチャに対応します。系統は SageMaker Unified Studio のグラフビューで視覚化でき、API でもクエリできます。また、Spark ジョブの履歴全体を横断する変換比較も可能です。

**アップデート前の課題**

- Spark ジョブの変換履歴の可視化が困難でした
- データ問題が発生した場合、根本原因特定に時間がかかりました
- 列単位での影響範囲把握ができませんでした

**アップデート後の改善**

- Spark ジョブの系統が自動キャプチャされ、SageMaker Unified Studio で可視化
- 複雑な ETL パイプラインの問題を素早く診断
- 列単位での系統トレーサビリティ実現

## サービスアップデートの詳細

### 主要機能

1. **自動系統キャプチャ**
   - Visual ETL で「Save to Project」実行時、自動的に系統キャプチャ開始
   - AWS Glue Spark と EMR Spark 対応
   - スキーマと変換の詳細トラッキング

2. **視覚化とクエリ**
   - SageMaker Unified Studio でグラフ形式で系統を表示
   - API でプログラマティックに系統データクエリ

3. **列単位トレーサビリティ**
   - データ資産単位での系統に加え、列単位での系統も追跡
   - データ品質問題を特定列に関連付け可能

4. **変換履歴比較**
   - Spark ジョブ実行履歴を横断する変換パターンを比較
   - 過去実行との差分確認

## 技術仕様

### サポート対象 Spark 実行環境

| 環境 | 対応状況 |
|------|--------|
| EMR-EC2 | ✓ 対応 |
| EMR-Serverless | ✓ 対応 |
| EMR-EKS | ✓ 対応 |
| AWS Glue (Spark) | ✓ 対応 |

### 系統キャプチャ 設定パラメータ

Visual ETL 実行時に自動設定されるパラメータ:

```json
{
    "spark.extraListeners": "io.openlineage.spark.agent.OpenLineageSparkListener",
    "spark.openlineage.transport.type": "amazon_datazone_api",
    "spark.openlineage.transport.domainId": "{DOMAIN_ID}",
    "spark.openlineage.columnLineage.datasetLineageEnabled": "True"
}
```

### OpenLineage 統合

- OpenLineage 仕様に準拠
- Amazon DataZone API を通じて系統イベント送信
- LineageEvent サイズ上限: 300 KB

## 設定方法

### 前提条件

1. SageMaker Unified Studio (IDC ベースドメイン)
2. EMR-EC2/Serverless/EKS または AWS Glue へのアクセス権限
3. OpenLineage ライブラリ対応の Spark バージョン

### 手順

#### ステップ 1: Visual ETL フロー作成

SageMaker Unified Studio で Visual ETL フロー新規作成

#### ステップ 2: ETL 変換定義

ソース、変換ステップ、出力先を定義

```bash
# Visual ETL GUI で以下の操作:
# 1. データソース選択 (S3, Glue Data Catalog など)
# 2. 変換ステップ追加 (フィルタ、集約など)
# 3. 出力先指定
```

#### ステップ 3: 「Save to Project」実行

変換完了後、**Save to Project** クリック → 系統キャプチャ自動有効化

#### ステップ 4: ジョブ実行

**Run** クリック → Spark ジョブ実行、系統自動記録

#### ステップ 5: 系統を確認

SageMaker Unified Studio の **Data Lineage** セクションで視覚化されたグラフを確認

## メリット

### ビジネス面

- **トラブルシューティング時間短縮**: 系統の自動キャプチャで根本原因を素早く特定
- **コンプライアンス**: データ系統の完全なトレーサビリティで監査対応
- **ビジネスインテリジェンス**: データパイプラインの影響範囲が明確

### 技術面

- **自動化**: OpenLineage で系統キャプチャが自動化、手動記録不要
- **スケーラビリティ**: EMR と Glue の統合で大規模 Spark ジョブも対応
- **API サポート**: プログラマティックなアクセスで自動化ワークフロー構築

## ユースケース

### ユースケース 1: データ品質問題の根本原因調査

**シナリオ**: 本番データウェアハウスで異常値が検出された

**実装例**: 系統グラフで問題列をトレース、影響を与えた変換を特定

**効果**: 根本原因特定時間が数時間から数分に短縮

### ユースケース 2: 規制監査対応

**シナリオ**: データガバナンス規制で系統証明が必要

**実装例**: SageMaker Unified Studio から系統グラフをエクスポート、監査資料として提出

**効果**: コンプライアンス負担軽減、系統証跡の自動生成

### ユースケース 3: ETL パイプライン最適化

**シナリオ**: 複数ジョブの変換パターンを分析

**実装例**: 変換履歴比較 API で過去の実行との差分分析

**効果**: データパイプラインのボトルネック特定、パフォーマンス改善

## 利用可能リージョン

SageMaker Unified Studio サポート全リージョン

### サポートされる Spark バージョン要件

- EMR-EC2/EKS: 7.11 以上
- EMR-Serverless: 7.5 以上
- AWS Glue: 5.0 以上 (Spark DataFrame)

## 制限事項

- Dynamic DataFrame は未サポート (Spark DataFrame のみ)
- LineageEvent サイズ上限 300 KB
- IDC ベースドメイン のみ対応 (通常ドメイン未対応)

## 関連サービス・機能

- **AWS Glue**: ETL ジョブの系統管理
- **Amazon EMR**: Spark ジョブ実行
- **AWS DataZone**: 系統イベントの集約管理
- **Amazon S3**: Spark 出力データストレージ

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/apache-spark-lineage-amazon-sageMaker-unified-studio)
- [SageMaker Unified Studio データ系統 ドキュメント](https://docs.aws.amazon.com/sagemaker-unified-studio/latest/userguide/datazone-data-lineage-automate-capture-from-tools.html)
- [SageMaker Unified Studio サポートリージョン](https://docs.aws.amazon.com/sagemaker-unified-studio/latest/adminguide/supported-regions.html)

## まとめ

SageMaker Unified Studio の Apache Spark 系統管理により、複雑な ETL パイプラインのトレーサビリティが確保されます。自動キャプチャと視覚化により、データ品質問題の根本原因特定が加速し、コンプライアンス対応も容易になります。大規模なデータ処理ワークロードの可視化・最適化に不可欠なツールとなります。
