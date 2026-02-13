# Amazon S3 on Outposts - 第2世代 AWS Outposts racks 対応

**リリース日**: 2026年01月15日
**サービス**: Amazon S3 on Outposts
**機能**: 第2世代 AWS Outposts racks のサポート

## 概要

Amazon S3 on Outposts が、第2世代 AWS Outposts racks で利用可能になりました。これにより、データレジデンシー、低レイテンシ、ローカルデータ処理のユースケースをオンプレミス環境で実現できます。第2世代 Outposts racks 上の S3 on Outposts は、196 TB、490 TB、786 TB の 3 つのストレージティアを提供し、本番ワークロード、バックアップ、アーカイブワークロードなど、ワークロードに合わせてストレージティアを選択できます。

S3 on Outposts を使用すると、使い慣れた S3 API と機能を使用してデータの保存、保護、取得、アクセス制御を実行できます。AWS Outposts は、AWS インフラストラクチャ、サービス、ツールを、事実上あらゆるデータセンター、コロケーション スペース、オンプレミス施設に拡張する完全マネージド型サービスであり、一貫したハイブリッド体験を提供します。

**アップデート前の課題**

- 第1世代 Outposts racks では、ストレージ容量の選択肢が限定されていた (26 TB、48 TB、96 TB、240 TB、380 TB)
- 大規模なデータレジデンシーや低レイテンシのユースケースでは、複数の Outposts racks が必要になる場合があった
- 第1世代 Outposts racks のパフォーマンスは、最新世代の EC2 インスタンスと比較して制限されていた

**アップデート後の改善**

- 第2世代 Outposts racks で S3 on Outposts が利用可能になり、より高いパフォーマンスとスケーラビリティを実現
- 196 TB、490 TB、786 TB の 3 つの新しいストレージティアにより、ワークロードに合わせた柔軟な容量選択が可能
- 第2世代 Outposts racks の新しいネットワークアーキテクチャにより、組み込みの回復力とセルフサービス LGW 設定が提供される
- 最新の x86 ベースの EC2 インスタンス (M7i、C7i、R7i) をサポートし、前世代と比較して最大 40% のパフォーマンス向上を実現

## アーキテクチャ図

```mermaid
flowchart TD
    subgraph OnPrem["🏢 On-Premises / Co-location"]
        subgraph Outposts["⚡ AWS Outposts Rack<br/>(Second-Generation)"]
            S3["🪣 S3 on Outposts<br/>(196TB / 490TB / 786TB)"]
            EC2["💻 EC2 Instances<br/>(M7i / C7i / R7i)"]
            LGW["🌐 Local Gateway<br/>(Self-Service Configuration)"]
        end
    end

    subgraph AWS["☁️ AWS Cloud"]
        Region["🌍 AWS Region"]
        S3Region["🪣 S3 Bucket"]
        IAM["🔐 IAM"]
    end

    Apps(["📱 Applications"]) --> EC2
    EC2 --> S3
    S3 <-->|DataSync<br/>Replication| S3Region
    S3 -->|IAM Permissions<br/>(Local Cache)| IAM
    LGW <-->|Service Link| Region

    classDef onprem fill:#FFF3E0,stroke:#FF9800,stroke-width:2px,color:#333333
    classDef outposts fill:#E8F1FF,stroke:#4A90E2,stroke-width:2px,color:#333333
    classDef storage fill:#E8EAF6,stroke:#C5CAE9,stroke-width:2px,color:#283593
    classDef compute fill:#FFE0B2,stroke:#FFCC80,stroke-width:2px,color:#5D4037
    classDef network fill:#E9F7EC,stroke:#66BB6A,stroke-width:2px,color:#333333
    classDef cloud fill:none,stroke:#CCCCCC,stroke-width:2px,color:#666666
    classDef app fill:#E3F2FD,stroke:#BBDEFB,stroke-width:2px,color:#1565C0

    class OnPrem onprem
    class Outposts outposts
    class S3 storage
    class EC2 compute
    class LGW network
    class AWS cloud
    class Region,S3Region,IAM cloud
    class Apps app
```

第2世代 Outposts racks 上で S3 on Outposts が実行され、オンプレミスでのデータ処理と AWS クラウドとの統合を実現します。

## サービスアップデートの詳細

### 主要機能

1. **3 つのストレージティア**
   - **196 TB**: 中規模のワークロード向け
   - **490 TB**: 大規模なワークロード向け
   - **786 TB**: 非常に大規模なワークロード、バックアップ、アーカイブ向け
   - ワークロードに合わせて適切なストレージ容量を選択可能

2. **第2世代 Outposts racks の機能**
   - 最新の x86 ベース EC2 インスタンス (M7i、C7i、R7i) をサポート
   - 前世代と比較して vCPU、メモリ、ネットワーク帯域幅が 2 倍
   - 最大 40% のパフォーマンス向上
   - 新しいネットワークアーキテクチャによる組み込みの回復力

