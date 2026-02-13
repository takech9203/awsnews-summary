# Amazon Aurora および RDS - 追加リージョンでの r8g、r7g、r7i インスタンス対応

**リリース日**: 2026 年 1 月 20 日
**サービス**: Amazon Aurora, Amazon RDS
**機能**: 追加 AWS リージョンでの r8g、r7g、r7i データベースインスタンスのサポート

## 概要

AWS Graviton4 ベースの R8g データベースインスタンスが、Amazon Aurora (MySQL および PostgreSQL 互換) および Amazon RDS for PostgreSQL、MySQL、MariaDB の追加のアジアパシフィックリージョン (香港、大阪、ジャカルタ) で一般提供されました。R8g インスタンスは、Amazon Aurora with MySQL compatibility および Amazon RDS for PostgreSQL、MySQL、MariaDB において、アジアパシフィック (ソウル、シンガポール) および Canada (Central) リージョンでもサポートされるようになりました。

さらに、Amazon Aurora with MySQL および PostgreSQL compatibility は、アジアパシフィック (ハイデラバード) で R7i データベースインスタンス、アフリカ (ケープタウン) で R7g データベースインスタンスをサポートするようになりました。AWS Graviton4 ベースのインスタンスは、同等サイズの Graviton3 ベースのインスタンスと比較して、データベースエンジン、バージョン、ワークロードに応じて、最大 40% のパフォーマンス向上と最大 29% の価格/パフォーマンス向上を提供します。

**アップデート前の課題**

- 特定のリージョンでは、最新の Graviton4 ベースの R8g インスタンスを使用できませんでした
- 一部のリージョンでは、R7g および R7i インスタンスの選択肢が限られていました
- 最新のハードウェアを使用したパフォーマンス向上とコスト効率の改善が制限されていました

**アップデート後の改善**

- 追加のアジアパシフィックリージョン (香港、大阪、ジャカルタ、ソウル、シンガポール) および Canada (Central) で R8g インスタンスを使用できるようになりました
- アジアパシフィック (ハイデラバード) で R7i インスタンス、アフリカ (ケープタウン) で R7g インスタンスを使用できるようになりました
- 最大 40% のパフォーマンス向上と最大 29% の価格/パフォーマンス向上を享受できるようになりました

## サービスアップデートの詳細

### 主要機能

1. **AWS Graviton4 ベースの R8g インスタンス**
   - 最大 192 vCPU (48xlarge サイズ)
   - 8:1 のメモリ対 vCPU 比率
   - 最新の DDR5 メモリ
   - 最大 50Gbps の強化されたネットワーク帯域幅
   - 最大 40Gbps の Amazon EBS への帯域幅

2. **パフォーマンス向上**
   - 同等サイズの Graviton3 ベースのインスタンスと比較して、最大 40% のパフォーマンス向上
   - 同等サイズの Graviton3 ベースのインスタンスと比較して、最大 29% の価格/パフォーマンス向上
   - データベースエンジン、バージョン、ワークロードに応じて異なります

3. **新しいインスタンスサイズ**
   - R8g: 24xlarge および 48xlarge サイズを導入
   - より大規模なワークロードに対応

## 技術仕様

### サポートされるインスタンスタイプとリージョン

#### R8g インスタンス

| データベース | 新規サポートリージョン |
|-------------|------------------------|
| Amazon Aurora MySQL compatibility | アジアパシフィック (香港、大阪、ジャカルタ、ソウル、シンガポール)、Canada (Central) |
| Amazon Aurora PostgreSQL compatibility | アジアパシフィック (香港、大阪、ジャカルタ、ソウル、シンガポール)、Canada (Central) |
| Amazon RDS for PostgreSQL | アジアパシフィック (香港、大阪、ジャカルタ、ソウル、シンガポール)、Canada (Central) |
| Amazon RDS for MySQL | アジアパシフィック (香港、大阪、ジャカルタ、ソウル、シンガポール)、Canada (Central) |
| Amazon RDS for MariaDB | アジアパシフィック (香港、大阪、ジャカルタ、ソウル、シンガポール)、Canada (Central) |

#### R7i インスタンス

| データベース | 新規サポートリージョン |
|-------------|------------------------|
| Amazon Aurora MySQL compatibility | アジアパシフィック (ハイデラバード) |
| Amazon Aurora PostgreSQL compatibility | アジアパシフィック (ハイデラバード) |

