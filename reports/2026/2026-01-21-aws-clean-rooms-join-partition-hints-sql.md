# AWS Clean Rooms - SQL クエリの join および partition ヒント機能

**リリース日**: 2026 年 1 月 21 日
**サービス**: AWS Clean Rooms
**機能**: SQL クエリの join および partition ヒント機能

## 概要

AWS Clean Rooms が、SQL クエリで join および partition ヒントをサポートし、join 戦略とデータ パーティショニングを最適化してクエリパフォーマンスを向上させ、コストを削減できるようになりました。このアップデートにより、事前承認された分析テンプレートおよび アドホック SQL クエリにコメントスタイルの構文を使用して SQL ヒントを適用できます。

broadcast join ヒントを使用して大規模テーブルの join を最適化し、partition ヒントを使用してより良い並列処理のためのデータ分散を改善できます。例えば、測定会社がライブスポーツイベントを視聴した世帯数を分析する際、ルックアップテーブルに broadcast join ヒントを使用してクエリパフォーマンスを向上させ、コストを削減できます。

**アップデート前の課題**

- 大規模テーブルの join では、デフォルトの join 戦略が最適でない場合があり、クエリパフォーマンスが低下していました
- データ分散が均等でない場合、並列処理の効率が低下し、クエリ実行時間が長くなっていました
- クエリ最適化のための詳細な制御が不足しており、コストとパフォーマンスのバランスを取ることが困難でした

**アップデート後の改善**

- broadcast join ヒントを使用して、小さいテーブルをブロードキャストし、大規模テーブルの join を最適化できるようになりました
- partition ヒントを使用して、データを効率的に分散し、並列処理のパフォーマンスを向上させることができるようになりました
- コメントスタイルの構文で簡単にヒントを適用でき、事前承認された分析テンプレートおよび アドホック SQL クエリの両方で使用できるようになりました

## サービスアップデートの詳細

### 主要機能

1. **Join ヒント**
   - **BROADCAST**: 小さいテーブルをすべてのノードにブロードキャストして join を実行します。小さいルックアップテーブルと大規模ファクトテーブルの join に最適です
   - **MERGE**: ソートキーに基づいてテーブルをマージする shuffle sort merge join を提案します
   - **SHUFFLE_HASH**: 一方のテーブルをビルド側として使用し、もう一方をプローブ側として使用する shuffle hash join を提案します
   - **SHUFFLE_REPLICATE_NL**: 一方のテーブルを複製してネストループ join を実行します

2. **Partitioning ヒント**
   - **COALESCE**: パーティション数を指定された数に削減します
   - **REPARTITION**: ラウンドロビン分散を使用して、指定されたパーティション数にデータを再分割します
   - **REPARTITION_BY_RANGE**: 指定された列で範囲パーティショニングを使用して、データを再分割します
   - **REBALANCE**: クエリ結果の出力パーティションを再バランスし、各パーティションを適切なサイズにします

3. **コメントスタイルの構文**
   - SQL クエリ内でコメントスタイルの構文を使用してヒントを適用します
   - SELECT キーワードの直後に配置する必要があります
   - 複数のヒントをカンマで区切って指定できます

## 技術仕様

### Join ヒントの構文例

```sql
-- Broadcast join ヒントの使用例
SELECT /*+ BROADCAST(small_table) */
  l.id, s.name, l.value
FROM large_table l
JOIN small_table s ON l.small_id = s.id;

-- 複数テーブルへの Broadcast join ヒント
SELECT /*+ BROADCAST(lookup1, lookup2) */
  f.id, l1.name, l2.category
FROM fact_table f
JOIN lookup1 l1 ON f.lookup1_id = l1.id
JOIN lookup2 l2 ON f.lookup2_id = l2.id;
```

### Partitioning ヒントの構文例