3. **S3 API との互換性**
   - 使い慣れた S3 API と機能を使用してデータを管理
   - S3 Lifecycle ルール、バージョニング、暗号化、タグ付けをサポート
   - AWS IAM 権限のローカルキャッシュにより、オブジェクト API リクエストの初回バイトレイテンシを削減

4. **データレジデンシーと低レイテンシ**
   - データをオンプレミスに保存し、データレジデンシー要件を満たす
   - ローカルデータ処理により、低レイテンシアクセスを実現
   - AWS DataSync を使用して AWS リージョンとの間でデータ転送を自動化

## 技術仕様

### ストレージティア

| ストレージティア | 容量 | 主なユースケース |
|----------------|------|-----------------|
| Tier 1 | 196 TB | 中規模の本番ワークロード |
| Tier 2 | 490 TB | 大規模な本番ワークロード、バックアップ |
| Tier 3 | 786 TB | 非常に大規模なワークロード、アーカイブ |

### API 変更履歴

関連する API 変更はありません。既存の S3 API をそのまま使用できます。

### 第2世代 Outposts racks のスペック

| 項目 | 第1世代 | 第2世代 |
|------|--------|--------|
| EC2 インスタンス | M6i、C6i、R6i など | M7i、C7i、R7i など |
| vCPU / メモリ | 標準 | 2 倍 |
| ネットワーク帯域幅 | 標準 | 2 倍 |
| パフォーマンス | 標準 | 最大 40% 向上 |
| ネットワークアーキテクチャ | 標準 | 組み込みの回復力、セルフサービス LGW 設定 |

## 設定方法

### 前提条件

1. 第2世代 AWS Outposts racks がデプロイされている
2. AWS アカウントに S3 on Outposts の使用権限がある
3. オンプレミスと AWS リージョン間のネットワーク接続が確立されている

### 手順

#### ステップ1: S3 on Outposts のストレージティアを選択

```bash
# AWS CLI で S3 on Outposts の設定を確認
aws s3control list-regional-buckets \
    --account-id <account-id> \
    --outpost-id <outpost-id>
```

196 TB、490 TB、786 TB のいずれかのストレージティアを選択します。

#### ステップ2: S3 バケットを作成

```bash
# S3 on Outposts にバケットを作成
aws s3control create-bucket \
    --bucket <bucket-name> \
    --outpost-id <outpost-id>
```

Outposts 上に S3 バケットを作成します。

#### ステップ3: アクセスポイントを設定

```bash
# アクセスポイントを作成
aws s3control create-access-point \
    --account-id <account-id> \
    --name <access-point-name> \
    --bucket <bucket-name> \
    --vpc-configuration VpcId=<vpc-id>
```

VPC 内からアクセスするためのアクセスポイントを作成します。

#### ステップ4: データをアップロード

```bash
# S3 API を使用してデータをアップロード
aws s3 cp local-file.txt s3://<bucket-name>/remote-file.txt
```

使い慣れた S3 API を使用してデータをアップロードします。

## メリット

### ビジネス面

- **データレジデンシー要件の順守**: データをオンプレミスに保存し、規制や業界の要件を満たす
- **低レイテンシアクセス**: オンプレミスでのローカルデータ処理により、低レイテンシアクセスを実現
- **コスト最適化**: ワークロードに合わせて適切なストレージティアを選択し、コストを最適化

### 技術面

- **高パフォーマンス**: 第2世代 Outposts racks の最新 EC2 インスタンスにより、最大 40% のパフォーマンス向上
- **スケーラビリティ**: 196 TB から 786 TB までのストレージティアにより、大規模なワークロードに対応
- **一貫した体験**: AWS クラウドと同じ S3 API と機能を使用でき、一貫したハイブリッド体験を提供
- **回復力の向上**: 新しいネットワークアーキテクチャにより、組み込みの回復力とセルフサービス LGW 設定が提供される

## デメリット・制約事項

### 制限事項

- 第2世代 Outposts racks が利用可能なリージョンと国/地域に限定
- AWS アカウントあたり最大 100 個のバケットを作成可能
- ストレージ容量は選択したストレージティアに制限される

### 考慮すべき点

- 第2世代 Outposts racks の初期投資コストが必要
- オンプレミスと AWS リージョン間のネットワーク接続が必要
- データ転送コストが発生する場合がある (AWS DataSync 使用時)

## ユースケース

### ユースケース1: データレジデンシー要件を満たす大規模データ処理

**シナリオ**: 金融機関が規制要件により、顧客データをオンプレミスに保存しながら、大規模なデータ分析を実行したい。

**実装例**:
```bash
# 786 TB ストレージティアを選択
# S3 バケットを作成
aws s3control create-bucket \
    --bucket customer-data \
    --outpost-id op-123456789abcdef01

# データをアップロード
aws s3 sync /local/customer-data/ s3://customer-data/
```