#### R7g インスタンス

| データベース | 新規サポートリージョン |
|-------------|------------------------|
| Amazon Aurora MySQL compatibility | アフリカ (ケープタウン) |
| Amazon Aurora PostgreSQL compatibility | アフリカ (ケープタウン) |

### R8g インスタンスの仕様

| インスタンスサイズ | vCPU | メモリ (GiB) | ネットワーク帯域幅 (Gbps) | EBS 帯域幅 (Gbps) |
|-------------------|------|-------------|--------------------------|---------------------|
| r8g.large | 2 | 16 | 最大 12.5 | 最大 10 |
| r8g.xlarge | 4 | 32 | 最大 12.5 | 最大 10 |
| r8g.2xlarge | 8 | 64 | 最大 15 | 最大 10 |
| r8g.4xlarge | 16 | 128 | 最大 15 | 最大 10 |
| r8g.8xlarge | 32 | 256 | 12.5 | 10 |
| r8g.12xlarge | 48 | 384 | 20 | 15 |
| r8g.16xlarge | 64 | 512 | 25 | 20 |
| r8g.24xlarge | 96 | 768 | 37.5 | 30 |
| r8g.48xlarge | 192 | 1536 | 50 | 40 |

## 設定方法

### 前提条件

1. Amazon Aurora または RDS インスタンスが作成されているか、新規作成を予定していること
2. 適切な IAM 権限があること
3. サポートされるデータベースエンジンバージョンを使用していること

### 手順

#### ステップ 1: 新しいインスタンスの起動 (コンソールを使用)

1. Amazon RDS Management Console にアクセス
2. 「Create database」をクリック
3. データベースエンジン (Aurora MySQL、Aurora PostgreSQL、RDS MySQL、RDS PostgreSQL、RDS MariaDB) を選択
4. インスタンスクラスで「Memory optimized classes」を選択
5. R8g、R7g、または R7i インスタンスを選択
6. その他の設定を行い、「Create database」をクリック

#### ステップ 2: 新しいインスタンスの起動 (AWS CLI を使用)

```bash
# Aurora MySQL クラスターで R8g インスタンスを起動
aws rds create-db-cluster \
  --db-cluster-identifier my-aurora-cluster \
  --engine aurora-mysql \
  --engine-version 8.0.mysql_aurora.3.08.0 \
  --master-username admin \
  --master-user-password MyPassword123 \
  --region ap-southeast-1

aws rds create-db-instance \
  --db-instance-identifier my-aurora-instance \
  --db-cluster-identifier my-aurora-cluster \
  --engine aurora-mysql \
  --db-instance-class db.r8g.2xlarge \
  --region ap-southeast-1
```

これらのコマンドは、Aurora MySQL クラスターを作成し、R8g.2xlarge インスタンスを起動します。

#### ステップ 3: 既存のインスタンスのアップグレード

```bash
# 既存のインスタンスを R8g にアップグレード
aws rds modify-db-instance \
  --db-instance-identifier my-existing-instance \
  --db-instance-class db.r8g.4xlarge \
  --apply-immediately
```

このコマンドは、既存のインスタンスを R8g.4xlarge にアップグレードします。`--apply-immediately` フラグを使用すると、変更が即座に適用されます。

#### ステップ 4: パフォーマンスの確認

```bash
# CloudWatch メトリクスを使用してパフォーマンスを確認
aws cloudwatch get-metric-statistics \
  --namespace AWS/RDS \
  --metric-name CPUUtilization \
  --dimensions Name=DBInstanceIdentifier,Value=my-aurora-instance \
  --start-time 2026-01-20T00:00:00Z \
  --end-time 2026-01-21T00:00:00Z \
  --period 3600 \
  --statistics Average
```

このコマンドは、過去 24 時間の CPU 使用率の平均を取得します。

## メリット

### ビジネス面

- **コスト効率の向上**: 最大 29% の価格/パフォーマンス向上により、コストを削減できます
- **パフォーマンス向上**: 最大 40% のパフォーマンス向上により、より高速なクエリ実行が可能になります
- **スケーラビリティ**: 48xlarge サイズまでスケールアップできます

### 技術面

- **最新ハードウェア**: AWS Graviton4 プロセッサと DDR5 メモリを使用
- **高帯域幅**: 最大 50Gbps のネットワーク帯域幅と最大 40Gbps の EBS 帯域幅
- **互換性**: 既存の Aurora および RDS ワークロードと完全に互換性があります

