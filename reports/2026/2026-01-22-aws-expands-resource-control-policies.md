# AWS Resource Control Policies - Cognito と CloudWatch Logs のサポート拡大

**リリース日**: 2026年01月22日
**サービス**: AWS Organizations
**機能**: Resource Control Policies (RCP) の Amazon Cognito と Amazon CloudWatch Logs へのサポート拡大

## 概要

AWS Resource Control Policies (RCP) が、Amazon Cognito および Amazon CloudWatch Logs のサポートを追加しました。RCP は、組織内の権限を管理するために使用できる組織ポリシーの一種です。RCP を使用すると、組織内のリソースに対する最大利用可能権限を一元的に制御できます。

この拡張により、Amazon Cognito および Amazon CloudWatch Logs リソースに対する権限を RCP で管理できるようになりました。たとえば、組織外の ID がこれらのリソースにアクセスできないようにするポリシーを作成し、データペリメーター (data perimeter) を構築して AWS 環境全体でベースラインセキュリティ標準を適用できます。

**アップデート前の課題**

- Amazon Cognito および CloudWatch Logs リソースに対する組織レベルの権限制御が困難だった
- これらのサービスに対するデータペリメーターの構築が不完全だった
- 組織外の ID によるアクセスを一元的に制限できなかった

**アップデート後の改善**

- RCP を使用して Cognito および CloudWatch Logs リソースへのアクセスを一元管理できるようになった
- 組織外の ID によるアクセスを防止するポリシーを作成可能になった
- データペリメーターを強化し、ベースラインセキュリティ標準を組織全体に適用できるようになった

## サービスアップデートの詳細

### 主要機能

1. **Amazon Cognito のサポート**
   - Cognito ユーザープール、ID プール、およびその他の Cognito リソースに対する RCP 適用が可能
   - 組織外の ID による Cognito リソースへのアクセスを防止
   - Cognito リソースの権限を組織レベルで一元管理

2. **Amazon CloudWatch Logs のサポート**
   - CloudWatch Logs ロググループ、ログストリーム、およびその他のログリソースに対する RCP 適用が可能
   - 組織外の ID によるログデータへのアクセスを防止
   - ログリソースの権限を組織レベルで一元管理

3. **データペリメーターの構築**
   - 組織外の ID がリソースにアクセスできないようにするポリシーを作成
   - AWS 環境全体でベースラインセキュリティ標準を適用
   - 一元的な権限管理によりセキュリティコンプライアンスを強化

4. **組織レベルの権限制御**
   - 組織内のすべてのアカウントに対して一貫した権限ポリシーを適用
   - 最大利用可能権限を組織レベルで制御
   - 複数のアカウントにまたがる権限管理を簡素化

## 技術仕様

### Resource Control Policies (RCP) とは

RCP は、AWS Organizations で使用できる組織ポリシーの一種で、以下の特徴があります:

| 項目 | 詳細 |
|------|------|
| 適用範囲 | 組織内のリソースに対する最大利用可能権限を制御 |
| ポリシータイプ | 拒否ベースのポリシー (最大権限を制限) |
| 適用レベル | 組織ルート、組織単位 (OU)、または個別アカウント |
| 主なユースケース | データペリメーターの構築、ベースラインセキュリティの適用 |

### サポート対象サービス

今回の拡張により、以下のサービスが RCP でサポートされます:
- **Amazon Cognito**: ユーザープール、ID プール、その他の Cognito リソース
- **Amazon CloudWatch Logs**: ロググループ、ログストリーム、その他のログリソース

その他のサポート対象サービスの完全なリストは、[RCP documentation](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_rcps.html) を参照してください。

### ポリシー例

以下は、組織外の ID が Cognito リソースにアクセスできないようにする RCP の例です:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Principal": "*",
      "Action": "cognito-idp:*",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:PrincipalOrgID": "${aws:PrincipalOrgID}"
        }
      }
    }
  ]
}
```

## 設定方法

### 前提条件

1. AWS Organizations が有効化されていること
2. 組織の管理アカウントまたは委任管理者アカウントへのアクセス
3. RCP を有効化するための適切な IAM 権限

### 手順

#### ステップ1: AWS Organizations コンソールにアクセス

AWS Organizations コンソールにサインインします。

#### ステップ2: Resource Control Policies の有効化

Organizations の設定から、Resource Control Policies を有効化します (まだ有効化されていない場合)。

#### ステップ3: RCP ポリシーの作成

Cognito または CloudWatch Logs リソースに対する権限を制御する RCP ポリシーを作成します。

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Principal": "*",
      "Action": [
        "cognito-idp:*",
        "logs:*"
      ],
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:PrincipalOrgID": "o-xxxxxxxxxx"
        }
      }
    }
  ]
}
```

