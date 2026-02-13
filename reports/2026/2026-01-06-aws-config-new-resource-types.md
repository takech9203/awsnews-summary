# AWS Config - 21個の新しいリソースタイプのサポート追加

**リリース日**: 2026年1月6日
**サービス**: AWS Config
**機能**: 21個の追加リソースタイプのサポート

## 概要

AWS Config が Amazon EC2、Amazon SageMaker、Amazon S3 Tables などの主要サービスを含む21個の新しいリソースタイプのサポートを追加しました。この拡張により、AWS 環境全体に対するカバレッジが向上し、より広範なリソースの検出、評価、監査、修復が可能になります。

新しくサポートされたリソースタイプは、Config ルールおよび Config アグリゲーターでも利用可能になりました。すべてのリソースタイプの記録を有効にしている場合、AWS Config はこれらの新しいリソースタイプを自動的に追跡します。これにより、組織全体のコンプライアンス管理、セキュリティ監査、リソースガバナンスが強化されます。

**アップデート前の課題**

- 21個のリソースタイプについて、AWS Config による自動追跡と監査ができなかった
- これらのリソースの設定変更履歴を一元管理する仕組みがなかった
- Config ルールを使用したコンプライアンスチェックの対象外だった

**アップデート後の改善**

- 21個の新しいリソースタイプが AWS Config で自動的に追跡されるようになった
- Config ルールを使用して、これらのリソースのコンプライアンスチェックが可能になった
- Config アグリゲーターで複数アカウント・リージョンのリソースを一元管理できるようになった

## サービスアップデートの詳細

### 新しくサポートされたリソースタイプ

AWS Config が以下の21個のリソースタイプのサポートを追加しました:

1. **AWS::AppStream::AppBlockBuilder**
   - AppStream 2.0 のアプリケーションブロックビルダーの設定を追跡
   - アプリケーションパッケージングプロセスの監査が可能

2. **AWS::B2BI::Capability**
   - AWS B2B Data Interchange の Capability リソースの設定を管理
   - B2B トランザクション処理の設定変更を追跡

3. **AWS::CleanRoomsML::TrainingDataset**
   - AWS Clean Rooms ML のトレーニングデータセットの設定を追跡
   - データコラボレーションのガバナンスを強化

4. **AWS::CloudFront::KeyValueStore**
   - CloudFront のキーバリューストアの設定を監査
   - エッジコンピューティングの設定管理を強化

5. **AWS::Connect::SecurityProfile**
   - Amazon Connect のセキュリティプロファイルの変更を追跡
   - コンタクトセンターのアクセス制御を監査

6. **AWS::Deadline::Monitor**
   - AWS Deadline Cloud のモニターリソースを追跡
   - レンダリングファームの設定管理を強化

7. **AWS::EC2::SubnetCidrBlock**
   - VPC サブネットの CIDR ブロック設定を追跡
   - ネットワーク設定の変更履歴を管理

8. **AWS::ECR::ReplicationConfiguration**
   - Amazon ECR のレプリケーション設定を監査
   - コンテナイメージのマルチリージョン配布を管理

9. **AWS::GameLift::Build**
   - Amazon GameLift のビルド設定を追跡
   - ゲームサーバーのデプロイ履歴を管理

10. **AWS::GuardDuty::MalwareProtectionPlan**
    - GuardDuty のマルウェア保護プランの設定を追跡
    - セキュリティポリシーの変更履歴を管理

11. **AWS::ImageBuilder::LifecyclePolicy**
    - EC2 Image Builder のライフサイクルポリシーを監査
    - AMI 管理の自動化設定を追跡

12. **AWS::IoT::ThingGroup**
    - AWS IoT のモノグループの設定を追跡
    - IoT デバイスの組織構造を管理

13. **AWS::IoTSiteWise::Asset**
    - AWS IoT SiteWise のアセット設定を監査
    - 産業用 IoT データモデルを管理

14. **AWS::Location::APIKey**
    - Amazon Location Service の API キー設定を追跡
    - ロケーションサービスのアクセス管理を強化

15. **AWS::MediaPackageV2::OriginEndpoint**
    - AWS Elemental MediaPackage v2 のオリジンエンドポイント設定を監査
    - ライブストリーミング配信の設定を管理

16. **AWS::PCAConnectorAD::Connector**
    - AWS Private CA Connector for Active Directory の設定を追跡
    - 証明書発行プロセスのガバナンスを強化

17. **AWS::Route53::DNSSEC**
    - Route 53 の DNSSEC 設定を監査
    - DNS セキュリティ設定の変更履歴を管理

