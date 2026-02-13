# Amazon RDS for SQL Server - 最新 CU と GDR アップデート

**リリース日**: 2026年01月15日
**サービス**: Amazon RDS for SQL Server / Amazon RDS Custom for SQL Server
**機能**: 累積更新プログラム (CU) と一般配布リリース (GDR) のセキュリティアップデート

## 概要

Amazon RDS for SQL Server と Amazon RDS Custom for SQL Server は、Microsoft SQL Server の最新の一般配布リリース (GDR) と累積更新プログラム (CU) のサポートを開始しました。このリリースには、重大なセキュリティ脆弱性 CVE-2025-59499 に対処する GDR アップデートが含まれています。

**Amazon RDS for SQL Server のサポート対象:**
- SQL Server 2016 SP3+GDR KB5068401 (RDS version 13.00.6475.1.v1)
- SQL Server 2017 CU31+GDR KB5068402 (RDS version 14.00.3515.1.v1)
- SQL Server 2019 CU32+GDR KB5068404 (RDS version 15.00.4455.2.1.v1)
- SQL Server 2022 CU22 KB5068450 (RDS version 16.00.4225.2.1.v1)

**Amazon RDS Custom for SQL Server のサポート対象:**
- SQL Server 2019 CU32+GDR KB5068404 (RDS version 15.00.4455.2.1.v1)
- SQL Server 2022 CU21+GDR KB5068406 (RDS version 16.00.4222.2.1.v1)

AWS は、Amazon RDS Management Console、AWS SDK、または CLI を使用して、データベースインスタンスをアップグレードし、これらのアップデートを適用することを推奨しています。

**アップデート前の課題**

- セキュリティ脆弱性 CVE-2025-59499 が存在し、SQL Server インスタンスが攻撃にさらされるリスクがあった
- 最新の累積更新プログラムが適用されておらず、バグ修正や機能改善を利用できなかった
- 手動でセキュリティパッチを適用する必要があり、運用負荷が高かった

**アップデート後の改善**

- CVE-2025-59499 のセキュリティ脆弱性が修正され、SQL Server インスタンスのセキュリティが向上
- 最新の累積更新プログラムにより、バグ修正、パフォーマンス改善、機能強化を利用可能
- AWS Management Console、AWS SDK、CLI から簡単にアップグレードでき、運用負荷を削減
- RDS Custom では、OS レベルのカスタマイズを維持しながら、セキュリティアップデートを適用可能

## サービスアップデートの詳細

### 主要機能

1. **セキュリティ脆弱性の修正**
   - CVE-2025-59499 の脆弱性に対処
   - Microsoft のセキュリティ勧告に従った GDR アップデート
   - 既知の攻撃ベクトルから SQL Server インスタンスを保護

2. **累積更新プログラム (CU) のサポート**
   - SQL Server 2017 CU31、SQL Server 2019 CU32、SQL Server 2022 CU21/CU22
   - バグ修正、パフォーマンス改善、機能強化を含む
   - Microsoft の推奨に基づいた最新の安定バージョン

3. **簡単なアップグレードプロセス**
   - AWS Management Console から数クリックでアップグレード
   - AWS CLI/SDK を使用した自動化されたアップグレード
   - メンテナンスウィンドウ中の自動アップグレード設定も可能

4. **RDS Custom でのサポート**
   - OS レベルのカスタマイズを維持しながら、DB エンジンをアップグレード
   - 管理者権限での詳細な制御を保持
   - レガシーアプリケーションとの互換性を維持

## 技術仕様

### サポートされる SQL Server バージョン

#### Amazon RDS for SQL Server

| SQL Server バージョン | CU/GDR | KB 番号 | RDS バージョン |
|----------------------|--------|---------|---------------|
| SQL Server 2016 SP3 | SP3+GDR | KB5068401 | 13.00.6475.1.v1 |
| SQL Server 2017 | CU31+GDR | KB5068402 | 14.00.3515.1.v1 |
| SQL Server 2019 | CU32+GDR | KB5068404 | 15.00.4455.2.1.v1 |
| SQL Server 2022 | CU22 | KB5068450 | 16.00.4225.2.1.v1 |

