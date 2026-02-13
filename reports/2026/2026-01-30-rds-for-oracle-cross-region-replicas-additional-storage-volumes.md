# Amazon RDS for Oracle - クロスリージョンレプリカへの追加ストレージボリューム対応

**リリース日**: 2026年1月30日
**サービス**: Amazon RDS for Oracle
**機能**: クロスリージョンレプリカでの追加ストレージボリュームサポート

## 概要

Amazon RDS for Oracle が、追加ストレージボリュームを持つクロスリージョンレプリカをサポート開始しました。追加ストレージボリューム機能により、プライマリストレージボリュームに加えて最大 3 つのストレージボリューム (各最大 64 TiB) を追加でき、合計最大 256 TiB のストレージを構成できます。この機能により、アプリケーションのダウンタイムなしに、ワークロードの需要の変化に応じてストレージを追加・削除する柔軟性が得られます。

クロスリージョンレプリカをビジネスクリティカルなアプリケーション向けに設定している顧客は、追加ストレージボリュームを使用してストレージの柔軟性を得られるようになりました。追加ストレージボリュームを持つデータベースインスタンスのクロスリージョンレプリカを作成すると、Amazon RDS for Oracle は自動的にレプリカに同じストレージレイアウトを構成します。

**アップデート前の課題**

- クロスリージョンレプリカでは追加ストレージボリュームがサポートされていなかった
- ビジネスクリティカルなアプリケーションでクロスリージョンレプリカと追加ストレージボリュームの両方を同時に使用できなかった
- プライマリインスタンスとレプリカで異なるストレージ構成になる可能性があった
- ディザスタリカバリ時にストレージ構成の整合性を維持することが困難だった

**アップデート後の改善**

- クロスリージョンレプリカでも追加ストレージボリュームを使用可能
- プライマリインスタンスとレプリカで同じストレージレイアウトを自動構成
- AWS Management Console、AWS CLI、AWS SDK を使用してプライマリとレプリカの追加ストレージボリュームを変更可能
- ディザスタリカバリ時にレプリカを新しいスタンドアロンデータベースとして昇格、またはスイッチオーバーでプライマリとレプリカの役割を逆転可能
- 低 RPO (Recovery Point Objective) および RTO (Recovery Time Objective) を実現

## サービスアップデートの詳細

### 主要機能

1. **クロスリージョンレプリカへの追加ストレージボリューム対応**
   - プライマリストレージボリュームに加えて最大 3 つのストレージボリューム追加可能
   - 各ストレージボリュームは最大 64 TiB
   - 合計最大 256 TiB のストレージを構成可能

2. **自動ストレージレイアウト構成**
   - クロスリージョンレプリカ作成時に、プライマリインスタンスと同じストレージレイアウトを自動構成
   - プライマリとレプリカで一貫したストレージ構成を維持

3. **ディザスタリカバリ対応**
   - レプリカを新しいスタンドアロンデータベースとして昇格可能
   - スイッチオーバーでプライマリとレプリカの役割を逆転可能
   - 低 RPO および RTO でビジネスクリティカルなアプリケーションをサポート

## 技術仕様

### ストレージ構成

| 項目 | 詳細 |
|------|------|
| プライマリストレージボリューム | 1 ボリューム (標準) |
| 追加ストレージボリューム | 最大 3 ボリューム |
| 各ボリュームの最大サイズ | 64 TiB |
| 合計最大ストレージ | 256 TiB |

### ライセンス要件

| レプリカモード | 必要なライセンス |
|--------------|----------------|
| マウントモード | Oracle Database Enterprise Edition (EE) |
| 読み取り専用モード | Oracle Database Enterprise Edition (EE) + Oracle Active Data Guard |

### 利用可能リージョン

すべての AWS リージョン (AWS GovCloud (US) リージョンを含む)

## 設定方法

### 前提条件

1. Amazon RDS for Oracle データベースインスタンス (追加ストレージボリューム構成済み)
2. 適切な Oracle ライセンス (Enterprise Edition)
3. クロスリージョンレプリカを作成するための IAM 権限

### 手順

#### ステップ 1: 追加ストレージボリュームを持つプライマリインスタンスの作成

```bash
# プライマリインスタンスに追加ストレージボリュームを追加
aws rds modify-db-instance \
    --db-instance-identifier my-primary-instance \
    --additional-storage-volumes '[
        {"StorageType": "gp3", "AllocatedStorage": 1000},
        {"StorageType": "gp3", "AllocatedStorage": 2000}
    ]' \
    --apply-immediately
```

#### ステップ 2: クロスリージョンレプリカの作成

```bash
# クロスリージョンレプリカを作成
aws rds create-db-instance-read-replica \
    --db-instance-identifier my-replica-instance \
    --source-db-instance-identifier arn:aws:rds:us-east-1:123456789012:db:my-primary-instance \
    --region us-west-2
```

Amazon RDS for Oracle は自動的にレプリカに同じストレージレイアウトを構成します。

#### ステップ 3: レプリカのストレージボリューム変更

```bash
# レプリカの追加ストレージボリュームを変更
aws rds modify-db-instance \
    --db-instance-identifier my-replica-instance \
    --additional-storage-volumes '[
        {"StorageType": "gp3", "AllocatedStorage": 1500},
        {"StorageType": "gp3", "AllocatedStorage": 2500}
    ]' \
    --apply-immediately \
    --region us-west-2
```