18. **AWS::S3Tables::TableBucketPolicy**
    - Amazon S3 Tables のバケットポリシーを追跡
    - テーブル形式データのアクセス制御を監査

19. **AWS::SageMaker::UserProfile**
    - Amazon SageMaker のユーザープロファイル設定を管理
    - ML プラットフォームのアクセス制御を追跡

20. **AWS::SecretsManager::ResourcePolicy**
    - AWS Secrets Manager のリソースポリシーを監査
    - シークレットのアクセス制御を追跡

21. **AWS::SSMContacts::Contact**
    - AWS Systems Manager Incident Manager の連絡先設定を追跡
    - インシデント対応プロセスの設定を管理

## 技術仕様

### サポートされるリージョン

新しいリソースタイプは、それぞれのリソースが利用可能なすべての AWS リージョンで AWS Config による追跡が可能です。詳細は [AWS Config サポートリソースタイプのドキュメント](https://docs.aws.amazon.com/config/latest/developerguide/what-is-resource-config-coverage.html) をご参照ください。

### 自動記録の設定

すべてのリソースタイプの記録を有効にしている場合、AWS Config は新しいリソースタイプを自動的に追跡します:

```json
{
  "allSupported": true,
  "includeGlobalResourceTypes": true
}
```

### Config ルールでの利用

新しいリソースタイプは、マネージドルールおよびカスタムルールで使用可能です:

```json
{
  "ConfigRuleName": "check-s3-tables-bucket-policy",
  "Source": {
    "Owner": "CUSTOM_LAMBDA",
    "SourceIdentifier": "arn:aws:lambda:us-east-1:123456789012:function:check-policy"
  },
  "Scope": {
    "ComplianceResourceTypes": [
      "AWS::S3Tables::TableBucketPolicy"
    ]
  }
}
```

## 設定方法

### 前提条件

1. AWS Config が有効化されていること
2. Config レコーダーが設定されていること
3. 適切な IAM 権限を持つこと

### 手順

#### ステップ1: すべてのリソースタイプの記録を確認

```bash
aws configservice describe-configuration-recorders
```

現在の Config レコーダーの設定を確認します。`allSupported: true` が設定されている場合、新しいリソースタイプは自動的に記録されます。

#### ステップ2: 特定のリソースタイプを記録する場合

```bash
aws configservice put-configuration-recorder \
  --configuration-recorder name=default,roleARN=arn:aws:iam::123456789012:role/config-role \
  --recording-group file://recording-group.json
```

`recording-group.json` の例:
```json
{
  "allSupported": false,
  "includeGlobalResourceTypes": false,
  "resourceTypes": [
    "AWS::EC2::SubnetCidrBlock",
    "AWS::S3Tables::TableBucketPolicy",
    "AWS::SageMaker::UserProfile"
  ]
}
```

特定のリソースタイプのみを記録する場合は、`resourceTypes` に追加します。

#### ステップ3: Config ルールの作成

```bash
aws configservice put-config-rule \
  --config-rule file://config-rule.json
```

`config-rule.json` の例:
```json
{
  "ConfigRuleName": "subnet-cidr-compliance",
  "Description": "Check subnet CIDR block configuration",
  "Scope": {
    "ComplianceResourceTypes": [
      "AWS::EC2::SubnetCidrBlock"
    ]
  },
  "Source": {
    "Owner": "CUSTOM_LAMBDA",
    "SourceIdentifier": "arn:aws:lambda:us-east-1:123456789012:function:check-subnet-cidr"
  }
}
```

新しいリソースタイプに対するコンプライアンスルールを作成します。

## メリット

### ビジネス面

- **コンプライアンス管理の強化**: より多くのリソースタイプを監査対象に含めることで、コンプライアンス要件を満たしやすくなる
- **ガバナンス向上**: 組織全体のリソース設定を一元管理し、ポリシー違反を早期に検出できる
- **運用効率の向上**: 設定変更の自動追跡により、手動監査の負担を軽減

### 技術面

- **可視性の向上**: 21個の新しいリソースタイプの設定変更履歴を自動的に追跡
- **自動コンプライアンスチェック**: Config ルールを使用して、新しいリソースタイプのコンプライアンスを自動評価
- **マルチアカウント管理**: Config アグリゲーターで、複数アカウント・リージョンのリソースを一元管理

## デメリット・制約事項

### 制限事項

- リソースタイプは、それぞれのサービスが利用可能なリージョンでのみサポート
- 記録するリソースタイプの数が増えると、AWS Config の料金が増加する可能性がある
- 一部のリソースタイプは、特定の AWS サービスのバージョンや機能に依存する場合がある

### 考慮すべき点

- すべてのリソースタイプを記録する (`allSupported: true`) と、記録対象リソースの数が増加し、料金に影響する可能性がある
- 必要なリソースタイプのみを選択的に記録することで、コストを最適化できる
- Config ルールの評価頻度を適切に設定し、不要な評価を避ける

## ユースケース

### ユースケース1: ネットワークセキュリティの監査

**シナリオ**: VPC サブネットの CIDR ブロック設定が組織のポリシーに準拠しているかを自動的にチェックしたい。

**実装例**:
```json
{
  "ConfigRuleName": "subnet-cidr-policy-compliance",
  "Scope": {
    "ComplianceResourceTypes": ["AWS::EC2::SubnetCidrBlock"]
  },
  "Source": {
    "Owner": "CUSTOM_LAMBDA",
    "SourceIdentifier": "arn:aws:lambda:us-east-1:123456789012:function:check-subnet-policy"
  }
}
```

**効果**: サブネット CIDR ブロックの変更を自動的に検出し、ポリシー違反を早期に発見できます。

### ユースケース2: コンテナイメージのレプリケーション管理

**シナリオ**: Amazon ECR のレプリケーション設定が、ディザスタリカバリ要件を満たしているかを監査したい。

**実装例**:
```json
{
  "ConfigRuleName": "ecr-replication-compliance",
  "Scope": {
    "ComplianceResourceTypes": ["AWS::ECR::ReplicationConfiguration"]
  },
  "Source": {
    "Owner": "CUSTOM_LAMBDA",
    "SourceIdentifier": "arn:aws:lambda:us-east-1:123456789012:function:check-ecr-replication"
  }
}
```

**効果**: ECR レプリケーション設定の変更を追跡し、マルチリージョン配布が適切に設定されていることを確認できます。

### ユースケース3: シークレット管理のガバナンス

**シナリオ**: AWS Secrets Manager のリソースポリシーが、最小権限の原則に従っているかを監査したい。

**実装例**:
```json
{
  "ConfigRuleName": "secrets-manager-policy-compliance",
  "Scope": {
    "ComplianceResourceTypes": ["AWS::SecretsManager::ResourcePolicy"]
  },
  "Source": {
    "Owner": "CUSTOM_LAMBDA",
    "SourceIdentifier": "arn:aws:lambda:us-east-1:123456789012:function:check-secrets-policy"
  }
}
```

**効果**: シークレットのアクセス制御が適切に設定されていることを自動的に検証し、セキュリティリスクを低減できます。

## 料金

AWS Config の料金は、記録する設定アイテム数および Config ルールの評価回数に基づきます:

- **設定アイテムの記録**: リージョンごとに最初の 100,000 アイテムまで $0.003/アイテム
- **Config ルールの評価**: リージョンごとに最初の 100,000 評価まで $0.001/評価

### 料金例

| 使用量 | 月額料金（概算） |
|--------|------------------|
| 10,000 設定アイテム + 10 Config ルール (月10万評価) | $30 + $100 = $130 |
| 50,000 設定アイテム + 20 Config ルール (月20万評価) | $150 + $250 = $400 |

詳細な料金については、[AWS Config 料金ページ](https://aws.amazon.com/config/pricing/) をご参照ください。

## 利用可能リージョン

新しいリソースタイプは、それぞれのリソースが利用可能なすべての AWS リージョンで AWS Config による追跡が可能です。詳細は [AWS リージョン表](https://docs.aws.amazon.com/config/latest/developerguide/what-is-resource-config-coverage.html) をご参照ください。

## 関連サービス・機能

- **AWS Config Rules**: 新しいリソースタイプに対するコンプライアンスルールを作成
- **AWS Config Aggregator**: 複数アカウント・リージョンのリソース設定を一元管理
- **AWS Security Hub**: Config の検出結果を Security Hub に統合し、セキュリティ態勢を可視化

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/aws-config-new-resource-types/)
- [AWS Config サポートリソースタイプ](https://docs.aws.amazon.com/config/latest/developerguide/what-is-resource-config-coverage.html)
- [AWS Config Developer Guide](https://docs.aws.amazon.com/config/latest/developerguide/what-is-aws-config.html)
- [AWS Config 料金ページ](https://aws.amazon.com/config/pricing/)

## まとめ

AWS Config が21個の新しいリソースタイプをサポートしたことで、AWS 環境全体のガバナンスとコンプライアンス管理が大幅に強化されました。すべてのリソースタイプの記録を有効にしている場合、これらの新しいリソースは自動的に追跡されます。組織のコンプライアンス要件に応じて、新しいリソースタイプに対する Config ルールを作成し、セキュリティ態勢の向上を検討することをお勧めします。
