# AWS Config - 30 の新しいリソースタイプをサポート

**リリース日**: 2026 年 2 月 6 日
**サービス**: AWS Config
**機能**: 30 の追加リソースタイプサポート

📊 [このアップデートのインフォグラフィックを見る](https://takech9203.github.io/awsnews-summary/20260206-aws-config-new-resource-types.html)

## 概要

AWS Config が Amazon EKS、Amazon Q、AWS IoT などの主要サービスを含む 30 の追加リソースタイプをサポートしました。この拡張により、AWS 環境のカバレッジが向上し、より広範なリソースの発見、評価、監査、修復を効果的に行えるようになります。

すべてのリソースタイプの記録を有効にしている場合、AWS Config はこれらの新しい追加を自動的に追跡します。新しくサポートされたリソースタイプは、Config ルールと Config アグリゲーターでも利用可能です。

**アップデート前の課題**

- Amazon EKS Nodegroup や Amazon Q Business Application などのリソースを Config で追跡できなかった
- IoT 関連リソースの構成管理と監査が困難だった
- QuickSight のダッシュボードやデータセットをコンプライアンス監視対象にできなかった

**アップデート後の改善**

- 30 の新しいリソースタイプを自動追跡可能
- Config ルールによる自動コンプライアンスチェック対象が拡大
- Config アグリゲーターでの一元管理が可能なリソースが増加

## サービスアップデートの詳細

### 新しくサポートされたリソースタイプ

| リソースタイプ | 説明 |
|----------------|------|
| AWS::ApplicationSignals::ServiceLevelObjective | Application Signals の SLO |
| AWS::ARCZonalShift::AutoshiftObserverNotificationStatus | ARC ゾーンシフト通知ステータス |
| AWS::B2BI::Transformer | B2B 統合トランスフォーマー |
| AWS::CE::CostCategory | コストカテゴリ |
| AWS::CleanRooms::ConfiguredTable | Clean Rooms 設定テーブル |
| AWS::CleanRooms::Membership | Clean Rooms メンバーシップ |
| AWS::CodeArtifact::PackageGroup | CodeArtifact パッケージグループ |
| AWS::Connect::Prompt | Amazon Connect プロンプト |
| AWS::EKS::Nodegroup | EKS ノードグループ |
| AWS::GameLift::MatchmakingRuleSet | GameLift マッチメイキングルールセット |
| AWS::GameLift::Script | GameLift スクリプト |
| AWS::Glue::Crawler | Glue クローラー |
| AWS::InternetMonitor::Monitor | Internet Monitor |
| AWS::IoT::BillingGroup | IoT 課金グループ |
| AWS::IoT::ResourceSpecificLogging | IoT リソース固有ログ |
| AWS::IoT::SoftwarePackage | IoT ソフトウェアパッケージ |
| AWS::IoT::TopicRule | IoT トピックルール |
| AWS::IoTWireless::Destination | IoT Wireless 送信先 |
| AWS::IoTWireless::DeviceProfile | IoT Wireless デバイスプロファイル |
| AWS::IoTWireless::NetworkAnalyzerConfiguration | IoT Wireless ネットワークアナライザー設定 |
| AWS::IoTWireless::TaskDefinition | IoT Wireless タスク定義 |
| AWS::IoTWireless::WirelessGateway | IoT Wireless ゲートウェイ |
| AWS::Kinesis::ResourcePolicy | Kinesis リソースポリシー |
| AWS::PCAConnectorSCEP::Connector | PCA Connector SCEP |
| AWS::QBusiness::Application | Amazon Q Business アプリケーション |
| AWS::QuickSight::DataSet | QuickSight データセット |
| AWS::QuickSight::Dashboard | QuickSight ダッシュボード |
| AWS::Route53::DNSSEC | Route 53 DNSSEC |
| AWS::SSM::PatchBaseline | Systems Manager パッチベースライン |
| AWS::Transfer::User | Transfer Family ユーザー |

### カテゴリ別分類

| カテゴリ | リソースタイプ数 | 主なリソース |
|----------|------------------|--------------|
| IoT | 8 | TopicRule, WirelessGateway など |
| Analytics | 4 | QuickSight, Glue Crawler |
| AI/ML | 1 | Amazon Q Business |
| コンテナ | 1 | EKS Nodegroup |
| セキュリティ | 2 | Route53 DNSSEC, PCA Connector |
| その他 | 14 | Cost Category, Clean Rooms など |

## 技術仕様

### Config ルールでの利用

新しいリソースタイプは Config ルールで利用可能です。

```yaml
# CloudFormation での Config ルール例
Resources:
  EKSNodegroupCompliance:
    Type: AWS::Config::ConfigRule
    Properties:
      ConfigRuleName: eks-nodegroup-security-check
      Scope:
        ComplianceResourceTypes:
          - AWS::EKS::Nodegroup
      Source:
        Owner: CUSTOM_LAMBDA
        SourceIdentifier: !GetAtt ComplianceLambda.Arn
```

### Config アグリゲーターでの利用

組織全体でこれらのリソースを一元管理できます。

```bash
# AWS CLI で Config アグリゲーターのステータス確認
aws configservice describe-configuration-aggregator-sources-status \
    --configuration-aggregator-name my-aggregator
```

## 設定方法

### 前提条件

1. AWS Config の有効化
2. 適切な IAM 権限
3. S3 バケット (設定履歴の保存用)

### 手順

#### ステップ 1: 全リソースタイプの記録を確認

```bash
# 現在の記録設定を確認
aws configservice describe-configuration-recorder-status
```

すべてのリソースタイプの記録が有効になっている場合、新しいリソースタイプは自動的に追跡されます。

#### ステップ 2: 特定リソースタイプの記録を有効化

```bash
# 特定のリソースタイプのみ記録する場合
aws configservice put-configuration-recorder \
    --configuration-recorder name=default,roleARN=arn:aws:iam::123456789012:role/config-role \
    --recording-group '{"allSupported":false,"resourceTypes":["AWS::EKS::Nodegroup","AWS::QBusiness::Application"]}'
```

このコマンドで特定のリソースタイプのみを記録対象に設定できます。

#### ステップ 3: Config ルールの作成

新しいリソースタイプに対する Config ルールを作成して、コンプライアンスを監視します。

## メリット

### ビジネス面

- **コンプライアンス強化**: より多くのリソースタイプを監査対象に
- **リスク軽減**: 構成変更の追跡範囲拡大
- **ガバナンス向上**: 組織全体での一貫した構成管理

### 技術面

- **自動追跡**: 既存の設定で自動的に新リソースを記録
- **統合監視**: Config アグリゲーターでの一元管理
- **ルール適用**: 新リソースに対する自動コンプライアンスチェック

## デメリット・制約事項

### 制限事項

- リソースタイプによってはサポートされるリージョンが限定される
- 一部のリソースタイプは特定の Config ルールでのみ利用可能

### 考慮すべき点

- 追加のリソース記録により、Config の料金が増加する可能性
- 大量のリソースがある場合、初回の設定スナップショット作成に時間がかかる

## ユースケース

### ユースケース 1: EKS クラスターのコンプライアンス監視

**シナリオ**: EKS ノードグループの構成がセキュリティポリシーに準拠しているかを監視

**実装例**:
```yaml
# EKS Nodegroup のセキュリティチェックルール
ConfigRuleName: eks-nodegroup-ami-compliance
ComplianceResourceTypes:
  - AWS::EKS::Nodegroup
```

**効果**: EKS ノードグループの構成変更を追跡し、非準拠を自動検出

### ユースケース 2: IoT デバイス管理の監査

**シナリオ**: IoT ワイヤレスゲートウェイの構成変更を追跡

**効果**: IoT インフラの構成履歴を完全に記録し、監査に対応

### ユースケース 3: BI ダッシュボードのガバナンス

**シナリオ**: QuickSight ダッシュボードとデータセットの構成管理

**効果**: BI リソースの変更履歴を追跡し、データガバナンスを強化

## 料金

AWS Config の標準料金が適用されます。記録されるリソース数と Config ルール評価回数に応じた課金となります。

詳細は [AWS Config 料金ページ](https://aws.amazon.com/config/pricing/) を参照してください。

## 利用可能リージョン

新しくサポートされたリソースタイプは、各リソースがサポートされているすべての AWS リージョンで利用可能です。詳細は [AWS Config のリソースカバレッジ](https://docs.aws.amazon.com/config/latest/developerguide/what-is-resource-config-coverage.html) を参照してください。

## 関連サービス・機能

- **AWS Config Rules**: リソースのコンプライアンス評価
- **AWS Config Aggregator**: マルチアカウント・マルチリージョンの一元管理
- **AWS Security Hub**: セキュリティ検出結果の一元管理

## 参考リンク

- 📊 [インフォグラフィック](https://takech9203.github.io/awsnews-summary/20260206-aws-config-new-resource-types.html)
- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/aws-config-new-resource-types/)
- [AWS Config リソースカバレッジ](https://docs.aws.amazon.com/config/latest/developerguide/what-is-resource-config-coverage.html)
- [AWS Config 料金](https://aws.amazon.com/config/pricing/)

## まとめ

AWS Config の 30 の新しいリソースタイプサポートにより、EKS、IoT、QuickSight、Amazon Q などの重要なサービスの構成管理とコンプライアンス監視が可能になりました。特に IoT 関連リソースの追加は、IoT ソリューションを運用している組織にとって大きな価値があります。既存の Config 設定を確認し、新しいリソースタイプの監視を開始することを推奨します。
