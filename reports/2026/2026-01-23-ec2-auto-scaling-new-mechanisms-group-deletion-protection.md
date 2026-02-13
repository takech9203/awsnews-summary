# EC2 Auto Scaling - グループ削除保護メカニズム

**リリース日**: 2026 年 1 月 23 日
**サービス**: EC2 Auto Scaling
**機能**: グループ削除保護メカニズムの導入

## 概要

EC2 Auto Scaling は、新しいポリシー条件キー `autoscaling:ForceDelete` を導入しました。この条件キーは、DeleteAutoScalingGroup アクションと共に使用され、削除時に ForceDelete パラメータを使用できるかどうかを制御します。これにより、実行中のインスタンスがまだ含まれている Auto Scaling グループ (ASG) を削除できるかどうかが決定されます。IAM ポリシーでこの条件キーを使用して削除権限を制限できます。これにより、実行中のインスタンスがまだ含まれている ASG の誤った削除を防ぐセーフティ対策が提供されます。

さらに、EC2 Auto Scaling は、グループレベルでの削除保護を提供するようになりました。新しい削除保護設定は、ASG の作成時または更新時に設定できます。この新機能により、ワークロードの重要度に基づいて強化された制御を設定でき、誤った削除から保護し、アプリケーションの可用性を維持するために複数の保護レベルが利用可能です。

**アップデート前の課題**

- 実行中のインスタンスを含む ASG が誤って削除される可能性があった
- IAM ポリシーで強制削除操作を制限するメカニズムが限定的だった
- 重要なワークロードに対する削除保護が不十分だった

**アップデート後の改善**

- `autoscaling:ForceDelete` 条件キーで IAM ポリシーレベルで削除を制限可能になった
- グループレベルでの削除保護設定により、重要な ASG を保護できるようになった
- 多層防御により、意図しない ASG の終了を防止できるようになった

## サービスアップデートの詳細

### 主要機能

1. **autoscaling:ForceDelete 条件キー**
   - IAM ポリシーで ForceDelete パラメータの使用を制御
   - DeleteAutoScalingGroup アクションと共に使用
   - 実行中のインスタンスを含む ASG の削除を制限

2. **グループレベルの削除保護**
   - ASG の作成時または更新時に設定可能
   - ワークロードの重要度に基づいて保護レベルを設定
   - 誤った削除からアプリケーションの可用性を維持

3. **多層防御**
   - IAM ポリシーとグループレベルの保護を組み合わせ
   - 強制削除操作を制限し、重要な ASG に保護制御を設定
   - 意図しない ASG 終了に対する包括的な防御

## 技術仕様

### autoscaling:ForceDelete 条件キー

| 項目 | 詳細 |
|------|------|
| 条件キー | `autoscaling:ForceDelete` |
| 対応アクション | DeleteAutoScalingGroup |
| 用途 | ForceDelete パラメータの使用を制御 |

### IAM ポリシー例

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "autoscaling:DeleteAutoScalingGroup",
      "Resource": "*",
      "Condition": {
        "Bool": {
          "autoscaling:ForceDelete": "true"
        }
      }
    }
  ]
}
```

このポリシーは、ForceDelete パラメータを使用した ASG の削除を拒否します。

### 削除保護の設定

```bash
# ASG 作成時に削除保護を設定
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name my-asg \
  --min-size 1 \
  --max-size 5 \
  --desired-capacity 2 \
  --deletion-protection Enabled

# 既存 ASG に削除保護を設定
aws autoscaling update-auto-scaling-group \
  --auto-scaling-group-name my-asg \
  --deletion-protection Enabled
```

## 設定方法

### 前提条件

1. EC2 Auto Scaling グループの作成権限
2. IAM ポリシーの設定権限
3. AWS CLI または AWS SDK のインストール

### 手順

#### ステップ 1: IAM ポリシーでの制限

IAM ポリシーで `autoscaling:ForceDelete` 条件キーを使用して、強制削除を制限します。

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "autoscaling:DeleteAutoScalingGroup",
      "Resource": "arn:aws:autoscaling:*:*:autoScalingGroup:*:autoScalingGroupName/critical-*",
      "Condition": {
        "Bool": {
          "autoscaling:ForceDelete": "true"
        }
      }
    }
  ]
}
```

#### ステップ 2: グループレベルの削除保護設定

