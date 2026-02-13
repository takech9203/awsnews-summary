# AWS Billing Conductor - カスタムラインアイテム機能強化

**リリース日**: 2025年12月17日
**サービス**: AWS Billing Conductor
**機能**: サービス固有カスタムラインアイテム

## 概要

AWS Billing Conductor は、カスタムラインアイテムの機能を強化しました。この更新により、1 つの AWS サービスまたは選択した複数の AWS サービスにスコープを設定したサービス固有のカスタムラインアイテムを作成できるようになりました。また、これらのラインアイテムを請求書ページ、Cost Explorer、Cost and Usage Records などのプロフォーマ請求アーティファクトでどのように表示するかを選択できます。

この機能強化により、より精密でカスタマイズされたチャージバックおよび再請求戦略を作成でき、価格構造をより正確に反映し、プロフォーマユーザーのトレーサビリティ体験を向上させることができます。

**アップデート前の課題**

- カスタムラインアイテムは特定のサービスにスコープを設定できなかった
- 請求アーティファクトでの表示方法をカスタマイズできなかった
- Savings Plans 料金への割引適用や共有サポート料金の配分が困難だった

**アップデート後の改善**

- 1 つまたは複数の AWS サービスにスコープを設定したカスタムラインアイテムを作成可能
- 請求アーティファクトでの表示方法 (明細化または統合) を選択可能
- Savings Plans 料金への割引適用や AWS Support サービスへの共有料金配分が可能

## サービスアップデートの詳細

### 主要機能

1. **サービス固有スコープ**
   - 単一の AWS サービスにスコープを設定
   - 複数の AWS サービスを選択してスコープを設定
   - 柔軟なチャージバック戦略の実現

2. **表示設定オプション**
   - 明細化表示: 各サービスごとに個別に表示
   - 統合表示: 選択したサービスの下に統合して表示
   - プロフォーマ請求アーティファクト全体で一貫した表示

3. **対応する請求グループ**
   - 標準請求グループ: すべての価格プランタイプで利用可能
   - 請求転送請求グループ: カスタマー管理価格プラン選択時のみ利用可能

## 技術仕様

### カスタムラインアイテムの設定オプション

| 設定項目 | 説明 | オプション |
|---------|------|-----------|
| コスト参照値 | 対象となる AWS サービス | 単一または複数のサービス |
| 表示設定 | 請求アーティファクトでの表示方法 | 明細化 / 統合 |
| 料金タイプ | 適用する料金の種類 | パーセンテージ / 固定額 |

### 対応する請求アーティファクト

| アーティファクト | 説明 |
|-----------------|------|
| Bills Page | AWS 請求書ページ |
| Cost Explorer | コスト分析ツール |
| Cost and Usage Records | 詳細なコストと使用状況レポート |

## 設定方法

### 前提条件

1. AWS Billing Conductor が有効化されていること
2. 請求グループが作成済みであること
3. 適切な IAM 権限

### 手順

#### ステップ 1: AWS Billing Conductor コンソールへのアクセス

AWS マネジメントコンソールから Billing Conductor にアクセスします。

```bash
# AWS CLI でカスタムラインアイテムを作成する場合
aws billingconductor create-custom-line-item \
  --name "EC2-Discount" \
  --description "EC2 usage discount for enterprise customers" \
  --billing-group-arn "arn:aws:billingconductor::123456789012:billinggroup/my-billing-group" \
  --billing-period-range StartMonth=2025-12,EndMonth=2025-12 \
  --charge-details '{
    "Type": "PERCENTAGE",
    "Percentage": {
      "PercentageValue": -10.0
    }
  }'
```

カスタムラインアイテムを作成し、EC2 使用料に 10% の割引を適用します。

#### ステップ 2: サービス固有スコープの設定

特定の AWS サービスにスコープを設定します。

```bash
aws billingconductor create-custom-line-item \
  --name "Savings-Plans-Adjustment" \
  --description "Savings Plans fee adjustment" \
  --billing-group-arn "arn:aws:billingconductor::123456789012:billinggroup/my-billing-group" \
  --billing-period-range StartMonth=2025-12,EndMonth=2025-12 \
  --charge-details '{
    "Type": "PERCENTAGE",
    "Percentage": {
      "PercentageValue": -5.0,
      "AssociatedValues": ["AmazonEC2", "AWSLambda"]
    }
  }'
```

EC2 と Lambda の Savings Plans 料金に 5% の割引を適用します。

#### ステップ 3: 表示設定の構成

請求アーティファクトでの表示方法を設定します。

```json
{
  "customLineItem": {
    "name": "Support-Allocation",
    "description": "Shared AWS Support charges allocation",
    "chargeDetails": {
      "type": "FLAT",
      "flat": {
        "chargeValue": 500.00
      }
    },
    "displaySettings": {
      "displayMode": "CONSOLIDATED",
      "consolidatedService": "AWSSupport"
    }
  }
}
```

