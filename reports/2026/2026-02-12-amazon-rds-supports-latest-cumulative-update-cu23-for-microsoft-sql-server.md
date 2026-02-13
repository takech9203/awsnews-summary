# Amazon RDS for SQL Server - 最新累積更新プログラム CU23 のサポート

**リリース日**: 2026 年 2 月 12 日
**サービス**: Amazon RDS for SQL Server
**機能**: SQL Server 2022 CU23 (KB5078297) のサポート

📊 [このアップデートのインフォグラフィックを見る](https://takech9203.github.io/20260212-amazon-rds-supports-latest-cumulative-update-cu23-for-microsoft-sql-server.html)

## 概要

Amazon RDS for SQL Server が、Microsoft SQL Server 2022 の最新累積更新プログラム CU23 (KB5078297) のサポートを開始しました。この累積更新プログラムには、Microsoft が提供するセキュリティ修正とバグ修正が含まれています。

AWS は、Amazon RDS Management Console、AWS SDK、または CLI を使用して、データベースインスタンスをアップグレードし、CU23 を適用することを推奨しています。

**アップデート前の課題**

- SQL Server 2022 の最新のセキュリティ修正とバグ修正が適用されていなかった
- CU22 以前のバージョンに含まれる既知の問題が残存していた

**アップデート後の改善**

- CU23 (KB5078297) のセキュリティ修正とバグ修正が適用可能になった
- Microsoft が推奨する最新の安定バージョンにアップグレード可能になった
- AWS Management Console、SDK、CLI から簡単にアップグレードできる

## サービスアップデートの詳細

### 主要機能

1. **SQL Server 2022 CU23 のサポート**
   - 累積更新プログラム CU23 (KB5078297) を RDS for SQL Server で利用可能
   - Microsoft が提供するセキュリティ修正を含む
   - パフォーマンス改善とバグ修正を含む

2. **簡単なアップグレードプロセス**
   - AWS Management Console から数クリックでアップグレード可能
   - AWS CLI/SDK を使用した自動化されたアップグレードをサポート
   - メンテナンスウィンドウ中の自動アップグレード設定にも対応

## 技術仕様

### サポートされるバージョン

| SQL Server バージョン | CU | KB 番号 |
|----------------------|-----|---------|
| SQL Server 2022 | CU23 | KB5078297 |

### アップグレード方法

```bash
# AWS CLI を使用した RDS インスタンスのアップグレード
aws rds modify-db-instance \
  --db-instance-identifier mydbinstance \
  --engine-version <CU23対応RDSバージョン> \
  --apply-immediately
```

## 設定方法

### 前提条件

1. Amazon RDS for SQL Server 2022 のインスタンスが稼働している
2. AWS Management Console へのアクセス権限、または AWS CLI/SDK の設定
3. アップグレード前のバックアップ (推奨)

### 手順

#### ステップ 1: バックアップの作成

```bash
# 手動スナップショットを作成
aws rds create-db-snapshot \
  --db-instance-identifier mydbinstance \
  --db-snapshot-identifier mydbinstance-pre-cu23-snapshot
```

アップグレード前に、データベースの手動スナップショットを作成します。万が一の場合に備えたロールバック手段を確保します。

#### ステップ 2: AWS Management Console からアップグレード

1. AWS Management Console を開き、Amazon RDS サービスに移動
2. ナビゲーションペインで「データベース」を選択
3. アップグレードするデータベースインスタンスを選択
4. 「変更」ボタンをクリック
5. 「DB エンジンのバージョン」セクションで、CU23 対応バージョンを選択
6. 「すぐに適用」または「次のメンテナンスウィンドウ中に適用」を選択
7. 「データベースの変更」をクリック

AWS Management Console を使用して、数クリックでアップグレードを実行します。

#### ステップ 3: アップグレードの確認

```bash
# アップグレードの進行状況を確認
aws rds describe-db-instances \
  --db-instance-identifier mydbinstance \
  --query 'DBInstances[0].[EngineVersion,DBInstanceStatus]'
```

アップグレードが完了したら、データベースに接続してバージョンを確認し、アプリケーションが正常に動作することをテストします。

## メリット

### ビジネス面

- **セキュリティリスクの軽減**: 最新のセキュリティ修正を適用し、潜在的な脆弱性を修正
- **コンプライアンスの維持**: 最新パッチの適用により、業界標準とコンプライアンス要件を満たす
- **システムの安定性向上**: バグ修正により、予期しない障害のリスクを低減

### 技術面

- **パフォーマンス改善**: CU23 に含まれるパフォーマンス最適化を利用可能
- **バグ修正**: 既知のバグが修正され、SQL Server の安定性が向上
- **簡単なアップグレード**: AWS Management Console または CLI から数ステップでアップグレード可能

## デメリット・制約事項

### 制限事項

- アップグレード中は、データベースインスタンスが一時的に利用不可になる
- ダウングレードは直接サポートされていない (スナップショットからの復元が必要)
- SQL Server 2022 のみが対象

### 考慮すべき点

- アップグレード前に、テスト環境で互換性を確認することを推奨
- ビジネスに影響の少ない時間帯にアップグレードを計画
- アップグレード後にクエリプランが変更される可能性があるため、パフォーマンスの監視を推奨

## ユースケース

### ユースケース 1: 本番環境のセキュリティ強化

**シナリオ**: 本番環境の Amazon RDS for SQL Server 2022 インスタンスに最新のセキュリティ修正を適用したい。

**実装例**:
```bash
# メンテナンスウィンドウ中にアップグレードを予約
aws rds modify-db-instance \
  --db-instance-identifier prod-sqlserver \
  --engine-version <CU23対応RDSバージョン> \
  --no-apply-immediately
```

**効果**: メンテナンスウィンドウ中にセキュリティ修正が適用され、ビジネスへの影響を最小限に抑えながらセキュリティを向上させます。

### ユースケース 2: 開発環境での事前検証

**シナリオ**: CU23 のアップグレードを本番環境に適用する前に、開発環境で互換性を検証したい。

**実装例**:
```bash
# 開発環境を即座にアップグレード
aws rds modify-db-instance \
  --db-instance-identifier dev-sqlserver \
  --engine-version <CU23対応RDSバージョン> \
  --apply-immediately
```

**効果**: 開発環境で事前にアップグレードを検証し、アプリケーションの互換性やパフォーマンスへの影響を本番適用前に確認できます。

## 料金

SQL Server のバージョンアップグレード自体に追加料金はかかりません。Amazon RDS for SQL Server の標準的な料金体系が適用されます。

| 項目 | 説明 |
|------|------|
| インスタンス時間 | DB インスタンスの稼働時間に基づいて課金 |
| ストレージ | プロビジョニングされたストレージ容量に基づいて課金 |
| バックアップ | 自動バックアップと手動スナップショットのストレージに対して課金 |

## 利用可能リージョン

Amazon RDS for SQL Server が利用可能なすべての AWS リージョンで提供されています。

## 関連サービス・機能

- **Amazon RDS Automated Backups**: アップグレード前のバックアップを自動作成
- **Amazon RDS Performance Insights**: アップグレード後のパフォーマンスを監視
- **Amazon CloudWatch**: RDS インスタンスのメトリクスを監視し、アップグレード後の動作を確認

## 参考リンク

- 📊 [インフォグラフィック](https://takech9203.github.io/20260212-amazon-rds-supports-latest-cumulative-update-cu23-for-microsoft-sql-server.html)
- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-rds-latest-cumulative-update-cu23-microsoft-sql-server/)
- [ドキュメント: Upgrading RDS for SQL Server](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.SQLServer.html)
- [Microsoft KB5078297 - SQL Server 2022 CU23](https://learn.microsoft.com/en-us/troubleshoot/sql/releases/sqlserver-2022/cumulativeupdate23)
- [料金ページ](https://aws.amazon.com/rds/sqlserver/pricing/)

## まとめ

Amazon RDS for SQL Server が SQL Server 2022 の最新累積更新プログラム CU23 (KB5078297) のサポートを開始しました。このアップデートには Microsoft が提供するセキュリティ修正とバグ修正が含まれており、すべての RDS for SQL Server 2022 ユーザーにアップグレードが推奨されます。AWS Management Console、SDK、CLI から簡単にアップグレードでき、本番環境への適用前にテスト環境での検証を行うことを推奨します。
