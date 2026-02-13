# AWS Config - 13 個の新しいマネージドルールを追加

**リリース日**: 2026年1月22日
**サービス**: AWS Config
**機能**: 新しいマネージドルール

## 概要

AWS Config は、セキュリティ、耐久性、運用に関する 13 個の新しいマネージドルールをリリースしました。これらのルールにより、Amazon Cognito User pools、Amazon EBS Snapshots、AWS CloudFormation Stacks など、より多くの AWS リソースのコンプライアンス状態を評価できるようになります。

新しいルールは AWS Config コンソールから直接検索、有効化、管理でき、アカウント全体または AWS Organizations 全体にデプロイできます。Conformance Packs を活用することで、これらのルールをグループ化し、マルチアカウント環境での統一的なガバナンスを効率化できます。

この拡張により、組織はより包括的なセキュリティポスチャ評価を実現し、規制要件やベストプラクティスへの準拠を自動化できます。

**アップデート前の課題**

- Cognito User pools や EBS Snapshots などのリソースに対する自動コンプライアンスチェックが限定的だった
- CloudFormation スタックのセキュリティ設定を自動評価する手段が不足していた
- ECS タスク定義のセキュリティベストプラクティスを監視する機能が必要だった
- マルチアカウント環境での一貫したガバナンスの適用が困難だった

**アップデート後の改善**

- 13 個の新しいマネージドルールにより、より多くのリソースタイプのコンプライアンスを評価可能に
- Cognito、EBS、ECS、CloudFormation、CloudFront、SES などのセキュリティ設定を自動監視
- Conformance Packs により、マルチアカウントでの統一的なガバナンスが容易に
- セキュリティポスチャの可視性が大幅に向上

## サービスアップデートの詳細

### 新しいマネージドルール一覧

1. **AURORA_GLOBAL_DATABASE_ENCRYPTION_AT_REST**
   - Aurora グローバルデータベースの保存時暗号化が有効かを確認
   - データ保護とコンプライアンス要件への対応

2. **CLOUDFORMATION_STACK_SERVICE_ROLE_CHECK**
   - CloudFormation スタックに適切なサービスロールが設定されているかを確認
   - 権限管理のベストプラクティスを実施

3. **CLOUDFORMATION_TERMINATION_PROTECTION_CHECK**
   - CloudFormation スタックの削除保護が有効かを確認
   - 重要なインフラストラクチャの誤削除を防止

4. **CLOUDFRONT_DISTRIBUTION_KEY_GROUP_ENABLED**
   - CloudFront ディストリビューションでキーグループが有効かを確認
   - 署名付き URL のセキュリティ管理を強化

5. **COGNITO_USER_POOL_DELETE_PROTECTION_ENABLED**
   - Cognito User Pool の削除保護が有効かを確認
   - ユーザーデータとアイデンティティの保護

6. **COGNITO_USER_POOL_MFA_ENABLED**
   - Cognito User Pool で多要素認証 (MFA) が有効かを確認
   - アカウントセキュリティの強化

7. **COGNITO_USERPOOL_CUST_AUTH_THREAT_FULL_CHECK**
   - Cognito User Pool のカスタム認証脅威検出が完全に有効かを確認
   - 高度な脅威保護の実装

8. **EBS_SNAPSHOT_BLOCK_PUBLIC_ACCESS**
   - EBS スナップショットのパブリックアクセスがブロックされているかを確認
   - データ漏洩リスクの軽減

9. **ECS_CAPACITY_PROVIDER_TERMINATION_CHECK**
   - ECS キャパシティプロバイダーの削除保護が設定されているかを確認
   - コンテナインフラの安定性を維持

10. **ECS_TASK_DEFINITION_EFS_ENCRYPTION_ENABLED**
    - ECS タスク定義で EFS 暗号化が有効かを確認
    - コンテナデータの保存時暗号化

11. **ECS_TASK_DEFINITION_LINUX_USER_NON_ROOT**
    - ECS タスク定義で Linux ユーザーが root 以外かを確認
    - コンテナセキュリティのベストプラクティス

12. **ECS_TASK_DEFINITION_WINDOWS_USER_NON_ADMIN**
    - ECS タスク定義で Windows ユーザーが管理者以外かを確認
    - Windows コンテナのセキュリティ強化

