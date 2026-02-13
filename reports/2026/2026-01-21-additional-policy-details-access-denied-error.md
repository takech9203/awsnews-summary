# AWS - アクセス拒否エラーメッセージの詳細化

**リリース日**: 2026 年 1 月 21 日
**サービス**: AWS IAM, AWS Organizations
**機能**: アクセス拒否エラーメッセージへのポリシー ARN の追加

## 概要

AWS が、同一アカウントおよび同一組織内のシナリオにおいて、アクセス拒否エラーメッセージに AWS Identity and Access Management (IAM) および AWS Organizations ポリシーの Amazon Resource Name (ARN) を含めるようになりました。これにより、アクセス拒否の原因となった正確なポリシーを迅速に特定し、問題をトラブルシューティングするためのアクションを実行できます。

このアップデートにより、Service Control Policies (SCP)、Resource Control Policies (RCP)、アイデンティティベースポリシー、セッションポリシー、アクセス許可境界のポリシー ARN がエラーメッセージに含まれるようになりました。

**アップデート前の課題**

- アクセス拒否エラーメッセージには、ポリシータイプのみが含まれており、どのポリシーが原因でアクセスが拒否されたのかを特定するのが困難でした
- 同じタイプのポリシーが複数存在する場合、どのポリシーを修正すべきかを特定するために追加の調査が必要でした
- トラブルシューティングに時間がかかり、問題解決が遅れることがありました

**アップデート後の改善**

- アクセス拒否エラーメッセージに、アクセスを拒否したポリシーの ARN が含まれるようになりました
- 同じタイプのポリシーが複数ある場合でも、どのポリシーを修正すべきかが明確になりました
- 明示的な拒否ケースに対して、直接どのポリシーに対処すべきかがわかるようになりました

## サービスアップデートの詳細

### 主要機能

1. **ポリシー ARN の追加**
   - Service Control Policies (SCP)
   - Resource Control Policies (RCP)
   - アイデンティティベースポリシー
   - セッションポリシー
   - アクセス許可境界 (Permission Boundaries)

2. **同一アカウント・同一組織内での適用**
   - 同一 AWS アカウント内でのアクセス拒否エラーに適用
   - 同一 AWS Organizations 組織内でのアクセス拒否エラーに適用

3. **段階的な展開**
   - すべての AWS サービスおよび AWS リージョンで段階的に利用可能になります

## 技術仕様

### エラーメッセージの形式

**アップデート前のエラーメッセージ:**
```
User: arn:aws:iam::123456789012:user/example-user is not authorized to perform:
s3:PutObject on resource: arn:aws:s3:::example-bucket/* because no identity-based
policy allows the s3:PutObject action
```

**アップデート後のエラーメッセージ:**
```
User: arn:aws:iam::123456789012:user/example-user is not authorized to perform:
s3:PutObject on resource: arn:aws:s3:::example-bucket/* with an explicit deny in
an identity-based policy: arn:aws:iam::123456789012:policy/DenyS3PutObject
```

### ポリシータイプと ARN の例

| ポリシータイプ | ARN の例 |
|----------------|----------|
| アイデンティティベースポリシー | arn:aws:iam::123456789012:policy/ExamplePolicy |
| Service Control Policy (SCP) | arn:aws:organizations::123456789012:policy/o-exampleorg/service_control_policy/p-examplepolicy |
| Resource Control Policy (RCP) | arn:aws:organizations::123456789012:policy/o-exampleorg/resource_control_policy/p-examplepolicy |
| セッションポリシー | arn:aws:iam::123456789012:policy/SessionPolicy |
| アクセス許可境界 | arn:aws:iam::123456789012:policy/PermissionBoundary |

## 設定方法

### 前提条件

この機能は自動的に有効になり、設定は不要です。AWS サービスによって段階的に展開されます。

### トラブルシューティング手順

#### ステップ 1: エラーメッセージの確認

アクセス拒否エラーが発生した場合、エラーメッセージを確認してポリシー ARN を特定します。

```bash
# AWS CLI を使用して操作を実行
aws s3 cp example.txt s3://example-bucket/

# エラーメッセージの例
# An error occurred (AccessDenied) when calling the PutObject operation:
# User: arn:aws:iam::123456789012:user/example-user is not authorized to
# perform: s3:PutObject on resource: arn:aws:s3:::example-bucket/* with
# an explicit deny in an identity-based policy:
# arn:aws:iam::123456789012:policy/DenyS3PutObject
```

#### ステップ 2: ポリシーの確認

エラーメッセージに含まれるポリシー ARN を使用して、該当するポリシーを確認します。

```bash
# IAM ポリシーの詳細を取得
aws iam get-policy --policy-arn arn:aws:iam::123456789012:policy/DenyS3PutObject

# ポリシーバージョンの詳細を取得
aws iam get-policy-version \
  --policy-arn arn:aws:iam::123456789012:policy/DenyS3PutObject \
  --version-id v1
```

