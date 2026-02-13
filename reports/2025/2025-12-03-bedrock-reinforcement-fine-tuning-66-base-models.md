# Amazon Bedrock - 強化学習ファインチューニング（Reinforcement Fine-Tuning）

**リリース日**: 2025年12月3日  
**サービス**: Amazon Bedrock  
**機能**: Reinforcement Fine-Tuning（強化学習ファインチューニング）


## 概要

Amazon Bedrock に強化学習ファインチューニング（Reinforcement Fine-Tuning: RFT）機能が追加されました。この機能により、深い機械学習の専門知識や大量のラベル付きデータがなくても、モデルの精度を向上させることが可能になります。

従来のファインチューニング手法では大量のデータが必要でしたが、RFT では少量のプロンプトセットを使用してモデルを特定の要件に合わせて調整できます。同じプロンプトに対する複数の応答へのフィードバックを通じてモデルを学習させることで、良い応答とは何かの判断力を向上させます。

Amazon Bedrock の強化学習ファインチューニングは、ベースモデルと比較して平均66%の精度向上を実現します。これにより、高品質を維持しながら、より小型で高速、コスト効率の良いモデルバリアントを使用できます。

**アップデート前の課題**

- 以前はモデルのファインチューニングに大量のラベル付きデータと深い機械学習の専門知識が必要だった
- 以前は特定のユースケースに合わせたモデルカスタマイズに高度な ML インフラストラクチャと人材が必要だった
- 以前は小型で効率的なモデルを使用しながら高い精度を維持することが困難だった

**アップデート後の改善**

- 今回のアップデートにより、少量のプロンプトセットと報酬関数でモデルをカスタマイズでき、大量のラベル付きデータが不要になった
- 今回のアップデートにより、ベースモデルと比較して平均 66% の精度向上を実現できるようになった
- 今回のアップデートにより、ルールベースまたは AI ベースのグレーダーで柔軟に報酬関数を定義できるようになった


## サービスアップデートの詳細

### 主要機能

1. **自動化されたRFTワークフロー**
   - 高度なモデルカスタマイズ技術を一般の開発者でも利用可能に
   - 専門的なMLインフラストラクチャや人材が不要
   - セキュアなAWS環境内でデータを保持

2. **柔軟なトレーニングデータ入力**
   - ローカルコンピュータから直接トレーニングデータをアップロード可能
   - Amazon S3 に保存済みのデータセットを選択可能
   - ラベル付きデータセットが不要

3. **報酬関数の定義**
   - 検証可能なルールベースのグレーダー
   - AIベースのジャッジ
   - 組み込みテンプレートを使用した最適化
   - コード生成や数学的推論などの客観的タスクに対応
   - 指示追従やチャットボットインタラクションなどの主観的タスクに対応


## 技術仕様

### 対応モデル

| 項目 | 詳細 |
|------|------|
| ローンチ時対応モデル | Amazon Nova 2 Lite |
| 今後の対応 | 追加モデルのサポート予定 |

### API変更履歴

| 日付 | サービス | 変更内容 |
|------|----------|----------|
| 2025/12/03 | Amazon Bedrock | 1 new 6 updated api methods - RFTサポートとカスタムモデルデプロイメント更新機能の追加 |

### 新規API

**UpdateCustomModelDeployment**
- 新しいカスタムモデルでカスタムモデルデプロイメントを更新
- 新しいデプロイメントエンドポイントを作成せずに更新されたモデルをデプロイ可能

### 更新されたAPI

**CreateModelCustomizationJob** - 新しいカスタマイズタイプとRFT設定の追加

```python
# customizationType に新しい値が追加
customizationType='REINFORCEMENT_FINE_TUNING'

# RFT設定パラメータ
customizationConfig={
    'rftConfig': {
        'graderConfig': {
            'lambdaGrader': {
                'lambdaArn': 'string'
            }
        },
        'hyperParameters': {
            'epochCount': 123,
            'batchSize': 123,
            'learningRate': 0.0,
            'maxPromptLength': 123,
            'trainingSamplePerPrompt': 123,
            'inferenceMaxTokens': 123,
            'reasoningEffort': 'low'|'medium'|'high',
            'evalInterval': 123
        }
    }
}
```

### RFTハイパーパラメータ

| パラメータ | 説明 |
|-----------|------|
| epochCount | トレーニングエポック数 |
| batchSize | バッチサイズ |
| learningRate | 学習率 |
| maxPromptLength | 最大プロンプト長 |
| trainingSamplePerPrompt | プロンプトあたりのトレーニングサンプル数 |
| inferenceMaxTokens | 推論時の最大トークン数 |
| reasoningEffort | 推論の努力レベル（low/medium/high） |
| evalInterval | 評価間隔 |


