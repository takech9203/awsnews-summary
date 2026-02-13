# Amazon Bedrock - Reserved Tier の Claude Sonnet 4.5 GovCloud 対応

**リリース日**: 2026 年 1 月 21 日
**サービス**: Amazon Bedrock
**機能**: Reserved Tier の Claude Sonnet 4.5 GovCloud (US-West) 対応

## 概要

Amazon Bedrock が、予測可能なパフォーマンスと保証されたトークン/分 (tokens-per-minute) 容量を必要とするワークロード向けに設計された Reserved サービス Tier を AWS GovCloud (US-West) リージョンの Anthropic Claude Sonnet 4.5 に拡大しました。Reserved Tier は、ミッションクリティカルなアプリケーションのためにサービスレベルを予測可能に保つための優先的なコンピュート容量を予約する機能を提供します。

入力トークン/分と出力トークン/分の容量を個別に割り当てる柔軟性により、ワークロードの正確な要件に合わせてコストを制御できます。これは、多くのワークロードが非対称なトークン使用パターンを持つため、特に価値があります。予約した容量を超えてアプリケーションがトークン/分を必要とする場合、サービスは自動的に従量課金制の Standard Tier にオーバーフローし、中断のない運用を保証します。

**アップデート前の課題**

- AWS GovCloud (US-West) リージョンで予測可能なパフォーマンスと保証された容量が必要なミッションクリティカルなアプリケーションに対応できませんでした
- トークン使用量のピーク時にパフォーマンスが低下する可能性がありました
- 非対称なトークン使用パターン (要約タスクや コンテンツ生成など) に対して、コストを最適化する柔軟性が不足していました

**アップデート後の改善**

- AWS GovCloud (US-West) リージョンで Reserved Tier を使用して、予測可能なパフォーマンスと保証された容量を確保できるようになりました
- 入力トークン/分と出力トークン/分の容量を個別に割り当てることで、ワークロードに合わせてコストを最適化できるようになりました
- 予約容量を超える場合は、Standard Tier に自動的にオーバーフローし、中断のない運用を実現できるようになりました

## サービスアップデートの詳細

### 主要機能

1. **優先的なコンピュート容量の予約**
   - ミッションクリティカルなアプリケーションのために、優先的なコンピュート容量を予約できます
   - 予約した容量は、トークン/分 (tokens-per-minute) で指定します
   - 1 ヶ月または 3 ヶ月の期間で容量を予約できます

2. **非対称なトークン使用パターンへの対応**
   - 入力トークン/分と出力トークン/分の容量を個別に割り当てられます
   - 要約タスク: 多くの入力トークン、少ない出力トークン
   - コンテンツ生成: 少ない入力トークン、多くの出力トークン
   - ワークロードの特性に合わせて最適化できます

3. **自動オーバーフロー**
   - 予約した容量を超える場合、Standard Tier に自動的にオーバーフローします
   - 中断のない運用を保証します
   - ピーク時のトラフィックにも対応できます

4. **GOV-CRIS クロスリージョンプロファイル**
   - AWS GovCloud (US-West) の顧客は、GOV-CRIS クロスリージョンプロファイルを介して Reserved Tier にアクセスできます
   - GovCloud 環境でのコンプライアンス要件を満たします

## 技術仕様

### Reserved Tier の詳細

| 項目 | 詳細 |
|------|------|
| 対応モデル | Anthropic Claude Sonnet 4.5 |
| 利用可能リージョン | AWS GovCloud (US-West) |
| 予約期間 | 1 ヶ月または 3 ヶ月 |
| 容量の単位 | トークン/分 (tokens-per-minute) |
| 料金体系 | 固定価格/1K トークン/分、月額請求 |
| アップタイム目標 | 99.5% |

### トークン容量の割り当て

```
入力トークン/分: ワークロードの入力要件に基づいて設定
出力トークン/分: ワークロードの出力要件に基づいて設定
```

例:
- 要約タスク: 入力 10,000 トークン/分、出力 1,000 トークン/分
- コンテンツ生成: 入力 1,000 トークン/分、出力 10,000 トークン/分

## 設定方法

### 前提条件

1. AWS GovCloud (US-West) アカウントがあること
2. Amazon Bedrock へのアクセス権限があること
3. Reserved Tier へのアクセスを AWS アカウントチームに申請済みであること

### 手順

#### ステップ 1: Reserved Tier へのアクセス申請

AWS アカウントチームに連絡して、Reserved Tier へのアクセスを申請します。

#### ステップ 2: 容量の計算

ワークロードのトークン使用パターンを分析し、必要な入力トークン/分と出力トークン/分を計算します。

```python
# 例: 要約タスクの容量計算
input_tokens_per_request = 5000  # 平均入力トークン数
output_tokens_per_request = 500  # 平均出力トークン数
requests_per_minute = 10  # 分あたりのリクエスト数

required_input_tpm = input_tokens_per_request * requests_per_minute
required_output_tpm = output_tokens_per_request * requests_per_minute

print(f"必要な入力トークン/分: {required_input_tpm}")
print(f"必要な出力トークン/分: {required_output_tpm}")
```

#### ステップ 3: Reserved Tier の設定

AWS コンソールまたは API を使用して、Reserved Tier を設定します。

```bash
# AWS CLI を使用した例 (実際の API は AWS アカウントチームに確認してください)
aws bedrock create-reserved-capacity \
  --model-id anthropic.claude-sonnet-4-5-v1:0 \
  --region us-gov-west-1 \
  --duration-months 1 \
  --input-tokens-per-minute 50000 \
  --output-tokens-per-minute 5000
```