AWS Support サービスの下に統合して表示される固定料金を設定します。

#### ステップ 4: 請求グループへの関連付け

カスタムラインアイテムを請求グループに関連付けます。

```bash
aws billingconductor associate-custom-line-item-to-billing-group \
  --custom-line-item-arn "arn:aws:billingconductor::123456789012:customlineitem/cli-xxxxx" \
  --billing-group-arn "arn:aws:billingconductor::123456789012:billinggroup/my-billing-group"
```

## メリット

### ビジネス面

- **精密なチャージバック**: サービスごとに異なる割引率や料金を適用可能
- **透明性の向上**: プロフォーマユーザーに対する請求の可視性が向上
- **柔軟な価格戦略**: 顧客ごとにカスタマイズされた価格構造を実現

### 技術面

- **管理の簡素化**: 複数のサービスを 1 つのカスタムラインアイテムで管理
- **レポートの一貫性**: 請求アーティファクト全体で統一された表示
- **自動化対応**: API を通じた自動化が可能

## デメリット・制約事項

### 制限事項

- 中国リージョン (北京、寧夏) では利用不可
- 請求転送請求グループではカスタマー管理価格プランが必要
- 一部の設定変更は翌請求期間から反映

### 考慮すべき点

- 既存のカスタムラインアイテムからの移行計画が必要
- 複雑な価格構造では設計に時間がかかる可能性
- プロフォーマユーザーへの変更通知が必要

## ユースケース

### ユースケース 1: Savings Plans 料金への割引適用

**シナリオ**: エンタープライズ顧客に対して Savings Plans 料金に追加割引を適用

**実装例**:
```json
{
  "name": "Enterprise-SP-Discount",
  "chargeDetails": {
    "type": "PERCENTAGE",
    "percentage": {
      "percentageValue": -15.0,
      "associatedValues": ["SavingsPlans"]
    }
  },
  "displaySettings": {
    "displayMode": "ITEMIZED"
  }
}
```

**効果**: Savings Plans 料金に 15% の割引が適用され、請求書に明細として表示

### ユースケース 2: 共有サポート料金の配分

**シナリオ**: AWS Support の料金を複数の部門に均等に配分

**実装例**:
```json
{
  "name": "Support-Allocation-DeptA",
  "chargeDetails": {
    "type": "FLAT",
    "flat": {
      "chargeValue": 1000.00
    }
  },
  "displaySettings": {
    "displayMode": "CONSOLIDATED",
    "consolidatedService": "AWSSupport"
  }
}
```

**効果**: 各部門の請求書に AWS Support として統合された固定料金が表示

### ユースケース 3: マルチサービス割引パッケージ

**シナリオ**: 特定のサービス群 (コンピューティング関連) に対してパッケージ割引を適用

**実装例**:
```json
{
  "name": "Compute-Bundle-Discount",
  "chargeDetails": {
    "type": "PERCENTAGE",
    "percentage": {
      "percentageValue": -20.0,
      "associatedValues": ["AmazonEC2", "AWSLambda", "AmazonECS", "AmazonEKS"]
    }
  },
  "displaySettings": {
    "displayMode": "ITEMIZED"
  }
}
```

**効果**: EC2、Lambda、ECS、EKS の使用料に 20% の割引が適用

## 料金

AWS Billing Conductor の使用に追加料金はかかりません。標準の AWS 料金が適用されます。

## 利用可能リージョン

この機能は、AWS 中国リージョン (北京、寧夏) を除くすべての AWS 商用リージョンで利用可能です。

## 関連サービス・機能

- **AWS Cost Explorer**: コスト分析と可視化
- **AWS Cost and Usage Report**: 詳細なコストレポート
- **AWS Organizations**: マルチアカウント管理
- **AWS Budgets**: 予算管理とアラート

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/12/enhanced-custom-line-item-controls-aws-billing-conductor/)
- [ドキュメント - AWS Billing Conductor](https://docs.aws.amazon.com/billingconductor/latest/userguide/what-is-billingconductor.html)
- [AWS Billing Conductor](https://aws.amazon.com/aws-cost-management/aws-billing-conductor/)

## まとめ

AWS Billing Conductor のカスタムラインアイテム機能強化により、より精密で柔軟なチャージバックおよび再請求戦略を実現できるようになりました。サービス固有のスコープ設定と表示オプションにより、複雑な価格構造を持つ組織でも、透明性の高い請求管理が可能です。マルチテナント環境や複数部門への請求配分を行っている場合は、この機能を活用して請求管理を最適化することをお勧めします。