## 設定方法

### 前提条件

1. AWS アカウント
2. Amazon Bedrock へのアクセス権限
3. トレーニングデータ（S3またはローカル）
4. IAM ロール（S3アクセス権限付き）

### 手順

#### ステップ1: Amazon Bedrock コンソールにアクセス

[Amazon Bedrock コンソール](https://console.aws.amazon.com/bedrock) にアクセスし、カスタムモデルセクションに移動します。

#### ステップ2: トレーニングデータの準備

トレーニングデータをローカルからアップロードするか、S3 URI を指定します。

#### ステップ3: 報酬関数の設定

```python
# Lambda グレーダーを使用する場合
graderConfig = {
    'lambdaGrader': {
        'lambdaArn': 'arn:aws:lambda:region:account:function:grader-function'
    }
}
```

#### ステップ4: カスタマイズジョブの作成

AWS SDK または API を使用してジョブを作成します。


## メリット

### ビジネス面

- **コスト削減**: より小型で効率的なモデルを使用可能
- **迅速な導入**: 専門家不要で高度なカスタマイズが可能
- **セキュリティ**: データがAWS環境外に出ない

### 技術面

- **66%の精度向上**: ベースモデルと比較した平均的な改善
- **少量データでの学習**: 大量のラベル付きデータが不要
- **柔軟な報酬関数**: ルールベースとAIベースの両方に対応


## デメリット・制約事項

### 制限事項

- ローンチ時は Amazon Nova 2 Lite のみ対応
- 追加モデルのサポートは今後予定

### 考慮すべき点

- 報酬関数の設計がモデル品質に大きく影響
- Lambda グレーダーを使用する場合は追加のLambda関数の開発が必要


## ユースケース

### ユースケース1: コード生成の最適化

**シナリオ**: 社内のコーディング規約に沿ったコード生成モデルを作成したい

**実装例**:
```python
# ルールベースのグレーダーでコード品質を評価
customizationConfig = {
    'rftConfig': {
        'graderConfig': {
            'lambdaGrader': {
                'lambdaArn': 'arn:aws:lambda:us-east-1:123456789012:function:code-quality-grader'
            }
        },
        'hyperParameters': {
            'reasoningEffort': 'high'
        }
    }
}
```

**効果**: 社内規約に準拠したコード生成の精度向上

### ユースケース2: カスタマーサポートチャットボット

**シナリオ**: 企業固有の製品知識とトーンに合わせたチャットボットを構築

**実装例**:
```python
# AIベースのジャッジで応答品質を評価
customizationType = 'REINFORCEMENT_FINE_TUNING'
trainingDataConfig = {
    's3Uri': 's3://my-bucket/customer-support-prompts/'
}
```

**効果**: ブランドに合った一貫性のある顧客対応

### ユースケース3: 数学的推論タスク

**シナリオ**: 数学問題の解答精度を向上させたい

**実装例**:
```python
hyperParameters = {
    'reasoningEffort': 'high',
    'inferenceMaxTokens': 2048,
    'trainingSamplePerPrompt': 5
}
```

**効果**: 複雑な数学問題に対する正確な解答生成


## 料金

料金の詳細は [Amazon Bedrock 料金ページ](https://aws.amazon.com/bedrock/pricing/) を参照してください。


## 利用可能リージョン

Amazon Bedrock が利用可能なリージョンで提供されます。詳細は公式ドキュメントを確認してください。


## 関連サービス・機能

- **Amazon Bedrock Custom Models**: 従来のファインチューニング機能
- **Amazon Bedrock Model Distillation**: モデル蒸留機能
- **Amazon S3**: トレーニングデータの保存
- **AWS Lambda**: カスタムグレーダー関数の実行


## 参考リンク

- [公式発表](https://aws.amazon.com/about-aws/whats-new/2025/12/bedrock-reinforcement-fine-tuning-66-base-models/)
- [ドキュメント](https://docs.aws.amazon.com/bedrock/latest/userguide/reinforcement-fine-tuning.html)
- [ローンチブログ](https://aws.amazon.com/blogs/aws/improve-model-accuracy-with-reinforcement-fine-tuning-in-amazon-bedrock/)
- [料金ページ](https://aws.amazon.com/bedrock/pricing/)
- [Amazon Bedrock API リファレンス](https://docs.aws.amazon.com/bedrock/latest/APIReference/welcome.html)


## まとめ

Amazon Bedrock の強化学習ファインチューニングは、専門知識や大量データなしでモデル精度を平均66%向上させる画期的な機能です。Amazon Nova 2 Lite から利用開始でき、コンソールまたはAPIから簡単にセットアップできます。