#### ステップ4: ポリシーの適用

作成した RCP ポリシーを、組織ルート、OU、または個別アカウントに適用します。

#### ステップ5: ポリシーの検証

テストアカウントで、組織外の ID が Cognito および CloudWatch Logs リソースにアクセスできないことを確認します。

## メリット

### ビジネス面

- **セキュリティコンプライアンスの強化**: 組織レベルでベースラインセキュリティ標準を適用
- **運用効率の向上**: 複数のアカウントにまたがる権限管理を一元化
- **リスクの削減**: 組織外の ID によるアクセスを防止し、データ漏洩リスクを低減

### 技術面

- **一元的な権限管理**: 組織レベルで権限を制御し、管理を簡素化
- **データペリメーターの構築**: 組織外の ID によるアクセスを体系的に防止
- **柔軟な適用範囲**: 組織ルート、OU、個別アカウントに柔軟に適用可能

## デメリット・制約事項

### 制限事項

- RCP は拒否ベースのポリシーであり、許可を追加することはできない
- RCP は IAM ポリシーや SCP (Service Control Policies) と組み合わせて評価される
- 過度に制限的な RCP は、正当なアクセスもブロックする可能性がある

### 考慮すべき点

- RCP を適用する前に、影響を受けるリソースとアクセスパターンを十分に理解する必要がある
- テスト環境で RCP を検証してから本番環境に適用することを推奨
- RCP、SCP、IAM ポリシーの相互作用を理解する必要がある

## ユースケース

### ユースケース1: Cognito ユーザープールのデータペリメーター

**シナリオ**: 組織外の ID が Cognito ユーザープールにアクセスできないようにする

**実装例**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Principal": "*",
      "Action": "cognito-idp:*",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:PrincipalOrgID": "o-xxxxxxxxxx"
        }
      }
    }
  ]
}
```

**効果**: 組織外の ID による Cognito ユーザープールへのアクセスを防止し、データ漏洩リスクを低減

### ユースケース2: CloudWatch Logs のアクセス制限

**シナリオ**: 組織外の ID がログデータにアクセスできないようにする

**実装例**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Principal": "*",
      "Action": "logs:*",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:PrincipalOrgID": "o-xxxxxxxxxx"
        }
      }
    }
  ]
}
```

**効果**: 組織外の ID によるログデータへのアクセスを防止し、機密情報の保護を強化

### ユースケース3: 組織全体のベースラインセキュリティ

**シナリオ**: 組織内のすべてのアカウントに対して、Cognito および CloudWatch Logs のベースラインセキュリティポリシーを適用

**実装例**:
- RCP を組織ルートに適用し、すべてのアカウントに一貫したセキュリティポリシーを適用
- 特定の OU に対してより厳格なポリシーを追加適用

**効果**: 組織全体で一貫したセキュリティ標準を維持し、コンプライアンス要件を満たす

## 料金

AWS Resource Control Policies (RCP) の使用に追加料金は発生しません。AWS Organizations の一部として無料で利用できます。

## 利用可能リージョン

RCP は、すべての AWS 商用リージョンおよび AWS GovCloud (US) リージョンで利用可能です。

## 関連サービス・機能

- **AWS Organizations**: 複数の AWS アカウントを一元管理
- **Service Control Policies (SCP)**: 組織内のアカウントに対する最大権限を制御
- **IAM Policies**: 個別のリソースとプリンシパルに対する権限を制御
- **AWS Identity and Access Management (IAM)**: AWS リソースへのアクセスを安全に管理

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/aws-expands-resource-control-policies/)
- [Resource Control Policies (RCPs) documentation](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_rcps.html)
- [Data Perimeters on AWS](https://aws.amazon.com/identity/data-perimeters-on-aws/)
- [Amazon Cognito](https://aws.amazon.com/cognito/)
- [Amazon CloudWatch Logs](https://aws.amazon.com/cloudwatch/)

## まとめ

AWS Resource Control Policies の Amazon Cognito および Amazon CloudWatch Logs へのサポート拡大により、組織レベルでのセキュリティ管理がさらに強化されました。データペリメーターを構築し、組織外の ID によるアクセスを防止することで、AWS 環境全体でベースラインセキュリティ標準を適用できます。複数の AWS アカウントを管理している組織は、RCP を活用してセキュリティコンプライアンスを強化し、運用効率を向上させることを検討してください。
