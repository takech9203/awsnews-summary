# AWS IAM - Outbound Identity Federation

**リリース日**: 2025 年 11 月 24 日  
**サービス**: AWS IAM  
**機能**: Outbound Identity Federation - 外部サービスへのアクセス簡素化


## 概要

AWS IAM は、Outbound Identity Federation を発表しました。この機能により、AWS から外部サービスへのアクセスを簡素化できます。

AWS の認証情報を使用して、サードパーティサービスやオンプレミスシステムに安全にアクセスできるようになります。

**アップデート前の課題**

- 以前は AWS から外部サービスにアクセスする際、API キーやシークレットなどの長期認証情報を管理する必要があった
- 以前は外部サービスへの認証情報を Secrets Manager などで個別に管理し、ローテーションを設定する必要があった
- 以前は AWS 認証情報を使用して外部サービスに直接認証する標準的な方法がなかった

**アップデート後の改善**

- 今回のアップデートにより、AWS 認証情報を使用して OIDC トークンや SAML アサーションを発行し、外部サービスに安全にアクセスできるようになった
- 今回のアップデートにより、短期認証情報の使用で長期シークレットの管理が不要になり、セキュリティリスクが軽減された
- 今回のアップデートにより、認証情報管理が一元化され、SaaS 統合やハイブリッドクラウド環境での運用が簡素化された


## サービスアップデートの詳細

### 主要機能

1. **外部サービスへの認証**
   - AWS 認証情報を使用した外部アクセス
   - OIDC トークンの発行
   - SAML アサーションのサポート

2. **セキュアな認証フロー**
   - 短期認証情報の使用
   - 最小権限の原則
   - 監査可能なアクセス

3. **統合の簡素化**
   - 認証情報管理の一元化
   - シークレット管理の削減
   - 運用負担の軽減


## 技術仕様

### Outbound Identity Federation の仕組み

| 項目 | 詳細 |
|------|------|
| プロトコル | OIDC, SAML 2.0 |
| トークン有効期間 | 設定可能 (最大 12 時間) |
| 対象 | サードパーティサービス、オンプレミス |
| 監査 | CloudTrail 統合 |

### 設定例

```json
{
  "OutboundFederationConfig": {
    "TargetService": "https://api.example.com",
    "TokenType": "OIDC",
    "Audience": "example-service",
    "Duration": 3600
  }
}
```

この設定例は、Outbound Identity Federation の構成を定義しています。`TargetService` で外部サービスの URL を指定し、`TokenType` で OIDC トークンを使用することを指定します。`Audience` はトークンの対象サービスを識別し、`Duration` でトークンの有効期間 (秒) を設定します。


## 設定方法

### 前提条件

1. IAM ロールが作成済み
2. 外部サービスが OIDC/SAML をサポート
3. 適切な IAM 権限

### 手順

#### ステップ 1: Outbound Federation の設定

```bash
aws iam create-outbound-federation-config \
  --name my-federation \
  --target-service https://api.example.com \
  --token-type OIDC \
  --audience example-service
```

#### ステップ 2: トークンの取得

```bash
aws iam get-outbound-federation-token \
  --federation-config-name my-federation
```

このコマンドは、設定した Outbound Federation 構成に基づいて、外部サービスへのアクセスに使用できる OIDC トークンを取得します。取得したトークンは、外部サービスへの認証に使用できます。


## メリット

### ビジネス面

- **運用簡素化**: 認証情報管理の一元化
- **セキュリティ強化**: 長期認証情報の削減
- **コンプライアンス**: 監査可能なアクセス

### 技術面

- **統合容易性**: 標準プロトコルのサポート
- **短期認証情報**: セキュリティリスクの軽減
- **自動化**: プログラムによるトークン取得


## デメリット・制約事項

### 制限事項

- 外部サービスが OIDC/SAML をサポートしている必要がある
- トークン有効期間に制限がある

### 考慮すべき点

- 外部サービスとの統合設計
- トークンのライフサイクル管理


## ユースケース

### ユースケース 1: SaaS 統合

**シナリオ**: AWS から SaaS サービスに安全にアクセスしたい

**効果**: Outbound Federation で AWS 認証情報を使用してアクセス

### ユースケース 2: ハイブリッドクラウド

**シナリオ**: AWS からオンプレミスシステムにアクセス

**効果**: SAML を使用してオンプレミスへの認証を実現

### ユースケース 3: マルチクラウド

**シナリオ**: AWS から他のクラウドプロバイダーにアクセス

**効果**: OIDC トークンで他クラウドへの認証を簡素化


## 料金

IAM の標準料金が適用されます。Outbound Identity Federation に追加料金はありません。


## 利用可能リージョン

AWS IAM が利用可能なすべてのリージョンで利用可能です。


## 関連サービス・機能

- **AWS IAM**: アイデンティティ管理
- **AWS IAM Identity Center**: シングルサインオン
- **AWS Secrets Manager**: シークレット管理


## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/11/aws-iam-identity-federation-external-services-jwts/)
- [AWS Blog](https://aws.amazon.com/blogs/aws/simplify-access-to-external-services-using-aws-iam-outbound-identity-federation/)
- [IAM ドキュメント](https://docs.aws.amazon.com/iam/)


## まとめ

AWS IAM の Outbound Identity Federation により、外部サービスへのアクセスが大幅に簡素化されました。AWS 認証情報を使用して安全に外部サービスにアクセスでき、認証情報管理の負担を軽減できます。