#### ステップ 4: ディザスタリカバリ時のレプリカ昇格

```bash
# レプリカを新しいスタンドアロンデータベースとして昇格
aws rds promote-read-replica \
    --db-instance-identifier my-replica-instance \
    --region us-west-2
```

## メリット

### ビジネス面

- **ストレージ柔軟性**: ワークロードの需要に応じてストレージを追加・削除可能
- **ダウンタイムなし**: アプリケーションのダウンタイムなしにストレージを変更可能
- **ディザスタリカバリ**: 低 RPO および RTO でビジネスクリティカルなアプリケーションをサポート

### 技術面

- **大容量ストレージ**: 合計最大 256 TiB のストレージを構成可能
- **自動構成**: レプリカ作成時にプライマリと同じストレージレイアウトを自動構成
- **一貫性**: プライマリとレプリカで一貫したストレージ構成を維持

## デメリット・制約事項

### 制限事項

- Oracle Database Enterprise Edition (EE) ライセンスが必要
- 読み取り専用モードでは Oracle Active Data Guard ライセンスが必要
- 追加ストレージボリュームは最大 3 つまで

### 考慮すべき点

- Oracle ライセンス要件を事前に確認する必要がある
- クロスリージョンレプリカのデータ転送料金が発生する
- レプリカのストレージ変更はプライマリとは独立して実行可能だが、一貫性を維持することが推奨される

## ユースケース

### ユースケース 1: ビジネスクリティカルなアプリケーションのディザスタリカバリ

**シナリオ**: 大規模なデータベースを持つビジネスクリティカルなアプリケーションで、ディザスタリカバリとストレージ柔軟性の両方が必要

**実装例**:
```bash
# 追加ストレージボリュームを持つプライマリインスタンスを作成
aws rds modify-db-instance \
    --db-instance-identifier my-primary-instance \
    --additional-storage-volumes '[
        {"StorageType": "gp3", "AllocatedStorage": 5000},
        {"StorageType": "gp3", "AllocatedStorage": 10000}
    ]'

# クロスリージョンレプリカを作成
aws rds create-db-instance-read-replica \
    --db-instance-identifier my-replica-instance \
    --source-db-instance-identifier arn:aws:rds:us-east-1:123456789012:db:my-primary-instance \
    --region us-west-2
```

**効果**: ディザスタリカバリ時に低 RPO および RTO を実現し、ストレージ柔軟性も維持

### ユースケース 2: ワークロード需要の変化に対応

**シナリオ**: データ量の増加に応じて、プライマリとレプリカのストレージを動的に調整する必要がある

**実装例**:
```bash
# プライマリインスタンスのストレージを追加
aws rds modify-db-instance \
    --db-instance-identifier my-primary-instance \
    --additional-storage-volumes '[
        {"StorageType": "gp3", "AllocatedStorage": 8000},
        {"StorageType": "gp3", "AllocatedStorage": 12000}
    ]'

# レプリカのストレージも同様に調整
aws rds modify-db-instance \
    --db-instance-identifier my-replica-instance \
    --additional-storage-volumes '[
        {"StorageType": "gp3", "AllocatedStorage": 8000},
        {"StorageType": "gp3", "AllocatedStorage": 12000}
    ]' \
    --region us-west-2
```

**効果**: アプリケーションのダウンタイムなしに、ワークロード需要に応じてストレージを動的に調整

## 料金

- **RDS インスタンス料金**: 標準的な RDS インスタンス料金
- **ストレージ料金**: 追加ストレージボリュームに対する標準的なストレージ料金
- **クロスリージョンデータ転送料金**: クロスリージョンレプリカのデータ転送に対する料金
- **Oracle ライセンス料金**: License Included または BYOL

### 料金例

| 使用量 | 月額料金 (概算) |
|--------|------------------|
| プライマリインスタンス + 追加ストレージ 50 TiB | $500-1,000 |
| クロスリージョンレプリカ + 追加ストレージ 50 TiB | $500-1,000 |
| クロスリージョンデータ転送 | 変動 |

## 利用可能リージョン

すべての AWS リージョン (AWS GovCloud (US) リージョンを含む)

## 関連サービス・機能

- **Amazon RDS Multi-AZ**: 同一リージョン内の高可用性
- **Oracle Active Data Guard**: 読み取り専用レプリカ機能
- **AWS Backup**: データベースバックアップの一元管理
- **Amazon CloudWatch**: データベースパフォーマンスの監視

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/rds-for-oracle-cross-region-replicas-additional-storage-volumes/)
- [Amazon RDS for Oracle ユーザーガイド](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/User_Oracle_AdditionalStorage.html)
- [Amazon RDS 料金ページ](https://aws.amazon.com/rds/oracle/pricing/)

## まとめ

Amazon RDS for Oracle がクロスリージョンレプリカで追加ストレージボリュームをサポートしたことにより、ビジネスクリティカルなアプリケーションでディザスタリカバリとストレージ柔軟性の両方を同時に実現できるようになりました。プライマリインスタンスとレプリカで一貫したストレージ構成を維持しながら、アプリケーションのダウンタイムなしにワークロード需要に応じてストレージを動的に調整できます。低 RPO および RTO を実現し、ビジネス継続性を確保できます。