```sql
-- Repartition ヒントの使用例
SELECT /*+ REPARTITION(100) */
  customer_id, SUM(purchase_amount)
FROM purchases
GROUP BY customer_id;

-- 列指定による Repartition ヒント
SELECT /*+ REPARTITION(customer_id, region) */
  customer_id, region, COUNT(*)
FROM customers
GROUP BY customer_id, region;

-- Rebalance ヒントの使用例
SELECT /*+ REBALANCE */
  category, AVG(price)
FROM products
GROUP BY category;
```

### ヒントの組み合わせ

```sql
-- Join ヒントと Partitioning ヒントの組み合わせ
SELECT /*+ BROADCAST(dim_customer), REPARTITION(100) */
  f.order_id, c.customer_name, f.total_amount
FROM fact_orders f
JOIN dim_customer c ON f.customer_id = c.customer_id
WHERE f.order_date >= '2026-01-01';
```

### ヒント適用の制約

| 制約 | 説明 |
|------|------|
| 配置位置 | SELECT キーワードの直後に配置する必要があります |
| パラメータ | 数値パラメータは 1 から 2147483647 の正の整数である必要があります |
| 列名 | REPARTITION および REPARTITION_BY_RANGE ヒントの列名は入力スキーマに存在する必要があります |
| サポート範囲 | Differential Privacy SQL クエリまたは PySpark ジョブではサポートされていません |

## 設定方法

### 前提条件

1. AWS Clean Rooms のコラボレーションが設定されていること
2. 分析テンプレートまたは アドホック SQL クエリを実行する権限があること
3. テーブルのサイズとデータ分散を理解していること

### 手順

#### ステップ 1: テーブルサイズの確認

join ヒントを適用する前に、テーブルのサイズを確認します。

```sql
-- テーブルの行数を確認
SELECT COUNT(*) FROM large_table;
SELECT COUNT(*) FROM small_table;
```

これにより、どのテーブルをブロードキャストするかを決定できます。

#### ステップ 2: ヒントなしのクエリ実行

まず、ヒントなしでクエリを実行し、ベースラインのパフォーマンスを確認します。

```sql
SELECT l.id, s.name, l.value
FROM large_table l
JOIN small_table s ON l.small_id = s.id;
```

#### ステップ 3: ヒントの適用とパフォーマンス比較

broadcast join ヒントを適用してクエリを実行し、パフォーマンスを比較します。

```sql
SELECT /*+ BROADCAST(small_table) */
  l.id, s.name, l.value
FROM large_table l
JOIN small_table s ON l.small_id = s.id;
```

クエリの実行時間とコストを確認し、ヒントの効果を評価します。

## メリット

### ビジネス面

- **コスト削減**: クエリパフォーマンスを最適化することで、計算リソースの使用量を削減し、コストを削減できます
- **分析速度の向上**: クエリ実行時間を短縮し、ビジネスインサイトをより迅速に取得できます
- **スケーラビリティ**: 大規模データセットでも効率的にクエリを実行できます

### 技術面

- **join 戦略の制御**: broadcast join ヒントを使用して、最適な join 戦略を選択できます
- **データ分散の最適化**: partition ヒントを使用して、並列処理のためのデータ分散を改善できます
- **柔軟性**: 事前承認された分析テンプレートおよび アドホック SQL クエリの両方でヒントを使用できます

## デメリット・制約事項

### 制限事項

- ヒントは提案であり、リソース制約や実行条件に基づいて無視される場合があります
- Differential Privacy SQL クエリまたは PySpark ジョブではサポートされていません
- 数値パラメータは 1 から 2147483647 の正の整数である必要があります
- 科学的表記法はサポートされていません

### 考慮すべき点

- ヒントを適用する前に、テーブルのサイズとデータ分散を理解してください
- ヒントが常に期待通りの結果をもたらすとは限らないため、パフォーマンスを測定して検証してください
- 不適切なヒントの使用は、逆にパフォーマンスを低下させる可能性があります

## ユースケース

