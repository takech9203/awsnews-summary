# Amazon Redshift - マテリアライズドビュー (MV) のデータ共有機能強化

**リリース日**: 2025 年 12 月 23 日
**サービス**: Amazon Redshift
**機能**: マテリアライズドビュー (MV) のデータ共有機能強化

## 概要

Amazon Redshift で、マテリアライズドビュー (MV) に関する 4 つの新機能がリリースされました。複数の Amazon Redshift データウェアハウスから MV の作成とリフレッシュコマンドを実行できるようになり、共有された MV 上に新たな MV を作成することも可能になりました。さらに、MV 作成の DDL コマンドに対するコンカレンシースケーリングもサポートされました。

これらの機能強化により、大規模な分析ワークロードのスケーリングと、予測可能な SLA を持つ回復力のある分析アプリケーションの構築が容易になります。

**アップデート前の課題**

- MV の作成とリフレッシュは単一のデータウェアハウスからのみ実行可能だった
- 共有された MV 上に新たな MV を作成できなかった
- MV 作成時にリソース不足が発生すると、処理が遅延していた

**アップデート後の改善**

- 複数のデータウェアハウスから MV の作成とリフレッシュが可能に
- 共有 MV 上に MV を作成可能に
- MV 作成 DDL のコンカレンシースケーリングにより、リソース不足時も自動スケール

## サービスアップデートの詳細

### 主要機能

1. **複数データウェアハウスからの MV 操作**
   - 複数の Amazon Redshift クラスターまたはワークグループから CREATE MV と REFRESH MV コマンドを実行可能
   - データ共有環境での柔軟な MV 管理

2. **共有 MV 上への MV 作成**
   - データ共有で共有された MV を基に、新たな MV を作成可能
   - 階層的な MV 構造の構築が可能に

3. **MV 作成 DDL のコンカレンシースケーリング**
   - CREATE MATERIALIZED VIEW DDL コマンドのコンカレンシースケーリングをサポート
   - メインクラスターのリソース不足時に自動的にスケールアウト

4. **予測可能な SLA**
   - コンカレンシースケーリングにより、一貫したパフォーマンスを提供
   - ワークロードの増加に対応した自動スケーリング

## 技術仕様

### コンカレンシースケーリングの有効化

```sql
-- コンカレンシースケーリングの有効化
ALTER SYSTEM SET enable_concurrency_scaling = true;

-- ワークロード管理 (WLM) キューでコンカレンシースケーリングを有効化
CREATE WLM QUEUE mv_queue
WITH (
    concurrency_scaling = 'auto',
    query_group = 'mv_operations'
);
```

コンカレンシースケーリングを有効化することで、MV 作成 DDL が自動的にスケールアウトされます。

### データ共有環境での MV 作成

```sql
-- プロデューサークラスターでデータ共有を作成
CREATE DATASHARE sales_share;
ALTER DATASHARE sales_share ADD SCHEMA public;
ALTER DATASHARE sales_share ADD TABLE public.sales;

-- コンシューマークラスターでデータ共有をマウント
CREATE DATABASE sales_db FROM DATASHARE sales_share
OF ACCOUNT '123456789012' NAMESPACE 'ns-xxxxx';

-- コンシューマークラスターで共有テーブル上に MV を作成
CREATE MATERIALIZED VIEW mv_sales_summary AS
SELECT 
    date_trunc('month', sale_date) as month,
    product_category,
    SUM(amount) as total_sales
FROM sales_db.public.sales
GROUP BY 1, 2;
```

データ共有環境で、共有されたテーブル上に MV を作成できます。

### 共有 MV 上への MV 作成

```sql
-- プロデューサークラスターで MV を作成し共有
CREATE MATERIALIZED VIEW mv_daily_sales AS
SELECT 
    sale_date,
    SUM(amount) as daily_total
FROM sales
GROUP BY sale_date;

ALTER DATASHARE sales_share ADD MATERIALIZED VIEW mv_daily_sales;

-- コンシューマークラスターで共有 MV 上に新たな MV を作成
CREATE MATERIALIZED VIEW mv_weekly_sales AS
SELECT 
    date_trunc('week', sale_date) as week,
    SUM(daily_total) as weekly_total
FROM sales_db.public.mv_daily_sales
GROUP BY 1;
```

共有された MV を基に、さらに集計した MV を作成できます。

## 設定方法

### 前提条件

1. Amazon Redshift クラスターまたは Serverless ワークグループ
2. データ共有の設定（複数クラスター間で使用する場合）
3. コンカレンシースケーリングの有効化（自動スケーリングを使用する場合）

### 手順

#### ステップ 1: コンカレンシースケーリングの有効化

```sql
-- パラメータグループでコンカレンシースケーリングを有効化
-- AWS コンソールまたは CLI で設定

-- WLM キューの設定
ALTER WLM QUEUE default
SET concurrency_scaling = 'auto';
```

コンカレンシースケーリングを有効化して、MV 作成時の自動スケーリングを設定します。

#### ステップ 2: データ共有の設定

```sql
-- プロデューサー側
CREATE DATASHARE analytics_share;
ALTER DATASHARE analytics_share ADD SCHEMA analytics;
ALTER DATASHARE analytics_share ADD ALL TABLES IN SCHEMA analytics;

-- コンシューマーへの権限付与
GRANT USAGE ON DATASHARE analytics_share TO ACCOUNT '123456789012';
```

データ共有を設定して、複数のクラスター間でデータを共有します。

#### ステップ 3: MV の作成とリフレッシュ