#### Amazon RDS Custom for SQL Server

| SQL Server バージョン | CU/GDR | KB 番号 | RDS バージョン |
|----------------------|--------|---------|---------------|
| SQL Server 2019 | CU32+GDR | KB5068404 | 15.00.4455.2.1.v1 |
| SQL Server 2022 | CU21+GDR | KB5068406 | 16.00.4222.2.1.v1 |

### セキュリティ脆弱性 CVE-2025-59499

**深刻度**: 高 (High)
**影響**: SQL Server の特定の機能において、権限昇格やサービス拒否攻撃のリスク
**対策**: GDR アップデートを適用してパッチを適用

詳細については、Microsoft のセキュリティアドバイザリと KB 記事を参照してください。

### アップグレード方法

```bash
# AWS CLI を使用した RDS インスタンスのアップグレード
aws rds modify-db-instance \
  --db-instance-identifier mydbinstance \
  --engine-version 16.00.4225.2.1.v1 \
  --apply-immediately

# RDS Custom インスタンスのアップグレード
aws rds modify-db-instance \
  --db-instance-identifier mycustominstance \
  --engine-version 15.00.4455.2.1.v1 \
  --apply-immediately
```

## 設定方法

### 前提条件

1. Amazon RDS for SQL Server または RDS Custom for SQL Server のインスタンスが稼働している
2. AWS Management Console へのアクセス権限、または AWS CLI/SDK の設定
3. アップグレード前のバックアップ (推奨)

### 手順

#### ステップ1: バックアップの作成

```bash
# 手動スナップショットを作成
aws rds create-db-snapshot \
  --db-instance-identifier mydbinstance \
  --db-snapshot-identifier mydbinstance-pre-upgrade-snapshot
```

アップグレード前に、データベースの手動スナップショットを作成して、万が一の場合に備えます。

#### ステップ2: AWS Management Console からアップグレード

1. AWS Management Console を開き、Amazon RDS サービスに移動
2. ナビゲーションペインで「データベース」を選択
3. アップグレードするデータベースインスタンスを選択
4. 「変更」ボタンをクリック
5. 「DB エンジンのバージョン」セクションで、新しいバージョンを選択
   - SQL Server 2022 の場合: `16.00.4225.2.1.v1`
   - SQL Server 2019 の場合: `15.00.4455.2.1.v1`
6. 「すぐに適用」または「次のメンテナンスウィンドウ中に適用」を選択
7. 「データベースの変更」をクリック

#### ステップ3: アップグレードの確認

```bash
# アップグレードの進行状況を確認
aws rds describe-db-instances \
  --db-instance-identifier mydbinstance \
  --query 'DBInstances[0].[EngineVersion,DBInstanceStatus]'

# 接続してバージョンを確認
sqlcmd -S mydbinstance.xxxxxx.region.rds.amazonaws.com -U admin -P password -Q "SELECT @@VERSION"
```

アップグレードが完了したら、データベースに接続してバージョンを確認し、アプリケーションが正常に動作することをテストします。

#### ステップ4: アプリケーションのテスト

```sql
-- アプリケーションの主要な機能をテスト
-- 例: クエリのパフォーマンス、接続性、データ整合性など
SELECT TOP 10 * FROM YourTable;

-- エラーログを確認
EXEC sp_readerrorlog;
```

アップグレード後、アプリケーションの主要な機能をテストし、エラーログを確認して、問題がないことを確認します。

## メリット

### ビジネス面

- **セキュリティリスクの軽減**: CVE-2025-59499 の脆弱性を修正し、データ侵害のリスクを削減
- **コンプライアンスの維持**: 最新のセキュリティパッチを適用することで、業界標準とコンプライアンス要件を満たす
- **ビジネス継続性**: 安定した最新バージョンにより、予期しないダウンタイムを防止

### 技術面

