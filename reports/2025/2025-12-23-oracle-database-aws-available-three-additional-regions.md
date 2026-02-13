# Oracle Database@AWS - 東京、フランクフルト、オハイオリージョンで一般提供開始

**リリース日**: 2025年12月23日
**サービス**: Oracle Database@AWS
**機能**: 3 つの追加リージョンでの一般提供

## 概要

Oracle Database@AWS が、US-East-2 (オハイオ)、EU-Central-1 (フランクフルト)、AP-Northeast-1 (東京) の 3 つの追加 AWS リージョンで一般提供を開始しました。Oracle Database@AWS は、AWS データセンター内の Oracle Cloud Infrastructure (OCI) マネージド Oracle Exadata システム上でデータベースサービスにアクセスできるようにするサービスです。

この拡張により、EU および日本でリージョン内データレジデンシー要件を持つお客様は、オンプレミスの Oracle Exadata アプリケーションを AWS に簡単に移行できるようになりました。これで、US-East-1 (バージニア北部)、US-West-2 (オレゴン)、US-East-2 (オハイオ)、EU-Central-1 (フランクフルト)、AP-Northeast-1 (東京) の 5 つのリージョンで OCI Exadata Database Service、OCI Autonomous Database on Dedicated Infrastructure、OCI Autonomous Recovery Service を実行できます。

**アップデート前の課題**

- Oracle Database@AWS は米国の 2 リージョン (バージニア北部、オレゴン) のみで利用可能だった
- EU や日本のデータレジデンシー要件を持つお客様は利用できなかった
- 地理的に離れたリージョンでのレイテンシが課題だった

**アップデート後の改善**

- 東京リージョンで日本のお客様がローカルにサービスを利用可能
- フランクフルトリージョンで EU のデータレジデンシー要件に対応
- オハイオリージョンで米国東部の冗長性オプションを提供

## サービスアップデートの詳細

### 主要機能

1. **OCI Exadata Database Service**
   - Oracle Exadata 上で実行されるフルマネージド Oracle Database
   - 高性能なトランザクション処理
   - 自動パッチ適用とバックアップ

2. **OCI Autonomous Database on Dedicated Infrastructure**
   - 自己駆動型、自己保護型、自己修復型データベース
   - 専用インフラストラクチャでの分離
   - 機械学習による自動チューニング

3. **OCI Autonomous Recovery Service**
   - 自動バックアップと復旧
   - ポイントインタイムリカバリ
   - クロスリージョンレプリケーション

## 技術仕様

### 利用可能リージョン

| リージョン | リージョンコード | 提供開始 |
|-----------|-----------------|---------|
| 米国東部 (バージニア北部) | us-east-1 | 既存 |
| 米国西部 (オレゴン) | us-west-2 | 既存 |
| 米国東部 (オハイオ) | us-east-2 | 新規 |
| 欧州 (フランクフルト) | eu-central-1 | 新規 |
| アジアパシフィック (東京) | ap-northeast-1 | 新規 |

### 利用可能なサービス

| サービス | 説明 |
|---------|------|
| OCI Exadata Database Service | Oracle Exadata 上のマネージド Oracle Database |
| OCI Autonomous Database on Dedicated Infrastructure | 専用インフラ上の自律型データベース |
| OCI Autonomous Recovery Service | 自動バックアップと復旧サービス |

## 設定方法

### 前提条件

1. AWS アカウント
2. AWS Marketplace での Oracle プライベートオファーの受諾
3. 適切な IAM 権限

### 手順

#### ステップ 1: AWS Marketplace でのプライベートオファー取得

AWS Marketplace から Oracle のプライベートオファーをリクエストします。