13. **SES_SENDING_TLS_REQUIRED**
    - SES でメール送信時に TLS が必須かを確認
    - メール通信の暗号化

## 技術仕様

### ルールのカテゴリ別分類

| カテゴリ | ルール数 | 対象サービス |
|---------|---------|-------------|
| セキュリティ | 8 | Cognito, EBS, CloudFront, SES |
| 耐久性 | 3 | CloudFormation, ECS |
| 暗号化 | 3 | Aurora, ECS, SES |
| アクセス制御 | 2 | ECS |

### ルールの適用例

```json
{
  "ConfigRuleName": "cognito-user-pool-mfa-enabled",
  "Description": "Cognito User Pool で MFA が有効化されているかを確認",
  "Source": {
    "Owner": "AWS",
    "SourceIdentifier": "COGNITO_USER_POOL_MFA_ENABLED"
  },
  "Scope": {
    "ComplianceResourceTypes": [
      "AWS::Cognito::UserPool"
    ]
  }
}
```

## 設定方法

### 前提条件

1. AWS Config が有効になっていること
2. 適切な IAM 権限を持っていること
3. 評価対象のリソースが存在すること

### 手順

#### ステップ 1: 単一ルールの有効化

```bash
# AWS CLI を使用してルールを追加
aws configservice put-config-rule --config-rule '{
  "ConfigRuleName": "cognito-mfa-check",
  "Source": {
    "Owner": "AWS",
    "SourceIdentifier": "COGNITO_USER_POOL_MFA_ENABLED"
  },
  "Scope": {
    "ComplianceResourceTypes": ["AWS::Cognito::UserPool"]
  }
}'
```

このコマンドは、Cognito User Pool の MFA チェックルールを有効化します。

#### ステップ 2: Conformance Pack の作成

```yaml
# conformance-pack.yaml
Resources:
  SecurityBaseline:
    Type: AWS::Config::ConformancePack
    Properties:
      ConformancePackName: security-baseline
      TemplateBody: |
        Resources:
          CognitoMFARule:
            Type: AWS::Config::ConfigRule
            Properties:
              ConfigRuleName: cognito-mfa-enabled
              Source:
                Owner: AWS
                SourceIdentifier: COGNITO_USER_POOL_MFA_ENABLED

          EBSSnapshotPublicCheck:
            Type: AWS::Config::ConfigRule
            Properties:
              ConfigRuleName: ebs-snapshot-public-check
              Source:
                Owner: AWS
                SourceIdentifier: EBS_SNAPSHOT_BLOCK_PUBLIC_ACCESS
```

このテンプレートは、複数のルールをグループ化した Conformance Pack を定義します。

#### ステップ 3: Organizations 全体へのデプロイ

```bash
# Organizations 全体に Conformance Pack をデプロイ
aws configservice put-organization-conformance-pack \
  --organization-conformance-pack-name security-baseline \
  --template-s3-uri s3://my-bucket/conformance-pack.yaml
```

このコマンドは、AWS Organizations 全体に Conformance Pack をデプロイします。

## メリット

### ビジネス面

- **コンプライアンス自動化**: 規制要件への準拠を自動的に監視し、監査コストを削減
- **リスク軽減**: セキュリティ設定ミスを早期に検出し、データ漏洩リスクを低減
- **運用効率化**: マルチアカウント環境での一貫したガバナンスを実現

### 技術面

- **包括的な評価**: より多くのリソースタイプのセキュリティ状態を監視
- **自動修復**: AWS Systems Manager Automation と連携し、非準拠リソースを自動修復
- **一元管理**: Conformance Packs により、複数のルールを統一的に管理

## デメリット・制約事項

### 制限事項

- 各ルールには AWS Config の評価コストが発生する
- リージョンごとに Config ルールを有効化する必要がある
- 一部のルールは特定のリソースタイプにのみ適用される

### 考慮すべき点

- ルールの数が増えると、AWS Config のコストも増加する
- 既存リソースの評価結果が非準拠の場合、修復作業が必要になる
- Conformance Pack のテンプレート管理が複雑になる可能性がある

## ユースケース

### ユースケース 1: 金融機関のコンプライアンス監視

**シナリオ**: 金融機関が GDPR や PCI DSS などの規制要件に準拠する必要がある。

