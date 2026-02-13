# Amazon OpenSearch Service - OI2 インスタンス

**リリース日**: 2025 年 12 月 17 日
**サービス**: Amazon OpenSearch Service
**機能**: OI2 (OpenSearch Optimized Instance) インスタンスファミリー

## 概要

Amazon OpenSearch Service に新しい OI2 インスタンスファミリーが追加されました。OI2 インスタンスは、OR2 インスタンスと比較して最大 9% 高いインデックス作成スループットを提供し、I8g インスタンスと比較すると最大 33% の向上を実現します。

OI2 インスタンスは、インデックス作成が多いワークロードに最適化されており、Amazon S3 ベースのマネージドストレージと第 3 世代 AWS Nitro SSD を組み合わせた高耐久性アーキテクチャを採用しています。

**アップデート前の課題**

- インデックス作成が多いワークロードでは、既存インスタンスのスループットが不足することがあった
- 高いインデックス作成スループットと耐久性の両立が困難だった
- ストレージコストとパフォーマンスのバランス調整が必要だった

**アップデート後の改善**

- OR2 比で最大 9%、I8g 比で最大 33% のインデックス作成スループット向上
- Amazon S3 ベースのマネージドストレージによる高耐久性
- 第 3 世代 AWS Nitro SSD によるキャッシング性能向上
- large から 24xlarge までの幅広いサイズ展開

## サービスアップデートの詳細

### 主要機能

1. **高いインデックス作成スループット**
   - OR2 インスタンス比で最大 9% 向上
   - I8g インスタンス比で最大 33% 向上
   - インデックス作成が多いワークロードに最適化

2. **OpenSearch Optimized アーキテクチャ**
   - OR2 インスタンスと同じアーキテクチャを採用
   - Amazon S3 ベースのリモートマネージドストレージ
   - 第 3 世代 AWS Nitro SSD によるキャッシング

3. **柔軟なサイズ展開**
   - large から 24xlarge までのサイズ
   - 最大 22.5 TB のストレージ
   - 従量課金とリザーブドインスタンスに対応

## 技術仕様

### インスタンスサイズ

| サイズ | 説明 |
|--------|------|
| large | エントリーレベル |
| xlarge | 小規模ワークロード |
| 2xlarge - 16xlarge | 中規模ワークロード |
| 24xlarge | 大規模ワークロード |

### アーキテクチャ

| コンポーネント | 説明 |
|---------------|------|
| コンピュート | OpenSearch 処理用 |
| NVMe SSD | 第 3 世代 AWS Nitro SSD (キャッシング用) |
| マネージドストレージ | Amazon S3 ベース (最大 22.5 TB) |

### 料金体系

| 項目 | 説明 |
|------|------|
| インスタンス料金 | 時間単位 (NVMe ストレージ含む) |
| マネージドストレージ | プロビジョニング容量に基づく |
| 購入オプション | オンデマンド、リザーブドインスタンス |

## 設定方法

### 前提条件

1. AWS アカウント
2. Amazon OpenSearch Service ドメインの作成権限
3. 適切な VPC 設定 (必要に応じて)

### 手順

#### ステップ 1: OpenSearch Service ドメインの作成

AWS Management Console で Amazon OpenSearch Service を開き、新しいドメインを作成します。

```bash
# AWS CLI を使用したドメイン作成例
aws opensearch create-domain \
  --domain-name my-domain \
  --engine-version OpenSearch_2.11 \
  --cluster-config InstanceType=oi2.large.search,InstanceCount=3 \
  --ebs-options EBSEnabled=false
```

OI2 インスタンスを選択する際は、インスタンスタイプに `oi2` プレフィックスを使用します。

#### ステップ 2: インスタンスサイズの選択

ワークロードに応じて適切なサイズを選択します。

- **small - medium ワークロード**: oi2.large.search - oi2.2xlarge.search
- **large ワークロード**: oi2.4xlarge.search - oi2.16xlarge.search
- **enterprise ワークロード**: oi2.24xlarge.search