1. [AWS Marketplace の Oracle ページ](https://aws.amazon.com/marketplace/pp/prodview-qks5dl3hr7nfw) にアクセス
2. プライベートオファーをリクエスト
3. Oracle からのオファーを受諾

#### ステップ 2: AWS マネジメントコンソールでのセットアップ

AWS マネジメントコンソールを使用してデータベースリソースをセットアップします。

1. Oracle Database@AWS コンソールにアクセス
2. 対象リージョン (東京、フランクフルト、オハイオ) を選択
3. データベースタイプを選択 (Exadata または Autonomous)
4. 構成パラメータを設定

#### ステップ 3: データベースの作成

選択したサービスに応じてデータベースを作成します。

```bash
# AWS CLI での例 (概念的)
aws odb create-database \
  --region ap-northeast-1 \
  --database-type exadata \
  --db-name myoracledb \
  --admin-password "SecurePassword123!"
```

## メリット

### ビジネス面

- **データレジデンシー対応**: EU および日本の規制要件に準拠
- **レイテンシ削減**: ローカルリージョンでのサービス提供
- **移行の簡素化**: オンプレミス Oracle Exadata からの移行パスを提供

### 技術面

- **高性能**: Oracle Exadata の性能を AWS で活用
- **マネージドサービス**: パッチ適用、バックアップ、監視の自動化
- **AWS 統合**: AWS サービスとのシームレスな連携

## デメリット・制約事項

### 制限事項

- AWS Marketplace でのプライベートオファーが必要
- 一部のリージョンでは利用不可
- Oracle ライセンスの考慮が必要

### 考慮すべき点

- Oracle と AWS の両方のコストが発生
- OCI と AWS の両方の知識が必要
- ネットワーク構成の計画が重要

## ユースケース

### ユースケース 1: 日本企業の Oracle ワークロード移行

**シナリオ**: 日本国内のデータレジデンシー要件を持つ企業が Oracle Exadata を AWS に移行

**実装例**:
- 東京リージョンで Oracle Database@AWS をセットアップ
- オンプレミス Oracle Exadata からデータを移行
- AWS サービス (Amazon S3、Amazon Redshift など) と統合

**効果**: データを日本国内に保持しながら、AWS のスケーラビリティを活用

### ユースケース 2: EU GDPR 対応

**シナリオ**: EU の GDPR 要件を満たすために、Oracle データベースを EU 内で運用

**実装例**:
- フランクフルトリージョンで Autonomous Database をデプロイ
- EU 内でのデータ処理を確保
- AWS のセキュリティサービスと統合

**効果**: GDPR 準拠を維持しながら、クラウドの利点を活用

### ユースケース 3: 米国東部での冗長性確保

**シナリオ**: 米国東部で Oracle ワークロードの冗長性を確保

**実装例**:
- バージニア北部とオハイオの両リージョンでデプロイ
- クロスリージョンレプリケーションを設定
- 災害復旧計画を実装

**効果**: 高可用性と災害復旧能力の向上

## 料金

Oracle Database@AWS の料金は、Oracle と AWS の両方のコンポーネントで構成されます。詳細は AWS Marketplace でのプライベートオファーを通じて確認してください。

## 利用可能リージョン

- 米国東部 (バージニア北部) - us-east-1
- 米国西部 (オレゴン) - us-west-2
- 米国東部 (オハイオ) - us-east-2 (新規)
- 欧州 (フランクフルト) - eu-central-1 (新規)
- アジアパシフィック (東京) - ap-northeast-1 (新規)

## 関連サービス・機能

- **AWS Marketplace**: Oracle プライベートオファーの取得
- **AWS Direct Connect**: オンプレミスとの接続
- **Amazon VPC**: ネットワーク分離
- **AWS IAM**: アクセス管理

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/12/oracle-database-aws-available-three-additional-regions/)
- [Oracle Database@AWS 概要](https://aws.amazon.com/marketplace/featured-seller/oracle)
- [ドキュメント](https://docs.aws.amazon.com/odb/)
- [AWS Marketplace - Oracle](https://aws.amazon.com/marketplace/pp/prodview-qks5dl3hr7nfw)

## まとめ

Oracle Database@AWS の東京、フランクフルト、オハイオリージョンでの一般提供開始により、日本および EU のお客様がデータレジデンシー要件を満たしながら Oracle Exadata ワークロードを AWS に移行できるようになりました。特に日本のお客様にとっては、東京リージョンでの提供開始により、低レイテンシでの Oracle Database サービス利用が可能になります。オンプレミス Oracle Exadata からの移行を検討している場合は、この機会に Oracle Database@AWS の評価を開始することをお勧めします。