**効果**: 786 TB のストレージティアにより、大量の顧客データをオンプレミスに保存し、データレジデンシー要件を満たしながら高速なデータ分析を実現。

### ユースケース2: 低レイテンシの画像・動画処理

**シナリオ**: メディア企業が、オンプレミスのスタジオで生成された大量の画像・動画ファイルを低レイテンシで処理したい。

**実装例**:
```bash
# 490 TB ストレージティアを選択
# S3 バケットを作成
aws s3control create-bucket \
    --bucket media-files \
    --outpost-id op-123456789abcdef01

# 画像・動画をアップロード
aws s3 cp /local/media/ s3://media-files/ --recursive

# EC2 インスタンス (M7i) で処理
aws ec2 run-instances \
    --image-id ami-0abcdef1234567890 \
    --instance-type m7i.4xlarge \
    --subnet-id subnet-outposts
```

**効果**: 490 TB のストレージティアと第2世代 Outposts racks の高性能 EC2 インスタンスにより、低レイテンシで大量の画像・動画を処理。

### ユースケース3: バックアップとアーカイブ

**シナリオ**: 企業が、本番データのバックアップとアーカイブをオンプレミスに保存し、災害復旧 (DR) に備えたい。

**実装例**:
```bash
# 786 TB ストレージティアを選択
# バックアップバケットを作成
aws s3control create-bucket \
    --bucket backup-archive \
    --outpost-id op-123456789abcdef01

# S3 Lifecycle ルールを設定
aws s3api put-bucket-lifecycle-configuration \
    --bucket backup-archive \
    --lifecycle-configuration file://lifecycle-policy.json

# AWS DataSync でリージョンにもレプリケーション
aws datasync create-task \
    --source-location-arn arn:aws:s3-outposts:::outpost/op-123456789abcdef01/bucket/backup-archive \
    --destination-location-arn arn:aws:s3:::backup-archive-dr
```

**効果**: 786 TB のストレージティアにより、大量のバックアップとアーカイブをオンプレミスに保存し、AWS リージョンにもレプリケーションして災害復旧に備える。

## 料金

S3 on Outposts の料金は、選択したストレージティアと Outposts racks の構成により異なります。第2世代 Outposts racks の料金には、以下が含まれます:

- Outposts racks のハードウェアとインフラストラクチャ
- S3 on Outposts のストレージ容量
- EC2 インスタンス、ネットワーク、その他のサービスの使用料

### 料金例

| ストレージティア | 月額料金 (概算) | 備考 |
|----------------|----------------|------|
| 196 TB | ベース料金 + ストレージ料金 | Outposts racks の構成により異なる |
| 490 TB | ベース料金 + ストレージ料金 | Outposts racks の構成により異なる |
| 786 TB | ベース料金 + ストレージ料金 | Outposts racks の構成により異なる |

詳細な料金情報は [AWS Outposts 料金ページ](https://aws.amazon.com/outposts/pricing/) を参照してください。

## 利用可能リージョン

第2世代 Outposts racks 上の S3 on Outposts は、第2世代 Outposts racks が利用可能なすべての AWS リージョンと国/地域で利用できます。

詳細なリージョン情報は [Outposts rack FAQs](https://aws.amazon.com/outposts/faqs/) を参照してください。

## 関連サービス・機能

- **AWS Outposts racks**: AWS インフラストラクチャをオンプレミスに拡張する完全マネージド型サービス
- **Amazon EC2**: Outposts racks 上で実行されるコンピュートインスタンス
- **AWS DataSync**: AWS リージョンと Outposts 間のデータ転送を自動化
- **AWS PrivateLink**: VPC 内からバケットとアクセスポイントを管理するためのプライベートエンドポイント
- **AWS Resource Access Manager (RAM)**: 複数のアカウント間で S3 容量を共有

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/amazon-s3-second-generation-aws-outposts-racks/)
- [S3 on Outposts ページ](https://aws.amazon.com/s3/outposts/)
- [ドキュメント: Amazon S3 on Outposts](https://docs.aws.amazon.com/AmazonS3/latest/dev/S3onOutposts.html)
- [AWS Outposts FAQs](https://aws.amazon.com/outposts/faqs/)
- [AWS Outposts 料金ページ](https://aws.amazon.com/outposts/pricing/)

## まとめ

Amazon S3 on Outposts が第2世代 AWS Outposts racks で利用可能になり、196 TB、490 TB、786 TB の 3 つの新しいストレージティアが提供されます。これにより、データレジデンシー、低レイテンシ、ローカルデータ処理のユースケースをオンプレミス環境で高いパフォーマンスとスケーラビリティで実現できます。第2世代 Outposts racks の最新 EC2 インスタンスと新しいネットワークアーキテクチャにより、前世代と比較して最大 40% のパフォーマンス向上が期待できます。金融機関、メディア企業、大規模なバックアップとアーカイブが必要な企業など、幅広いユースケースに対応可能です。