重要な ASG に削除保護を設定します。

```bash
aws autoscaling update-auto-scaling-group \
  --auto-scaling-group-name critical-app-asg \
  --deletion-protection Enabled
```

#### ステップ 3: 削除保護の確認

削除保護が有効になっていることを確認します。

```bash
aws autoscaling describe-auto-scaling-groups \
  --auto-scaling-group-names critical-app-asg \
  --query 'AutoScalingGroups[0].DeletionProtection'
```

## メリット

### ビジネス面

- **アプリケーションの可用性維持**: 誤った削除から重要なワークロードを保護
- **ダウンタイムの削減**: 意図しない ASG 削除によるサービス中断を防止
- **コンプライアンス**: 削除操作に対する制御とログ記録を強化

### 技術面

- **多層防御**: IAM ポリシーとグループレベルの保護を組み合わせた包括的な防御
- **柔軟な制御**: ワークロードの重要度に基づいて保護レベルを設定
- **運用の安全性**: 誤操作による重大な障害を防止

## デメリット・制約事項

### 制限事項

- 削除保護を有効にした ASG は、保護を無効にしてから削除する必要がある
- IAM ポリシーの設定が複雑になる可能性がある

### 考慮すべき点

- 削除保護を有効にする ASG を適切に選択する必要がある
- IAM ポリシーと削除保護の組み合わせを理解し、適切に設定する
- 削除が必要な場合の手順を明確にしておく

## ユースケース

### ユースケース 1: 本番環境の ASG 保護

**シナリオ**: 本番環境の重要な ASG を誤った削除から保護する。

**実装例**:
```bash
aws autoscaling update-auto-scaling-group \
  --auto-scaling-group-name production-web-asg \
  --deletion-protection Enabled
```

**効果**: 本番環境の ASG が誤って削除されることを防止し、サービスの可用性を維持します。

### ユースケース 2: IAM ポリシーでの強制削除制限

**シナリオ**: 開発チームが強制削除を実行できないよう IAM ポリシーで制限する。

**実装例**:
```json
{
  "Effect": "Deny",
  "Action": "autoscaling:DeleteAutoScalingGroup",
  "Resource": "*",
  "Condition": {
    "Bool": {
      "autoscaling:ForceDelete": "true"
    }
  }
}
```

**効果**: 実行中のインスタンスを含む ASG の強制削除を防止し、誤操作によるサービス中断を回避します。

### ユースケース 3: 多層防御の実装

**シナリオ**: IAM ポリシーとグループレベルの削除保護を組み合わせて、重要な ASG を包括的に保護する。

**実装例**:
```bash
# IAM ポリシーで強制削除を制限
# (上記のポリシーを適用)

# グループレベルの削除保護を設定
aws autoscaling update-auto-scaling-group \
  --auto-scaling-group-name critical-database-asg \
  --deletion-protection Enabled
```

**効果**: 複数の層で保護することで、重大な障害を防止し、アプリケーションの可用性を最大限に維持します。

## 料金

この機能に追加料金はかかりません。EC2 Auto Scaling の料金は変更されません。

## 利用可能リージョン

この機能は、すべての AWS リージョンおよび AWS GovCloud (US) リージョンで利用可能です。

## 関連サービス・機能

- **AWS IAM**: アクセス制御とポリシー管理
- **Amazon EC2**: Auto Scaling で管理されるインスタンス
- **AWS CloudTrail**: Auto Scaling の操作ログを記録

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/ec2-auto-scaling-new-mechanisms-group-deletion-protection)
- [削除保護ドキュメント](https://docs.aws.amazon.com/autoscaling/ec2/userguide/resource-deletion-protection.html)
- [ポリシー条件キー](https://docs.aws.amazon.com/autoscaling/ec2/userguide/control-access-using-iam.html#policy-auto-scaling-condition-keys)

## まとめ

EC2 Auto Scaling の新しい削除保護メカニズムにより、IAM ポリシーレベルとグループレベルの両方で ASG を保護できるようになりました。`autoscaling:ForceDelete` 条件キーと削除保護設定を組み合わせることで、誤った削除から重要なワークロードを守り、アプリケーションの可用性を維持できます。本番環境で Auto Scaling を使用している場合は、この新機能を活用して、運用の安全性を強化することをお勧めします。