### ユースケース 1: ルックアップテーブルとの join 最適化

**シナリオ**: 測定会社が、大規模なイベント視聴データと小規模な世帯ルックアップテーブルを join して、ライブスポーツイベントを視聴した世帯数を分析する。

**実装例**:
```sql
SELECT /*+ BROADCAST(household_lookup) */
  e.event_id,
  h.household_name,
  COUNT(*) as view_count
FROM event_views e
JOIN household_lookup h ON e.household_id = h.household_id
WHERE e.event_date = '2026-01-21'
GROUP BY e.event_id, h.household_name;
```

**効果**: ルックアップテーブルをブロードキャストすることで、join のパフォーマンスが向上し、クエリ実行時間とコストが削減されます。

### ユースケース 2: データ分散の改善

**シナリオ**: データが不均等に分散されており、一部のパーティションに負荷が集中している。

**実装例**:
```sql
SELECT /*+ REPARTITION(customer_id, region) */
  customer_id,
  region,
  SUM(sales_amount) as total_sales
FROM sales
GROUP BY customer_id, region;
```

**効果**: データを customer_id と region で再分割することで、より均等な分散を実現し、並列処理のパフォーマンスが向上します。

### ユースケース 3: 複数テーブルの join 最適化

**シナリオ**: ファクトテーブルと複数のディメンションテーブルを join する際、複数のディメンションテーブルをブロードキャストする。

**実装例**:
```sql
SELECT /*+ BROADCAST(dim_product, dim_store, dim_time) */
  f.sale_id,
  p.product_name,
  s.store_name,
  t.date,
  f.sale_amount
FROM fact_sales f
JOIN dim_product p ON f.product_id = p.product_id
JOIN dim_store s ON f.store_id = s.store_id
JOIN dim_time t ON f.time_id = t.time_id
WHERE t.year = 2026;
```

**効果**: 複数のディメンションテーブルをブロードキャストすることで、複数の join を効率的に実行できます。

## 料金

この機能自体に追加料金はかかりません。ただし、AWS Clean Rooms のクエリ実行に対しては通常の AWS Clean Rooms の料金が適用されます。

詳細については、[AWS Clean Rooms の料金](https://aws.amazon.com/clean-rooms/pricing/) を参照してください。

## 利用可能リージョン

この機能は、AWS Clean Rooms をサポートするすべての AWS リージョンで利用可能です。詳細については、[AWS Regions](https://docs.aws.amazon.com/general/latest/gr/clean-rooms.html#clean-rooms_region) を参照してください。

## 関連サービス・機能

- **AWS Clean Rooms**: セキュアなデータコラボレーションと分析を可能にするサービス
- **Amazon Athena**: S3 のデータに対して SQL クエリを実行するサーバーレスクエリサービス
- **AWS Glue**: データの準備と変換を行う ETL サービス

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/aws-clean-rooms-join-partition-hints-sql/)
- [Hints ドキュメント](https://docs.aws.amazon.com/clean-rooms/latest/sql-reference/sql-commands-hints-spark.html)
- [Join hints ドキュメント](https://docs.aws.amazon.com/clean-rooms/latest/sql-reference/join-hints.html)
- [Partitioning hints ドキュメント](https://docs.aws.amazon.com/clean-rooms/latest/sql-reference/partitioning-hints.html)
- [AWS Clean Rooms](https://aws.amazon.com/clean-rooms/)

## まとめ

AWS Clean Rooms の join および partition ヒント機能により、SQL クエリのパフォーマンスを最適化し、コストを削減できるようになりました。broadcast join ヒントを使用して大規模テーブルの join を効率化し、partition ヒントを使用してデータ分散を改善することで、より高速で効率的なデータ分析が可能になります。AWS Clean Rooms を使用してデータコラボレーションを行っている組織は、この機能を活用してクエリパフォーマンスとコスト効率を向上させることをお勧めします。