#### ステップ 4: Runtime API でサービス Tier を指定

Bedrock Runtime API を呼び出す際、`service_tier` パラメータを `reserved` に設定します。

```python
import boto3

bedrock_runtime = boto3.client('bedrock-runtime', region_name='us-gov-west-1')

response = bedrock_runtime.invoke_model(
    modelId='anthropic.claude-sonnet-4-5-v1:0',
    body=json.dumps({
        "prompt": "要約してください: ...",
        "max_tokens": 500,
        "service_tier": "reserved"
    })
)
```

## メリット

### ビジネス面

- **予測可能なパフォーマンス**: ミッションクリティカルなアプリケーションに対して、99.5% のアップタイムを目標としたサービスレベルを提供します
- **コスト最適化**: 非対称なトークン使用パターンに対して、入力と出力の容量を個別に割り当てることでコストを最適化できます
- **中断のない運用**: 自動オーバーフローにより、ピーク時でも中断のない運用を保証します

### 技術面

- **保証された容量**: 予約したトークン/分の容量が保証されます
- **柔軟な容量割り当て**: 入力と出力のトークン/分を個別に設定できます
- **GovCloud 対応**: AWS GovCloud (US-West) でコンプライアンス要件を満たしたサービスを利用できます

## デメリット・制約事項

### 制限事項

- Reserved Tier へのアクセスには、AWS アカウントチームへの申請が必要です
- 予約期間は 1 ヶ月または 3 ヶ月です
- GOV-CRIS クロスリージョンプロファイルを介してアクセスする必要があります

### 考慮すべき点

- ワークロードのトークン使用パターンを正確に把握して、適切な容量を予約してください
- 予約容量を超える使用は Standard Tier の料金で請求されます
- 予約容量が不足している場合は、Reserved Tier の容量を追加購入する必要があります

## ユースケース

### ユースケース 1: 政府機関のドキュメント要約

**シナリオ**: 政府機関が、大量のドキュメントを要約するミッションクリティカルなアプリケーションを運用している。

**実装例**:
- 入力トークン/分: 100,000 (長いドキュメントを処理)
- 出力トークン/分: 10,000 (簡潔な要約を生成)
- 予約期間: 3 ヶ月

**効果**: 予測可能なパフォーマンスと保証された容量により、ミッションクリティカルなアプリケーションを安定して運用できます。

### ユースケース 2: コンテンツ生成サービス

**シナリオ**: 政府機関が、市民向けのコンテンツを生成するサービスを提供している。

**実装例**:
- 入力トークン/分: 10,000 (短いプロンプトを処理)
- 出力トークン/分: 100,000 (長いコンテンツを生成)
- 予約期間: 1 ヶ月

**効果**: 非対称なトークン使用パターンに対して、コストを最適化しながら安定したサービスを提供できます。

### ユースケース 3: チャットボットサービス

**シナリオ**: 政府機関が、市民からの問い合わせに対応するチャットボットサービスを提供している。

**実装例**:
- 入力トークン/分: 50,000 (市民からの質問を処理)
- 出力トークン/分: 50,000 (回答を生成)
- 予約期間: 3 ヶ月
- ピーク時には Standard Tier に自動オーバーフロー

**効果**: 予約容量により基本的な負荷を処理し、ピーク時には自動オーバーフローで対応できます。

## 料金

Reserved Tier の料金は、固定価格/1K トークン/分で、月額請求されます。予約容量を超える使用は、Standard Tier の従量課金料金で請求されます。

詳細については、[Amazon Bedrock Reserved Tier ドキュメント](https://docs.aws.amazon.com/bedrock/latest/userguide/service-tiers-inference.html) を参照してください。

## 利用可能リージョン

この機能は、AWS GovCloud (US-West) リージョンで利用可能です。GOV-CRIS クロスリージョンプロファイルを介してアクセスできます。

## 関連サービス・機能

- **Amazon Bedrock Standard Tier**: 日常的な AI タスクのための一貫したパフォーマンスを提供するデフォルトのサービス Tier
- **Amazon Bedrock Priority Tier**: 価格プレミアムで最速の応答時間を提供するサービス Tier
- **Amazon Bedrock Flex Tier**: より長い処理時間を許容できるワークロードのためのコスト効率的な処理を提供するサービス Tier

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/amazon-bedrock-reserved-tier-for-claude-sonnet-in-govcloud/)
- [Service tiers for optimizing performance and cost ドキュメント](https://docs.aws.amazon.com/bedrock/latest/userguide/service-tiers-inference.html)
- [Amazon Bedrock in AWS GovCloud (US) ドキュメント](https://docs.aws.amazon.com/govcloud-us/latest/UserGuide/govcloud-bedrock.html)

## まとめ

Amazon Bedrock Reserved Tier が AWS GovCloud (US-West) リージョンの Claude Sonnet 4.5 に対応したことで、政府機関はミッションクリティカルなアプリケーションに対して予測可能なパフォーマンスと保証された容量を確保できるようになりました。入力と出力のトークン/分を個別に割り当てる柔軟性により、ワークロードに合わせてコストを最適化でき、自動オーバーフローにより中断のない運用を実現できます。AWS GovCloud で生成 AI アプリケーションを運用している組織は、Reserved Tier を活用してサービスレベルを向上させることをお勧めします。
