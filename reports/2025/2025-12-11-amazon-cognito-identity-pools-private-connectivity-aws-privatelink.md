# Amazon Cognito Identity Pools - AWS PrivateLink によるプライベート接続

**リリース日**: 2025 年 12 月 11 日
**サービス**: Amazon Cognito Identity Pools
**機能**: AWS PrivateLink によるプライベート接続サポート

## 概要

Amazon Cognito Identity Pools が AWS PrivateLink をサポートし、VPC と Cognito 間のプライベート接続を通じてフェデレーテッドアイデンティティを AWS 認証情報に安全に交換できるようになりました。この機能により、認証トラフィックをパブリックインターネット経由でルーティングする必要がなくなり、ワークロードのセキュリティが強化されます。

Identity Pools は認証済みおよびゲストアイデンティティを AWS IAM ロールにマッピングし、一時的な AWS 認証情報を提供します。この新機能により、これらの操作がセキュアでプライベートな接続を通じて行われるようになります。

**アップデート前の課題**

- Identity Pools への認証トラフィックがパブリックインターネットを経由していた
- VPC 内のワークロードからの認証にパブリック IP が必要だった
- セキュリティ要件の厳しい環境での利用が制限されていた


**アップデート後の改善**

- VPC エンドポイント経由でのプライベート接続が可能
- パブリックインターネットを経由しない認証
- セキュリティ要件の厳しい環境での利用が可能

## サービスアップデートの詳細

### 主要機能

1. **プライベート接続**
   - VPC と Cognito 間の直接接続
   - パブリックインターネット不要
   - インターフェイス VPC エンドポイント使用

2. **セキュリティ強化**
   - 認証トラフィックの完全なプライベート化
   - ネットワーク境界内での認証処理
   - ファイアウォールルールの簡素化

3. **既存機能との互換性**
   - 認証済みアイデンティティのサポート
   - ゲストアイデンティティのサポート
   - IAM ロールマッピング

## 技術仕様

### サポートされる操作

| 操作 | サポート |
|------|---------|
| GetId | ○ |
| GetCredentialsForIdentity | ○ |
| GetOpenIdToken | ○ |
| GetOpenIdTokenForDeveloperIdentity | ○ |

### VPC エンドポイント設定

| 項目 | 詳細 |
|------|------|
| エンドポイントタイプ | インターフェイス |
| サービス名 | com.amazonaws.{region}.cognito-identity |
| プライベート DNS | サポート |

## 設定方法

### 前提条件

1. VPC の作成
2. サブネットの設定
3. セキュリティグループの設定
4. Amazon Cognito Identity Pool の作成

### 手順

#### ステップ 1: VPC エンドポイントの作成

```bash
aws ec2 create-vpc-endpoint \
    --vpc-id vpc-1234567890abcdef0 \
    --service-name com.amazonaws.ap-northeast-1.cognito-identity \
    --vpc-endpoint-type Interface \
    --subnet-ids subnet-1234567890abcdef0 \
    --security-group-ids sg-1234567890abcdef0
```

Cognito Identity Pools 用の VPC インターフェイスエンドポイントを作成します。

#### ステップ 2: プライベート DNS の有効化

VPC エンドポイント作成時にプライベート DNS を有効化すると、既存のアプリケーションコードを変更せずにプライベート接続を使用できます。

#### ステップ 3: アプリケーションからの接続

VPC 内のアプリケーションから通常通り Cognito Identity Pools API を呼び出します。トラフィックは自動的に VPC エンドポイント経由でルーティングされます。

## メリット

### ビジネス面

- **コンプライアンス対応**: 規制要件を満たすプライベート接続
- **セキュリティ強化**: 認証トラフィックの完全なプライベート化
- **リスク軽減**: パブリックインターネット露出の排除

### 技術面

- **ネットワーク簡素化**: NAT ゲートウェイやインターネットゲートウェイ不要
- **低レイテンシー**: AWS バックボーンネットワーク経由の通信
- **既存コード互換**: アプリケーションコードの変更不要

## デメリット・制約事項

### 制限事項

- AWS China (Beijing) リージョンでは利用不可
- AWS GovCloud (US) リージョンでは利用不可
- VPC エンドポイントの追加料金が発生

### 考慮すべき点

- VPC エンドポイントの料金を考慮
- セキュリティグループの適切な設定が必要

## ユースケース

### ユースケース 1: 金融機関のセキュアな認証

**シナリオ**: 金融機関で、認証トラフィックをパブリックインターネットに露出させたくない

**効果**: VPC エンドポイント経由でプライベートに認証を処理

### ユースケース 2: ヘルスケアアプリケーション

**シナリオ**: HIPAA 準拠が必要なヘルスケアアプリケーションで、認証トラフィックを保護したい

**効果**: プライベート接続によりコンプライアンス要件を満たす

### ユースケース 3: プライベートサブネットからの認証

**シナリオ**: プライベートサブネット内のアプリケーションから Cognito を使用したい

**効果**: NAT ゲートウェイなしで Cognito Identity Pools にアクセス可能

## 料金

VPC エンドポイントの作成には追加料金が発生します。

| 項目 | 料金 |
|------|------|
| VPC エンドポイント | 時間あたりの料金 + データ処理料金 |

詳細は [AWS PrivateLink 料金ページ](https://aws.amazon.com/privatelink/pricing/) を参照してください。

## 利用可能リージョン

Amazon Cognito Identity Pools が利用可能なすべてのリージョン（AWS China (Beijing) および AWS GovCloud (US) を除く）で利用できます。

## 関連サービス・機能

- **Amazon Cognito User Pools**: ユーザー認証とディレクトリサービス（PrivateLink サポート済み）
- **AWS PrivateLink**: VPC とサービス間のプライベート接続
- **AWS IAM**: アイデンティティとアクセス管理

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/12/amazon-cognito-identity-pools-private-connectivity-aws-privatelink/)
- [VPC インターフェイスエンドポイントの作成](https://docs.aws.amazon.com/vpc/latest/privatelink/create-interface-endpoint.html)
- [Amazon Cognito 開発者ガイド](https://docs.aws.amazon.com/cognito/latest/developerguide/vpc-interface-endpoints.html)
- [AWS PrivateLink 料金](https://aws.amazon.com/privatelink/pricing/)

## まとめ

Amazon Cognito Identity Pools が AWS PrivateLink をサポートし、VPC からのプライベート接続が可能になりました。認証トラフィックをパブリックインターネットに露出させることなく、セキュアにフェデレーテッドアイデンティティを AWS 認証情報に交換できます。セキュリティ要件の厳しい環境やコンプライアンス対応が必要なワークロードに最適です。
