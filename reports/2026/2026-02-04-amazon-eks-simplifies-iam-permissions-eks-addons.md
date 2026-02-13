# Amazon EKS - EKS Pod Identity 統合による IAM 権限管理の簡素化

**リリース日**: 2026年2月4日
**サービス**: Amazon EKS
**機能**: EKS Pod Identity と EKS add-ons の統合 (AWS GovCloud US 地域)

## 概要

Amazon EKS は AWS GovCloud (US) リージョンで、EKS Pod Identity と EKS add-ons の直接統合をサポート開始しました。これにより、EKS add-ons が AWS サービスと連携する際の IAM 権限管理が大幅に簡素化されます。EKS add-ons のライフサイクル管理が EKS コンソール、CLI、API、eksctl、AWS CloudFormation などの IaC ツール経由で統一されたアプローチで実行可能になります。

**アップデート前の課題**

- EKS add-ons が AWS サービスと連携する際、IAM 権限を別途構成する必要があった
- Pod Identity の設定とアドオン管理が分離されており、ワークフローが複雑だった
- コンソール作成時に Pod Identity 対応アドオンの選択肢が限定されていた

**アップデート後の改善**

- EKS add-ons 操作から直接 Pod Identity を管理でき、ワンステップで権限設定が完了
- 複数管理ツール(EKS コンソール、CLI、CloudFormation など)で統一された方法で設定可能
- クラスター作成時に利用可能な Pod Identity 対応 add-ons が増加

## 主要機能

1. **EKS add-ons と Pod Identity の統合**
   - add-ons 管理画面から Pod Identity 権限を直接設定可能
   - add-ons 削除時に自動的に Pod Identity も削除され、リソース漏洩を防止

2. **統一された管理インターフェース**
   - EKS コンソール、CLI、eksctl で同じ方法で設定可能
   - AWS CloudFormation による IaC 対応

3. **Pod Identity 対応 add-ons の拡充**
   - クラスター作成時に選択可能な Pod Identity 互換 add-ons が増加

## 技術仕様

| 対応リージョン | 詳細 |
|---|---|
| AWS GovCloud (US-East) | 一般公開 |
| AWS GovCloud (US-West) | 一般公開 |

## メリット

### ビジネス面

- **管理負荷削減**: IAM 権限管理の複雑性が低下
- **セキュリティ向上**: 統一されたアクセス制御で権限管理が容易

### 技術面

- **ワークフロー統一**: 複数ツール間での管理アプローチが統一される
- **自動ライフサイクル管理**: リソースの確実なクリーンアップ

## ユースケース

### ユースケース1: GovCloud 環境での EKS クラスター構築

**シナリオ**: AWS GovCloud で新しい EKS クラスターを立ち上げ、コア add-ons(CNI、監視、ロギング等)を IAM 権限付きで構成する場合

**効果**: EKS コンソールから add-ons 選択時に Pod Identity 統合で一度に IAM 権限を設定でき、セットアップが高速化

### ユースケース2: CloudFormation による IaC 環境構築

**シナリオ**: 複数の GovCloud クラスターを CloudFormation で構築し、統一された Pod Identity 設定を適用する場合

**効果**: CloudFormation テンプレートで add-ons と権限を統一管理でき、運用性が向上

## 利用可能リージョン

- AWS GovCloud (US-East)
- AWS GovCloud (US-West)

## 関連サービス・機能

- **EKS Pod Identity**: Kubernetes アプリケーションに AWS IAM 権限を付与する仕組み
- **IAM**: アクセス制御管理
- **AWS CloudFormation**: Infrastructure as Code ツール

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-eks-simplifies-iam-permissions-eks-addons/)
- [EKS ユーザーガイド](https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html)

## まとめ

EKS add-ons と Pod Identity の統合により、AWS GovCloud ユーザーのクラスター管理が大幅に簡素化されます。特に IaC による環境構築や複数クラスターの管理シーンで、管理負荷が大きく軽減されます。