## メリット

### ビジネス面

- **コスト効率**: 高いスループットにより、同じワークロードをより少ないリソースで処理
- **スケーラビリティ**: 幅広いサイズ展開でビジネス成長に対応
- **運用負荷軽減**: マネージドストレージによる運用簡素化

### 技術面

- **高スループット**: インデックス作成が多いワークロードに最適
- **高耐久性**: Amazon S3 ベースのストレージによるデータ保護
- **低レイテンシー**: NVMe SSD キャッシングによる高速アクセス

## デメリット・制約事項

### 制限事項

- 現在 12 リージョンでのみ利用可能
- 東京リージョン (ap-northeast-1) で利用可能
- EBS ストレージオプションは使用不可 (マネージドストレージのみ)

### 考慮すべき点

- 既存ドメインからの移行には計画が必要
- マネージドストレージの料金体系を理解する必要がある
- ワークロード特性に応じたサイズ選択が重要

## ユースケース

### ユースケース 1: ログ分析プラットフォーム

**シナリオ**: 大量のアプリケーションログをリアルタイムでインデックス作成し、分析

**実装例**:
```
インスタンスタイプ: oi2.4xlarge.search
ノード数: 3
マネージドストレージ: 10 TB
```

**効果**: 高いインデックス作成スループットにより、ログの取り込み遅延を最小化

### ユースケース 2: E コマース検索エンジン

**シナリオ**: 商品カタログの頻繁な更新と高速検索を両立

**実装例**:
```
インスタンスタイプ: oi2.2xlarge.search
ノード数: 3
マネージドストレージ: 5 TB
```

**効果**: 商品情報の更新を高速に反映しながら、検索レスポンスを維持

### ユースケース 3: セキュリティ情報イベント管理 (SIEM)

**シナリオ**: セキュリティイベントの大量取り込みとリアルタイム分析

**実装例**:
```
インスタンスタイプ: oi2.8xlarge.search
ノード数: 6
マネージドストレージ: 20 TB
```

**効果**: セキュリティイベントの高速インデックス作成により、脅威検出の遅延を最小化

## 料金

OI2 インスタンスは、インスタンス料金とマネージドストレージ料金の組み合わせで課金されます。

### 料金例

| 項目 | 料金体系 |
|------|---------|
| OI2 インスタンス | 時間単位 (NVMe ストレージ含む) |
| マネージドストレージ | GB-月単位 |

詳細な料金については、[Amazon OpenSearch Service 料金ページ](https://aws.amazon.com/opensearch-service/pricing/)を参照してください。

## 利用可能リージョン

以下の 12 リージョンで利用可能です。

- US East (N. Virginia, Ohio)
- US West (Oregon)
- Canada (Central)
- Asia Pacific (Mumbai, Singapore, Sydney, Tokyo)
- Europe (Frankfurt, Ireland, London, Spain)

## 関連サービス・機能

- **Amazon OpenSearch Service**: フルマネージド検索・分析サービス
- **Amazon S3**: マネージドストレージのバックエンド
- **Amazon CloudWatch**: メトリクス監視
- **AWS IAM**: アクセス制御

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/12/amazon-opensearch-service-oi2-instances/)
- [Amazon OpenSearch Service 料金ページ](https://aws.amazon.com/opensearch-service/pricing/)
- [Amazon OpenSearch Service ドキュメント](https://docs.aws.amazon.com/opensearch-service/)

## まとめ

Amazon OpenSearch Service の OI2 インスタンスは、インデックス作成が多いワークロードに最適化された新しいインスタンスファミリーです。OR2 比で最大 9% のスループット向上と、Amazon S3 ベースの高耐久性ストレージを組み合わせることで、ログ分析や SIEM などのユースケースで高いパフォーマンスを発揮します。東京リージョンでも利用可能なため、日本のお客様もすぐに活用できます。