## デメリット・制約事項

### 制限事項

- すべてのデータベースエンジンバージョンで R8g インスタンスがサポートされているわけではありません
- 一部のリージョンでは、特定のインスタンスサイズが利用できない場合があります
- Graviton ベースのインスタンスは、ARM64 アーキテクチャを使用します

### 考慮すべき点

- データベースエンジンバージョンが R8g インスタンスをサポートしているか確認してください
- アプリケーションが ARM64 アーキテクチャと互換性があることを確認してください
- パフォーマンステストを実施して、ワークロードに適したインスタンスサイズを選択してください

## ユースケース

### ユースケース 1: 高性能データベースのアップグレード

**シナリオ**: 既存の R6g インスタンスから R8g インスタンスにアップグレードして、パフォーマンスを向上させる。

**実装例**:
1. 現在の R6g.4xlarge インスタンスのパフォーマンスを測定
2. R8g.4xlarge インスタンスにアップグレード
3. パフォーマンスを再測定して、向上を確認
4. コスト効率を評価

**効果**: 最大 40% のパフォーマンス向上と最大 29% の価格/パフォーマンス向上を実現できます。

### ユースケース 2: リージョン拡大

**シナリオ**: アジアパシフィックリージョンでの事業拡大に伴い、香港、大阪、ジャカルタリージョンで R8g インスタンスを使用してデータベースを展開する。

**実装例**:
1. 香港、大阪、ジャカルタリージョンで Aurora MySQL クラスターを作成
2. R8g インスタンスを使用して高性能データベースを提供
3. グローバル読み取りレプリカを設定して、地域間のデータ同期を実現

**効果**: 地域の顧客に対して、低レイテンシで高性能なデータベースサービスを提供できます。

### ユースケース 3: 大規模ワークロードのスケーリング

**シナリオ**: 大規模なデータベースワークロードに対して、R8g.48xlarge インスタンスを使用してスケールアップする。

**実装例**:
1. 現在の R7g.16xlarge インスタンスから R8g.48xlarge にアップグレード
2. 192 vCPU と 1536 GiB のメモリを活用して、大規模なワークロードを処理
3. I/O 最適化された Aurora クラスター構成を使用して、さらにパフォーマンスを向上

**効果**: 大規模なデータベースワークロードを効率的に処理できます。

## 料金

R8g インスタンスの料金は、インスタンスサイズとリージョンによって異なります。同等サイズの Graviton3 ベースのインスタンスと比較して、最大 29% の価格/パフォーマンス向上を提供します。

詳細については、[Amazon RDS Pricing](https://aws.amazon.com/rds/pricing/) を参照してください。

## 利用可能リージョン

R8g インスタンスは、以下のリージョンで利用可能です。

- US East (N. Virginia, Ohio)
- US West (Oregon)
- Europe (Frankfurt)
- アジアパシフィック (香港、大阪、ジャカルタ、ソウル、シンガポール)
- Canada (Central)

R7i インスタンスは、アジアパシフィック (ハイデラバード) で利用可能です。
R7g インスタンスは、アフリカ (ケープタウン) で利用可能です。

## 関連サービス・機能

- **Amazon Aurora**: 高性能でスケーラブルなクラウドネイティブデータベース
- **Amazon RDS**: マネージドリレーショナルデータベースサービス
- **AWS Graviton**: AWS が設計した ARM ベースのプロセッサ
- **Amazon CloudWatch**: データベースパフォーマンスを監視するサービス

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/amazon-aurora-rds-r8g-r7g-r7i-instances-additional-regions/)
- [Amazon Aurora Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Concepts.DBInstanceClass.SupportAurora.html)
- [Amazon RDS Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.DBInstanceClass.Support.html)
- [Amazon RDS Pricing](https://aws.amazon.com/rds/pricing/)

## まとめ

Amazon Aurora および RDS が追加リージョンで R8g、R7g、R7i データベースインスタンスをサポートしたことで、より多くの地域で最新のハードウェアを活用した高性能データベースを使用できるようになりました。AWS Graviton4 ベースの R8g インスタンスは、最大 40% のパフォーマンス向上と最大 29% の価格/パフォーマンス向上を提供し、コスト効率を大幅に改善します。アジアパシフィックリージョンやアフリカリージョンでデータベースを運用している組織は、この機能を活用してパフォーマンスとコスト効率を向上させることをお勧めします。
