# Amazon RDS / Aurora - スナップショット復元時のバックアップ設定

**リリース日**: 2026 年 2 月 13 日
**サービス**: Amazon RDS / Amazon Aurora
**機能**: Backup Configuration When Restoring Snapshots

📊 [このアップデートのインフォグラフィックを見る](https://takech9203.github.io/20260213-rds-aurora-backup-configuration-restoring-snapshots.html)

## 概要

Amazon RDS と Amazon Aurora が、スナップショットからの復元時にバックアップ保持期間と優先バックアップウィンドウの表示および変更をサポートしました。これまでは、復元されたインスタンスはスナップショットメタデータからバックアップパラメータを継承し、復元完了後にのみ変更可能でした。

このアップデートにより、復元を開始する前にバックアップ設定を確認でき、復元プロセス中にバックアップ保持期間と優先バックアップウィンドウを指定・変更できるようになりました。すべての RDS エンジン (MySQL、PostgreSQL、MariaDB、Oracle、SQL Server、Db2) と Aurora (MySQL/PostgreSQL) で利用可能です。

**アップデート前の課題**

- スナップショットから復元されたインスタンスは、スナップショットメタデータのバックアップ設定を自動的に継承していた
- バックアップ保持期間や優先バックアップウィンドウの変更は、復元完了後にのみ可能だった
- 復元前にバックアップ設定を確認する手段がなかった

**アップデート後の改善**

- 復元を開始する前に、スナップショットのバックアップ設定を確認可能になった
- 復元プロセス中にバックアップ保持期間を指定・変更できるようになった
- 復元プロセス中に優先バックアップウィンドウを指定・変更できるようになった
- 復元後の追加の変更操作が不要になり、運用手順が簡素化された

## サービスアップデートの詳細

### 主要機能

1. **復元前のバックアップ設定確認**
   - スナップショットからの復元を開始する前に、現在のバックアップ設定を表示
   - バックアップ保持期間と優先バックアップウィンドウの現在値を確認可能
   - コンソール、CLI、SDK のすべてで利用可能

2. **復元時のバックアップ保持期間の指定**
   - スナップショット復元時に、バックアップ保持期間を直接指定可能
   - 0 日 (自動バックアップ無効) から 35 日まで設定可能
   - スナップショットの元の設定を上書き可能

3. **復元時の優先バックアップウィンドウの指定**
   - スナップショット復元時に、優先バックアップウィンドウを直接設定可能
   - ビジネスに影響の少ない時間帯を復元時に指定可能
   - UTC 形式で hh:mm-hh:mm の形式で指定

## 技術仕様

### 対応エンジン

| エンジン | サポート状況 |
|----------|------------|
| Amazon RDS for MySQL | 対応 |
| Amazon RDS for PostgreSQL | 対応 |
| Amazon RDS for MariaDB | 対応 |
| Amazon RDS for Oracle | 対応 |
| Amazon RDS for SQL Server | 対応 |
| Amazon RDS for Db2 | 対応 |
| Amazon Aurora MySQL | 対応 |
| Amazon Aurora PostgreSQL | 対応 |

### バックアップ設定パラメータ

| パラメータ | 説明 | 値の範囲 |
|-----------|------|---------|
| BackupRetentionPeriod | 自動バックアップの保持日数 | 0-35 日 |
| PreferredBackupWindow | 優先バックアップウィンドウ | hh:mm-hh:mm (UTC) |

### API 変更履歴

| 日付 | サービス | 変更内容 |
|------|----------|----------|
| 2026/02/10 | [Amazon RDS](https://awsapichanges.com/archive/changes/56b6d8-rds.html) | 19 updated methods - RDS と Aurora の復元時にバックアップ保持期間と優先バックアップウィンドウを設定可能にするバックアップ設定を追加 |

## 設定方法

### 前提条件

1. Amazon RDS または Aurora のスナップショットが存在する
2. スナップショットからの復元に必要な IAM 権限が設定されている
3. AWS CLI v2 または AWS SDK の最新バージョン

### 手順

#### ステップ 1: コンソールからの復元時にバックアップ設定を指定

1. AWS Management Console を開き、Amazon RDS サービスに移動
2. ナビゲーションペインで「スナップショット」を選択
3. 復元するスナップショットを選択し、「スナップショットの復元」をクリック
4. 「バックアップ」セクションでバックアップ保持期間と優先バックアップウィンドウを確認・変更
5. 必要に応じて値を変更し、「DB インスタンスの復元」をクリック

復元時にバックアップ設定を直接指定することで、復元後の追加の変更操作が不要になります。

#### ステップ 2: CLI からの復元時にバックアップ設定を指定

```bash
# RDS スナップショットからの復元時にバックアップ設定を指定
aws rds restore-db-instance-from-db-snapshot \
  --db-instance-identifier restored-instance \
  --db-snapshot-identifier my-snapshot \
  --backup-retention-period 7 \
  --preferred-backup-window "03:00-04:00"
```

CLI を使用してスナップショットから復元する際に、`--backup-retention-period` と `--preferred-backup-window` パラメータを指定して、バックアップ設定を直接設定します。

#### ステップ 3: Aurora クラスタースナップショットからの復元

```bash
# Aurora クラスタースナップショットからの復元時にバックアップ設定を指定
aws rds restore-db-cluster-from-snapshot \
  --db-cluster-identifier restored-cluster \
  --snapshot-identifier my-aurora-snapshot \
  --engine aurora-mysql \
  --backup-retention-period 14 \
  --preferred-backup-window "02:00-03:00"
```

Aurora クラスタースナップショットからの復元時にも、同様にバックアップ保持期間と優先バックアップウィンドウを指定できます。

## メリット

### ビジネス面

- **運用効率の向上**: 復元後の追加設定変更が不要になり、運用手順が簡素化
- **ダウンタイムの削減**: 復元時にバックアップ設定を完了させることで、追加の変更に伴うダウンタイムを回避
- **コンプライアンスの維持**: 復元直後から適切なバックアップポリシーを適用でき、コンプライアンス要件を確実に満たす

### 技術面

- **ワンステップでの設定完了**: 復元とバックアップ設定を単一のオペレーションで実行
- **自動化の簡素化**: CI/CD パイプラインやインフラストラクチャ自動化スクリプトにおいて、復元時にバックアップ設定を含めることが可能
- **全エンジン対応**: RDS の全エンジンと Aurora MySQL/PostgreSQL で一貫したエクスペリエンスを提供

## デメリット・制約事項

### 制限事項

- バックアップ保持期間は 0 日から 35 日の範囲に制限される
- 優先バックアップウィンドウは最低 30 分の時間幅が必要
- この機能はスナップショットからの復元時のみ利用可能 (ポイントインタイムリカバリには別途設定が必要)

### 考慮すべき点

- 既存の自動化スクリプトがある場合、新しいパラメータの追加を検討
- バックアップ保持期間を 0 に設定すると自動バックアップが無効になるため、本番環境では注意が必要
- 復元時に指定しない場合は、従来どおりスナップショットメタデータの設定が継承される

## ユースケース

### ユースケース 1: ディザスタリカバリの自動化

**シナリオ**: DR 手順の一環として、スナップショットからの復元時に本番環境と同じバックアップポリシーを自動適用したい。

**実装例**:
```bash
# DR 復元スクリプト
aws rds restore-db-instance-from-db-snapshot \
  --db-instance-identifier prod-dr-restored \
  --db-snapshot-identifier prod-latest-snapshot \
  --backup-retention-period 14 \
  --preferred-backup-window "03:00-04:00" \
  --db-instance-class db.r6g.xlarge \
  --multi-az
```

**効果**: 復元直後から本番環境と同一のバックアップポリシーが適用され、DR 手順の追加ステップが不要になります。

### ユースケース 2: テスト環境の迅速なプロビジョニング

**シナリオ**: 本番スナップショットからテスト環境を復元する際、テスト用のバックアップ設定を適用したい。

**実装例**:
```bash
# テスト環境の復元 (バックアップ保持期間を短く設定)
aws rds restore-db-instance-from-db-snapshot \
  --db-instance-identifier test-env-db \
  --db-snapshot-identifier prod-snapshot \
  --backup-retention-period 1 \
  --preferred-backup-window "06:00-07:00"
```

**効果**: テスト環境に適したバックアップ設定 (短い保持期間) を復元時に直接適用でき、追加の変更操作が不要です。

### ユースケース 3: コンプライアンス要件への準拠

**シナリオ**: 規制要件により、データベースの復元直後から特定のバックアップ保持期間を維持する必要がある。

**実装例**:
```bash
# コンプライアンス要件に準拠した復元
aws rds restore-db-instance-from-db-snapshot \
  --db-instance-identifier compliance-db \
  --db-snapshot-identifier quarterly-snapshot \
  --backup-retention-period 35 \
  --preferred-backup-window "01:00-02:00"
```

**効果**: 復元直後からコンプライアンスで求められるバックアップ保持期間が適用され、一時的なポリシー違反状態を回避できます。

## 料金

この機能自体に追加料金は発生しません。Amazon RDS と Aurora の標準的なバックアップストレージ料金が適用されます。

| 項目 | 説明 |
|------|------|
| バックアップストレージ | プロビジョニングされたデータベースストレージの 100% までは無料。超過分は GB あたりの月額料金 |
| スナップショットストレージ | 手動スナップショットのストレージに対して GB あたりの月額料金 |

コンソール、CLI、SDK のいずれから利用しても追加コストはかかりません。

## 利用可能リージョン

すべての AWS 商用リージョンおよび AWS GovCloud (US) リージョンで利用可能です。

## 関連サービス・機能

- **Amazon RDS Automated Backups**: 自動バックアップの管理とバックアップ保持期間の設定
- **Amazon Aurora Backtrack**: Aurora MySQL でのデータベースの巻き戻し機能
- **AWS Backup**: 複数の AWS サービスにまたがるバックアップの一元管理
- **Amazon RDS Point-in-Time Recovery**: 特定の時点へのデータベース復元

## 参考リンク

- 📊 [インフォグラフィック](https://takech9203.github.io/20260213-rds-aurora-backup-configuration-restoring-snapshots.html)
- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-rds-backup-configuration-restoring-snapshots/)
- [ドキュメント: Backing up, restoring, and exporting data](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_CommonTasks.BackupRestore.html)
- [ドキュメント: Aurora Backup and Restore](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/BackupRestoreAurora.html)
- [API 変更履歴](https://awsapichanges.com/archive/changes/56b6d8-rds.html)
- [料金ページ](https://aws.amazon.com/rds/pricing/)

## まとめ

Amazon RDS と Aurora のスナップショット復元時にバックアップ保持期間と優先バックアップウィンドウを直接設定できるようになりました。これにより、復元後の追加設定変更が不要になり、運用手順が簡素化されます。すべての RDS エンジンと Aurora MySQL/PostgreSQL で利用可能で、コンソール、CLI、SDK から追加コストなしで利用できます。ディザスタリカバリの自動化やコンプライアンス要件への準拠において、特に効果を発揮するアップデートです。