このコマンドは、指定されたポリシーの詳細を取得し、どのアクションが拒否されているかを確認できます。

#### ステップ 3: ポリシーの修正

必要に応じて、ポリシーを修正してアクセスを許可します。

```bash
# ポリシーのアタッチを解除
aws iam detach-user-policy \
  --user-name example-user \
  --policy-arn arn:aws:iam::123456789012:policy/DenyS3PutObject

# または、ポリシーの内容を修正
aws iam create-policy-version \
  --policy-arn arn:aws:iam::123456789012:policy/DenyS3PutObject \
  --policy-document file://updated-policy.json \
  --set-as-default
```

## メリット

### ビジネス面

- **トラブルシューティング時間の短縮**: 問題の原因となったポリシーを迅速に特定できます
- **ダウンタイムの削減**: アクセス拒否エラーを迅速に解決し、ダウンタイムを最小限に抑えます
- **運用効率の向上**: トラブルシューティングプロセスが効率化され、運用コストが削減されます

### 技術面

- **詳細なエラー情報**: ポリシー ARN により、どのポリシーが問題の原因かが明確になります
- **複数ポリシーへの対応**: 同じタイプのポリシーが複数ある場合でも、正確に特定できます
- **効率的なデバッグ**: ポリシーの ARN から直接ポリシーにアクセスして確認できます

## デメリット・制約事項

### 制限事項

- クロスアカウントやクロス組織のシナリオでは、ポリシー ARN が含まれない場合があります
- 一部の AWS サービスでは、この機能がまだサポートされていない場合があります
- 段階的な展開のため、すべてのサービスで即座に利用できるわけではありません

### 考慮すべき点

- エラーメッセージに含まれるポリシー ARN は、明示的な拒否ケースに対してのみ表示されます
- 暗黙的な拒否 (許可がない場合) では、ポリシー ARN が表示されない場合があります
- エラーメッセージの形式は、AWS サービスによって異なる場合があります

## ユースケース

### ユースケース 1: 複数の IAM ポリシーがアタッチされている場合

**シナリオ**: ユーザーに複数の IAM ポリシーがアタッチされており、S3 バケットへのアクセスが拒否されている。どのポリシーが原因かを特定する必要がある。

**実装例**:
1. S3 バケットへのアクセスを試みる
2. エラーメッセージを確認して、ポリシー ARN を特定
3. 該当するポリシーを確認して、拒否ルールを特定
4. ポリシーを修正またはデタッチして、アクセスを許可

**効果**: 複数のポリシーの中から問題のポリシーを迅速に特定し、トラブルシューティング時間を短縮できます。

### ユースケース 2: Service Control Policy (SCP) による制限

**シナリオ**: AWS Organizations の SCP により、特定のサービスへのアクセスが拒否されている。

**実装例**:
1. サービスへのアクセスを試みる
2. エラーメッセージを確認して、SCP の ARN を特定
3. Organizations コンソールで該当する SCP を確認
4. SCP を修正またはデタッチして、アクセスを許可

**効果**: 組織レベルのポリシーを迅速に特定し、適切な対処を行えます。

### ユースケース 3: Permission Boundary による制限

**シナリオ**: ユーザーに Permission Boundary が設定されており、特定のアクションが拒否されている。

**実装例**:
1. アクションを実行しようとする
2. エラーメッセージを確認して、Permission Boundary の ARN を特定
3. Permission Boundary を確認して、制限を理解
4. Permission Boundary を修正するか、別の方法でアクセスを許可

**効果**: Permission Boundary による制限を迅速に特定し、適切な対処を行えます。

## 料金

この機能に追加料金はかかりません。

## 利用可能リージョン

この機能は、すべての AWS リージョンで段階的に利用可能になります。

## 関連サービス・機能

- **AWS IAM**: ユーザー、グループ、ロールのアクセス制御を管理するサービス
- **AWS Organizations**: 複数の AWS アカウントを一元管理するサービス
- **AWS CloudTrail**: AWS API 呼び出しのログを記録するサービス
- **IAM Access Analyzer**: IAM ポリシーを分析してセキュリティリスクを特定するサービス

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/additional-policy-details-access-denied-error/)
- [Troubleshoot access denied error messages ドキュメント](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_access-denied.html)
- [IAM documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/)

## まとめ

AWS がアクセス拒否エラーメッセージにポリシー ARN を含めるようになったことで、トラブルシューティングが大幅に効率化されました。複数のポリシーが存在する場合でも、どのポリシーが問題の原因かを迅速に特定し、適切な対処を行えます。IAM ポリシーや Organizations のポリシーを管理している組織は、この機能を活用してトラブルシューティング時間を短縮し、運用効率を向上させることをお勧めします。