**実装例**:
```bash
# セキュリティ関連ルールをまとめて有効化
aws configservice put-config-rule --config-rule file://cognito-mfa.json
aws configservice put-config-rule --config-rule file://ebs-snapshot-public.json
aws configservice put-config-rule --config-rule file://ses-tls-required.json
```

**効果**: 暗号化、MFA、アクセス制御などのセキュリティ要件を自動的に監視し、コンプライアンス監査を効率化できます。

### ユースケース 2: マルチアカウント環境でのセキュリティベースライン

**シナリオ**: 複数の AWS アカウントで一貫したセキュリティベースラインを適用したい。

**実装例**:
```yaml
# security-baseline-pack.yaml
Resources:
  CognitoSecurity:
    Type: AWS::Config::ConfigRule
    Properties:
      Source:
        Owner: AWS
        SourceIdentifier: COGNITO_USER_POOL_MFA_ENABLED

  CloudFormationProtection:
    Type: AWS::Config::ConfigRule
    Properties:
      Source:
        Owner: AWS
        SourceIdentifier: CLOUDFORMATION_TERMINATION_PROTECTION_CHECK

  ECSTaskSecurity:
    Type: AWS::Config::ConfigRule
    Properties:
      Source:
        Owner: AWS
        SourceIdentifier: ECS_TASK_DEFINITION_LINUX_USER_NON_ROOT
```

**効果**: Organizations 全体で統一されたセキュリティポリシーを適用し、管理オーバーヘッドを削減できます。

### ユースケース 3: コンテナワークロードのセキュリティ強化

**シナリオ**: ECS で実行しているコンテナアプリケーションのセキュリティを強化したい。

**実装例**:
```bash
# ECS 関連のセキュリティルールを有効化
aws configservice put-config-rule --config-rule '{
  "ConfigRuleName": "ecs-task-definition-user-non-root",
  "Source": {
    "Owner": "AWS",
    "SourceIdentifier": "ECS_TASK_DEFINITION_LINUX_USER_NON_ROOT"
  }
}'

aws configservice put-config-rule --config-rule '{
  "ConfigRuleName": "ecs-task-definition-efs-encryption",
  "Source": {
    "Owner": "AWS",
    "SourceIdentifier": "ECS_TASK_DEFINITION_EFS_ENCRYPTION_ENABLED"
  }
}'
```

**効果**: コンテナが root ユーザーで実行されていないか、EFS が暗号化されているかを自動監視し、セキュリティベストプラクティスを実施できます。

## 料金

AWS Config の料金は、記録される設定項目数とルールの評価数に基づきます。

### 料金例

| 項目 | 月額料金（概算） |
|------|------------------|
| 設定項目の記録 | 最初の 1,000 項目: $0.003/項目 |
| ルール評価 | 最初の 100,000 評価: $0.001/評価 |
| Conformance Pack 評価 | $0.0012/評価 |

*注: 正確な料金は [AWS Config 料金ページ](https://aws.amazon.com/config/pricing/) を参照してください。

## 利用可能リージョン

これらの新しいルールは、AWS Config がサポートされている全リージョンで利用可能です。各ルールの詳細なリージョン対応状況は、[Config マネージドルールのドキュメント](https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) を参照してください。

## 関連サービス・機能

- **AWS Security Hub**: Config の評価結果を統合し、包括的なセキュリティポスチャを可視化
- **AWS Systems Manager Automation**: 非準拠リソースの自動修復
- **AWS Organizations**: マルチアカウント環境での Conformance Pack の一括デプロイ

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/aws-config-launches-new-rules/)
- [AWS Config 開発者ガイド](https://docs.aws.amazon.com/config/latest/developerguide/DocumentHistory.html)
- [Config マネージドルールのドキュメント](https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
- [Config ルールの使用方法](https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_add-rules.html)

## まとめ

AWS Config の 13 個の新しいマネージドルールにより、Cognito、EBS、CloudFormation、ECS などのサービスに対する包括的なコンプライアンス監視が可能になりました。特に Conformance Packs と組み合わせることで、マルチアカウント環境での一貫したガバナンスを効率的に実現できます。セキュリティ要件が厳しい組織や、複雑なマルチアカウント環境を管理している組織は、これらの新しいルールを積極的に活用し、自動化されたコンプライアンス監視を実装することをお勧めします。