- **パフォーマンス改善**: 累積更新プログラムに含まれるパフォーマンス最適化を利用
- **バグ修正**: 既知のバグが修正され、システムの安定性が向上
- **簡単な管理**: AWS Management Console または CLI から簡単にアップグレード
- **RDS Custom の柔軟性**: OS レベルのカスタマイズを維持しながら、DB エンジンをアップグレード

## デメリット・制約事項

### 制限事項

- アップグレード中は、データベースインスタンスが一時的に利用不可になる (数分から数十分)
- 一部の互換性の問題が発生する可能性があるため、アップグレード前のテストが推奨される
- ダウングレードは直接サポートされていない (スナップショットからの復元が必要)

### 考慮すべき点

- アップグレード前に、テスト環境で互換性を確認することを強く推奨
- ビジネスに影響の少ない時間帯 (メンテナンスウィンドウ) にアップグレードを計画
- アップグレード後、クエリプランが変更される可能性があるため、パフォーマンスを監視
- RDS Custom の場合、OS レベルのカスタマイズが DB エンジンのアップグレードに影響を与えないか確認

## ユースケース

### ユースケース1: プロダクション環境のセキュリティ強化

**シナリオ**: プロダクション環境で Amazon RDS for SQL Server を使用しており、CVE-2025-59499 のセキュリティ脆弱性に対処する必要がある。

**実装例**:
```bash
# 1. 手動スナップショットを作成
aws rds create-db-snapshot \
  --db-instance-identifier prod-sqlserver \
  --db-snapshot-identifier prod-sqlserver-pre-upgrade

# 2. メンテナンスウィンドウ中にアップグレード
aws rds modify-db-instance \
  --db-instance-identifier prod-sqlserver \
  --engine-version 16.00.4225.2.1.v1 \
  --no-apply-immediately
```

**効果**: メンテナンスウィンドウ中にセキュリティパッチが自動的に適用され、ビジネスへの影響を最小限に抑えながら、セキュリティリスクを軽減します。

### ユースケース2: RDS Custom でのレガシーアプリケーション対応

**シナリオ**: RDS Custom for SQL Server を使用しており、OS レベルでカスタマイズを行っているレガシーアプリケーションがある。セキュリティアップデートを適用しつつ、カスタマイズを維持したい。

**実装例**:
```bash
# RDS Custom インスタンスをアップグレード
aws rds modify-db-instance \
  --db-instance-identifier custom-sqlserver \
  --engine-version 15.00.4455.2.1.v1 \
  --apply-immediately

# アップグレード後、OS レベルのカスタマイズを確認
# RDS Custom では、OS へのアクセスが可能
```

**効果**: OS レベルのカスタマイズ (特定のドライバ、レジストリ設定、サードパーティツールなど) を維持しながら、DB エンジンのセキュリティパッチを適用できます。レガシーアプリケーションの互換性を保ちつつ、セキュリティを強化します。

### ユースケース3: 開発・テスト環境の即時アップグレード

**シナリオ**: 開発・テスト環境で、最新の累積更新プログラムをテストし、プロダクション環境へのロールアウト前に互換性を確認したい。

**実装例**:
```bash
# 開発環境を即座にアップグレード
aws rds modify-db-instance \
  --db-instance-identifier dev-sqlserver \
  --engine-version 16.00.4225.2.1.v1 \
  --apply-immediately

# アプリケーションのテストスイートを実行
# 互換性の問題がないか確認
```

**効果**: 開発・テスト環境で最新バージョンをテストし、アプリケーションの互換性、パフォーマンス、クエリプランの変更を事前に確認できます。問題が発見された場合は、プロダクション環境へのロールアウトを延期し、修正を行うことができます。

## 料金

SQL Server のバージョンアップグレード自体に追加料金はかかりません。Amazon RDS for SQL Server と RDS Custom for SQL Server の標準的な料金体系が適用されます。