```sql
-- コンシューマークラスターで MV を作成
CREATE MATERIALIZED VIEW mv_analytics_summary AS
SELECT 
    event_date,
    event_type,
    COUNT(*) as event_count
FROM analytics_db.analytics.events
GROUP BY 1, 2;

-- MV のリフレッシュ
REFRESH MATERIALIZED VIEW mv_analytics_summary;
```

データ共有環境で MV を作成し、定期的にリフレッシュします。

## メリット

### ビジネス面

- **分析の柔軟性向上**: 複数のデータウェアハウスから MV を管理でき、分析ワークフローが柔軟に
- **SLA の予測可能性**: コンカレンシースケーリングにより、一貫したパフォーマンスを提供
- **コスト効率**: 必要な時だけスケールアウトし、コストを最適化

### 技術面

- **スケーラビリティ**: 大規模なワークロードに対応した自動スケーリング
- **階層的な MV 構造**: 共有 MV 上に MV を作成し、複雑な分析パイプラインを構築
- **マルチクラスター対応**: データ共有環境での柔軟な MV 管理

## デメリット・制約事項

### 制限事項

- コンカレンシースケーリングには追加料金が発生する場合がある
- 共有 MV のリフレッシュはプロデューサークラスターで実行する必要がある

### 考慮すべき点

- MV のリフレッシュ頻度とコストのバランスを考慮
- データ共有環境でのアクセス権限の適切な管理

## ユースケース

### ユースケース 1: マルチテナント分析環境

**シナリオ**: 複数のビジネスユニットが共有データを基に独自の分析 MV を作成

**実装例**:
```sql
-- 中央データチームがベース MV を作成・共有
CREATE MATERIALIZED VIEW mv_base_metrics AS
SELECT * FROM raw_data WHERE quality_check = 'passed';

ALTER DATASHARE central_share ADD MATERIALIZED VIEW mv_base_metrics;

-- 各ビジネスユニットが独自の MV を作成
-- マーケティングチーム
CREATE MATERIALIZED VIEW mv_marketing_metrics AS
SELECT * FROM central_db.public.mv_base_metrics
WHERE department = 'marketing';

-- セールスチーム
CREATE MATERIALIZED VIEW mv_sales_metrics AS
SELECT * FROM central_db.public.mv_base_metrics
WHERE department = 'sales';
```

**効果**: 各チームが独自の分析ニーズに合わせた MV を作成しつつ、データの一貫性を維持

### ユースケース 2: 階層的な集計パイプライン

**シナリオ**: 日次 → 週次 → 月次の階層的な集計 MV を構築

**実装例**:
```sql
-- 日次集計 MV
CREATE MATERIALIZED VIEW mv_daily AS
SELECT date, SUM(amount) as daily_total
FROM transactions GROUP BY date;

-- 週次集計 MV（日次 MV を基に）
CREATE MATERIALIZED VIEW mv_weekly AS
SELECT date_trunc('week', date) as week, SUM(daily_total) as weekly_total
FROM mv_daily GROUP BY 1;

-- 月次集計 MV（週次 MV を基に）
CREATE MATERIALIZED VIEW mv_monthly AS
SELECT date_trunc('month', week) as month, SUM(weekly_total) as monthly_total
FROM mv_weekly GROUP BY 1;
```

**効果**: 効率的な階層的集計により、クエリパフォーマンスを最適化

### ユースケース 3: 高負荷時の自動スケーリング

**シナリオ**: 月末のレポート作成時に大量の MV 作成が発生

**実装例**:
```sql
-- コンカレンシースケーリングを有効化
ALTER WLM QUEUE reporting_queue
SET concurrency_scaling = 'auto';

-- 月末レポート用の MV を一括作成
CREATE MATERIALIZED VIEW mv_monthly_report_1 AS ...;
CREATE MATERIALIZED VIEW mv_monthly_report_2 AS ...;
CREATE MATERIALIZED VIEW mv_monthly_report_3 AS ...;
-- リソース不足時は自動的にスケールアウト
```

**効果**: 高負荷時でも一貫したパフォーマンスで MV を作成

## 料金

Amazon Redshift の標準料金が適用されます。コンカレンシースケーリングを使用する場合、スケールアウトしたクラスターの使用時間に対して追加料金が発生します。

## 利用可能リージョン

Amazon Redshift が利用可能なすべての AWS リージョンで利用可能です。

## 関連サービス・機能

- **Amazon Redshift データ共有**: クラスター間でのデータ共有
- **Amazon Redshift コンカレンシースケーリング**: 自動スケーリング機能
- **Amazon Redshift Serverless**: サーバーレスデータウェアハウス

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/12/amazon-redshift-mv-datasharing-features/)
- [Amazon Redshift コンカレンシースケーリング ドキュメント](https://docs.aws.amazon.com/redshift/latest/dg/concurrency-scaling.html)
- [Amazon Redshift マテリアライズドビュー ドキュメント](https://docs.aws.amazon.com/redshift/latest/dg/materialized-view-overview.html)
- [Amazon Redshift データ共有 ドキュメント](https://docs.aws.amazon.com/redshift/latest/dg/datashare-overview.html)

## まとめ

Amazon Redshift の MV データ共有機能強化により、複数のデータウェアハウス間での柔軟な MV 管理が可能になりました。共有 MV 上への MV 作成や、MV 作成 DDL のコンカレンシースケーリングにより、大規模な分析ワークロードのスケーリングと予測可能な SLA の実現が容易になります。データ共有環境での分析パイプライン構築に最適な機能強化です。
