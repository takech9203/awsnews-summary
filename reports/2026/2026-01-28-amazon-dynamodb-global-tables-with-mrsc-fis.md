# Amazon DynamoDB グローバルテーブル - AWS FIS によるレジリエンステスト対応

**リリース日**: 2026年1月28日
**サービス**: Amazon DynamoDB
**機能**: マルチリージョン強整合性グローバルテーブルの AWS Fault Injection Service 統合

## 概要

Amazon DynamoDB のマルチリージョン強整合性 (MRSC) グローバルテーブルが、AWS Fault Injection Service (FIS) によるアプリケーションレジリエンステストをサポート開始しました。この機能により、リージョン障害などの実世界の障害シナリオを MRSC グローバルテーブルに対して作成し、アプリケーションがこれらの障害にどのように応答するかを観察し、レジリエンスメカニズムを検証できます。

MRSC グローバルテーブルは、選択した AWS リージョン間で DynamoDB テーブルを自動的にレプリケートし、高速で強整合性のある読み取りおよび書き込みパフォーマンスを実現し、99.999% の可用性を提供します。FIS は、アプリケーションのパフォーマンス、可観測性、レジリエンスを向上させるための制御されたフォールトインジェクション実験を実行するフルマネージドサービスです。

**アップデート前の課題**

- MRSC グローバルテーブルに対して、リージョン障害時のアプリケーション動作をテストする手段が限られていた
- 実際の障害が発生する前に、レジリエンスメカニズムを検証することが困難だった
- リージョン間レプリケーションの一時停止時のアプリケーション動作を観察できなかった
- 監視およびリカバリプロセスを事前に調整する手段が不足していた

**アップデート後の改善**

- FIS アクションを使用して、リージョン間レプリケーションの一時停止をシミュレート可能
- リージョン障害などの実世界の障害シナリオを制御された環境で作成可能
- アプリケーションが障害にどのように応答するかを観察し、レジリエンスメカニズムを検証可能
- 監視およびリカバリプロセスを調整し、レジリエンスとアプリケーションの可用性を向上

## サービスアップデートの詳細

### 主要機能

1. **FIS アクションによる障害シミュレーション**
   - リージョン間レプリケーションの一時停止をシミュレート
   - 制御された環境でリージョン障害時のアプリケーション動作をテスト
   - 実験の範囲と期間を細かく制御可能

2. **レジリエンスメカニズムの検証**
   - アプリケーションの障害応答を観察
   - 自動フェイルオーバーメカニズムの動作を確認
   - 監視アラートとリカバリプロセスの有効性を検証

3. **MRSC グローバルテーブルの特性**
   - 99.999% の可用性 SLA
   - ゼロ RPO (Recovery Point Objective) による最高レベルのレジリエンス
   - 強整合性のある読み取りおよび書き込みパフォーマンス

## 技術仕様

### 利用可能リージョン

| リージョン | サポート |
|-----------|---------|
| US East (N. Virginia) | ✅ |
| US East (Ohio) | ✅ |
| US West (Oregon) | ✅ |
| Asia Pacific (Tokyo) | ✅ |
| Asia Pacific (Osaka) | ✅ |
| Asia Pacific (Seoul) | ✅ |
| Europe (Ireland) | ✅ |
| Europe (London) | ✅ |
| Europe (Frankfurt) | ✅ |
| Europe (Paris) | ✅ |

### FIS アクション

| アクション | 説明 |
|-----------|------|
| aws:dynamodb:pause-replication | MRSC グローバルテーブルのリージョン間レプリケーションを一時停止 |

## 設定方法

### 前提条件

1. MRSC グローバルテーブルが作成済み
2. AWS FIS へのアクセス権限
3. 適切な IAM 権限が設定されていること

### 手順

#### ステップ 1: FIS 実験テンプレートの作成

```json
{
  "description": "Test MRSC global table replication pause",
  "targets": {
    "DynamoDBTable": {
      "resourceType": "aws:dynamodb:table",
      "resourceArns": [
        "arn:aws:dynamodb:us-east-1:123456789012:table/my-global-table"
      ],
      "selectionMode": "ALL"
    }
  },
  "actions": {
    "PauseReplication": {
      "actionId": "aws:dynamodb:pause-replication",
      "parameters": {
        "duration": "PT5M"
      },
      "targets": {
        "Tables": "DynamoDBTable"
      }
    }
  },
  "stopConditions": [
    {
      "source": "aws:cloudwatch:alarm",
      "value": "arn:aws:cloudwatch:us-east-1:123456789012:alarm:my-alarm"
    }
  ]
}
```

#### ステップ 2: FIS 実験の実行

```bash
# FIS 実験を開始
aws fis start-experiment \
    --experiment-template-id template-id \
    --client-token $(uuidgen)
```

#### ステップ 3: アプリケーション動作の監視