- **インスタンス時間**: 使用した DB インスタンスの時間数に基づいて課金
- **ストレージ**: プロビジョニングされたストレージ容量に基づいて課金
- **バックアップ**: 自動バックアップと手動スナップショットのストレージに対して課金
- **データ転送**: リージョン間のデータ転送に対して課金

### 料金例

| インスタンスタイプ | 月額料金 (概算、東京リージョン) |
|-------------------|------------------|
| db.m5.large (2 vCPU, 8 GiB) + 100 GB ストレージ | 約 $240 (インスタンス) + $23 (ストレージ) = **$263** |
| db.r5.xlarge (4 vCPU, 32 GiB) + 500 GB ストレージ | 約 $615 (インスタンス) + $115 (ストレージ) = **$730** |

*料金は変更される可能性があります。最新の料金については、[Amazon RDS 料金ページ](https://aws.amazon.com/rds/sqlserver/pricing/)を参照してください。*

## 利用可能リージョン

これらのアップデートは、Amazon RDS for SQL Server と RDS Custom for SQL Server が利用可能なすべての AWS リージョンで提供されています。

主要リージョン:
- 米国東部 (バージニア北部、オハイオ)
- 米国西部 (オレゴン、北カリフォルニア)
- 欧州 (アイルランド、フランクフルト、ロンドン)
- アジアパシフィック (東京、シンガポール、シドニー、ソウル)
- AWS GovCloud (US) リージョン

詳細なリージョン一覧については、[AWS グローバルインフラストラクチャ](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)を参照してください。

## 関連サービス・機能

- **Amazon RDS Automated Backups**: アップグレード前に自動バックアップを作成し、万が一の場合に復元
- **Amazon RDS Performance Insights**: アップグレード後のパフォーマンスを監視し、クエリプランの変更を確認
- **AWS Database Migration Service (DMS)**: 他のデータベースから RDS SQL Server へ移行する際、最新バージョンに直接移行可能
- **Amazon CloudWatch**: RDS インスタンスのメトリクスを監視し、アップグレード後の動作を確認

## 参考リンク

- [公式発表 (What's New) - Amazon RDS for SQL Server](https://aws.amazon.com/about-aws/whats-new/2026/01/amazon-rds-cu-gdr-updates-microsoft-sql-server/)
- [公式発表 (What's New) - Amazon RDS Custom for SQL Server](https://aws.amazon.com/about-aws/whats-new/2026/01/amazon-rds-custom-gdr-updates-microsoft-sql-server/)
- [ドキュメント: Upgrading RDS for SQL Server](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.SQLServer.html)
- [ドキュメント: Upgrading RDS Custom for SQL Server](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/custom-upgrading-sqlserver.html)
- [Microsoft KB5068401 (SQL Server 2016 SP3+GDR)](https://support.microsoft.com/help/5068401)
- [Microsoft KB5068402 (SQL Server 2017 CU31+GDR)](https://support.microsoft.com/help/5068402)
- [Microsoft KB5068404 (SQL Server 2019 CU32+GDR)](https://support.microsoft.com/help/5068404)
- [Microsoft KB5068450 (SQL Server 2022 CU22)](https://learn.microsoft.com/en-us/troubleshoot/sql/releases/sqlserver-2022/cumulativeupdate22)
- [Microsoft KB5068406 (SQL Server 2022 CU21+GDR)](https://support.microsoft.com/help/5068406)
- [料金ページ](https://aws.amazon.com/rds/sqlserver/pricing/)

## まとめ

Amazon RDS for SQL Server と RDS Custom for SQL Server の最新 CU と GDR アップデートは、重大なセキュリティ脆弱性 CVE-2025-59499 を修正し、バグ修正とパフォーマンス改善を提供する重要なリリースです。すべての RDS SQL Server ユーザーは、できるだけ早くこのアップデートを適用することを強くお勧めします。AWS Management Console または CLI から簡単にアップグレードでき、セキュリティリスクを軽減し、システムの安定性を向上させることができます。アップグレード前には、必ずバックアップを作成し、テスト環境で互換性を確認してください。