```bash
# CloudWatch Logs でアプリケーションログを監視
aws logs tail /aws/lambda/my-function --follow

# CloudWatch メトリクスで DynamoDB パフォーマンスを監視
aws cloudwatch get-metric-statistics \
    --namespace AWS/DynamoDB \
    --metric-name UserErrors \
    --dimensions Name=TableName,Value=my-global-table \
    --start-time 2026-01-28T00:00:00Z \
    --end-time 2026-01-28T23:59:59Z \
    --period 300 \
    --statistics Sum
```

## メリット

### ビジネス面

- **事前検証**: 実際の障害が発生する前に、レジリエンスメカニズムを検証
- **リスク軽減**: 制御された環境で障害シナリオをテストし、本番環境でのリスクを軽減
- **高可用性**: 99.999% の可用性 SLA により、ビジネス継続性を確保

### 技術面

- **制御された実験**: FIS により、障害シナリオを細かく制御してテスト可能
- **レジリエンス向上**: 障害応答を観察し、監視およびリカバリプロセスを調整
- **ゼロ RPO**: MRSC グローバルテーブルはゼロ RPO を提供し、データ損失を防止

## デメリット・制約事項

### 制限事項

- MRSC グローバルテーブルが対応する 10 リージョンでのみ利用可能
- FIS 実験の実行には適切な IAM 権限が必要
- 実験中はアプリケーションのパフォーマンスに影響が出る可能性がある

### 考慮すべき点

- 実験の範囲と期間を適切に設定し、本番環境への影響を最小限に抑える
- 停止条件 (CloudWatch アラーム) を設定し、実験が予期しない影響を与えた場合に自動停止
- 実験後、アプリケーションのログとメトリクスを詳細に分析

## ユースケース

### ユースケース 1: リージョン障害時のフェイルオーバーテスト

**シナリオ**: リージョン障害時に、アプリケーションが自動的に別のリージョンにフェイルオーバーすることを検証

**実装例**:
```bash
# FIS 実験でリージョン間レプリケーションを一時停止
aws fis start-experiment --experiment-template-id template-id
```

**効果**: リージョン障害時のアプリケーション動作を観察し、自動フェイルオーバーメカニズムが正常に動作することを確認

### ユースケース 2: 監視アラートの検証

**シナリオ**: リージョン間レプリケーションの一時停止時に、監視アラートが正常にトリガーされることを検証

**実装例**:
```bash
# CloudWatch アラームを設定してリージョン間レプリケーション遅延を監視
aws cloudwatch put-metric-alarm \
    --alarm-name replication-delay-alarm \
    --metric-name ReplicationLatency \
    --namespace AWS/DynamoDB \
    --statistic Average \
    --period 300 \
    --evaluation-periods 1 \
    --threshold 1000 \
    --comparison-operator GreaterThanThreshold
```

**効果**: 監視アラートが正常にトリガーされ、運用チームが迅速に対応できることを確認

## 料金

FIS 実験の実行には、以下の料金が発生します。

- **FIS 実験料金**: アクション分数に基づく従量課金
- **DynamoDB 料金**: テーブルの読み取り・書き込みキャパシティに基づく標準的な料金

### 料金例

| 使用量 | 月額料金 (概算) |
|--------|------------------|
| 10 回の実験 (各 5 分) | $0.50 |
| DynamoDB 使用料 (標準) | 変動 |

## 利用可能リージョン

US East (N. Virginia)、US East (Ohio)、US West (Oregon)、Asia Pacific (Tokyo)、Asia Pacific (Osaka)、Asia Pacific (Seoul)、Europe (Ireland)、Europe (London)、Europe (Frankfurt)、Europe (Paris)

## 関連サービス・機能

- **AWS Fault Injection Service (FIS)**: 制御されたフォールトインジェクション実験を実行
- **Amazon CloudWatch**: アプリケーションのパフォーマンスと障害を監視
- **DynamoDB グローバルテーブル (MREC)**: マルチリージョン結果整合性モード
- **DynamoDB SLA**: 99.999% の可用性保証

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/amazon-dynamodb-global-tables-with-mrsc-fis/)
- [DynamoDB FIS アクションドキュメント](https://docs.aws.amazon.com/fis/latest/userguide/fis-actions-reference.html#dynamodb-actions-reference)
- [DynamoDB グローバルテーブルドキュメント](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/V2globaltables_HowItWorks.html)
- [DynamoDB SLA](https://aws.amazon.com/dynamodb/sla/)

## まとめ

Amazon DynamoDB の MRSC グローバルテーブルが AWS FIS によるレジリエンステストをサポートしたことにより、リージョン障害などの実世界の障害シナリオを制御された環境でテストできるようになりました。これにより、実際の障害が発生する前にレジリエンスメカニズムを検証し、監視およびリカバリプロセスを調整して、アプリケーションの可用性とビジネス継続性を向上できます。
