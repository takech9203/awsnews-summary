# Amazon SageMaker HyperPod - ライフサイクルスクリプトデバッグ機能強化

**リリース日**: 2026 年 1 月 21 日
**サービス**: Amazon SageMaker HyperPod
**機能**: ライフサイクルスクリプトの強化されたデバッグ機能

## 概要

Amazon SageMaker HyperPod が、ライフサイクルスクリプトのトラブルシューティング機能を強化し、クラスターノードのプロビジョニング中の問題の特定と解決を容易にしました。SageMaker HyperPod は、AI/ML ワークロードの実行や、大規模言語モデル (LLM)、拡散モデル、基盤モデル (FM) などの最先端モデルの開発のための回復力のあるクラスターをプロビジョニングするのに役立ちます。

今回のアップデートにより、ライフサイクルスクリプトで問題が発生した際、詳細なエラーメッセージが提供され、実行ログを確認できる CloudWatch ログ グループとログ ストリーム名が表示されるようになりました。また、CloudWatch ログには、スクリプトの実行進捗を追跡するための特定のマーカーが含まれるようになり、問題が発生した箇所を迅速に特定できるようになりました。

**アップデート前の課題**

- ライフサイクルスクリプトのエラーが発生した際、詳細なエラー情報が不足しており、問題の特定に時間がかかりました
- CloudWatch ログ グループやログ ストリーム名を手動で探す必要があり、トラブルシューティングが困難でした
- スクリプトの実行進捗を追跡するマーカーがなく、どの段階で問題が発生したのか特定しにくい状況でした

**アップデート後の改善**

- エラーメッセージに CloudWatch ログ グループとログ ストリーム名が含まれるようになり、すぐにログを確認できるようになりました
- SageMaker コンソールの「View lifecycle script logs」ボタンから、該当する CloudWatch ログ ストリームに直接ナビゲートできるようになりました
- CloudWatch ログに特定のマーカー (ログ開始、スクリプトダウンロード中、ダウンロード完了、スクリプト成功/失敗) が追加され、問題発生箇所を迅速に特定できるようになりました

## サービスアップデートの詳細

### 主要機能

1. **詳細なエラーメッセージ**
   - ライフサイクルスクリプトのエラー発生時に、CloudWatch ログ グループとログ ストリーム名が含まれるエラーメッセージが提供されます
   - DescribeCluster API を実行するか、SageMaker コンソールのクラスター詳細ページでエラーメッセージを確認できます
   - エラーメッセージから直接ログの場所を特定できるため、トラブルシューティング時間を短縮できます

2. **コンソールからの直接アクセス**
   - SageMaker コンソールに「View lifecycle script logs」ボタンが追加されました
   - このボタンをクリックすることで、該当する CloudWatch ログ ストリームに直接ナビゲートできます
   - ログの場所を手動で探す必要がなくなり、効率的なトラブルシューティングが可能になります

3. **実行進捗マーカー**
   - CloudWatch ログに以下のマーカーが追加されました
     - ライフサイクルスクリプトログの開始
     - スクリプトのダウンロード中
     - スクリプトのダウンロード完了
     - スクリプトの成功または失敗
   - これらのマーカーにより、プロビジョニングプロセスのどの段階で問題が発生したかを迅速に特定できます

## 技術仕様

### CloudWatch ログの構造

| ログ項目 | 説明 |
|----------|------|
| ログ グループ名 | /aws/sagemaker/hyperpod/cluster-name |
| ログ ストリーム名 | instance-id/lifecycle-script-name |
| マーカー | スクリプト実行の各段階を示す特定のログエントリ |

### エラーメッセージの形式

エラーメッセージには以下の情報が含まれます。

```json
{
  "ErrorMessage": "Lifecycle script failed",
  "LogGroup": "/aws/sagemaker/hyperpod/my-cluster",
  "LogStream": "i-1234567890abcdef0/on-create.sh",
  "FailureReason": "Script execution failed with exit code 1"
}
```

## 設定方法

### 前提条件

1. Amazon SageMaker HyperPod クラスターが作成されていること
2. ライフサイクルスクリプトが設定されていること
3. CloudWatch Logs へのアクセス権限が付与されていること

### 手順

#### ステップ 1: エラーメッセージの確認

```bash
# DescribeCluster API を使用してエラー情報を取得
aws sagemaker describe-cluster --cluster-name my-hyperpod-cluster
```

このコマンドは、クラスターの詳細情報を取得し、ライフサイクルスクリプトのエラーがある場合、CloudWatch ログの場所を含むエラーメッセージを返します。

#### ステップ 2: CloudWatch ログの確認

SageMaker コンソールの「View lifecycle script logs」ボタンをクリックするか、以下のコマンドで CloudWatch ログを確認します。

```bash
# CloudWatch ログストリームの内容を取得
aws logs get-log-events \
  --log-group-name /aws/sagemaker/hyperpod/my-cluster \
  --log-stream-name i-1234567890abcdef0/on-create.sh
```

このコマンドは、指定されたログ ストリームのログイベントを取得し、ライフサイクルスクリプトの実行ログを表示します。

#### ステップ 3: マーカーを使用した問題の特定

CloudWatch ログ内のマーカーを確認し、どの段階で問題が発生したかを特定します。

```
[LIFECYCLE_SCRIPT_START] Starting lifecycle script execution
[SCRIPT_DOWNLOAD_START] Downloading script from s3://bucket/path/script.sh
[SCRIPT_DOWNLOAD_COMPLETE] Script download completed
[SCRIPT_EXECUTION_FAILED] Script failed with exit code 1
```

## メリット

### ビジネス面

- **ダウンタイムの削減**: 問題を迅速に特定して解決することで、クラスターのダウンタイムを最小限に抑えます
- **運用コストの削減**: トラブルシューティング時間の短縮により、運用コストを削減できます
- **生産性の向上**: クラスターを迅速に稼働させることで、AI/ML ワークロードの開発効率を向上させます

### 技術面

- **効率的なデバッグ**: エラーメッセージに含まれる CloudWatch ログの場所から、すぐにログを確認できます
- **詳細な実行追跡**: マーカーを使用して、スクリプト実行の各段階を追跡できます
- **統合されたワークフロー**: SageMaker コンソールから CloudWatch ログに直接アクセスできます

## デメリット・制約事項

### 制限事項

- この機能は、Amazon SageMaker HyperPod をサポートするすべての AWS リージョンで利用可能ですが、既存のクラスターには適用されません
- CloudWatch ログへのアクセス権限が必要です

### 考慮すべき点

- CloudWatch ログの保持期間を適切に設定し、必要なログが保持されるようにしてください
- ログの量が多い場合、CloudWatch Logs の料金が増加する可能性があります

## ユースケース

### ユースケース 1: クラスター作成時のスクリプトエラーのデバッグ

**シナリオ**: HyperPod クラスターの作成中にライフサイクルスクリプトがエラーを返し、クラスターのプロビジョニングが失敗した。

**実装例**:
1. DescribeCluster API を実行してエラーメッセージを確認
2. エラーメッセージに含まれる CloudWatch ログ グループとログ ストリーム名を使用してログを確認
3. マーカーを使用して、スクリプトのどの段階で問題が発生したかを特定
4. スクリプトを修正して再度クラスターを作成

**効果**: トラブルシューティング時間を大幅に短縮し、クラスターを迅速に稼働させることができます。

### ユースケース 2: ノード追加時のプロビジョニングエラーの解決

**シナリオ**: 既存のクラスターに新しいノードを追加する際、ライフサイクルスクリプトが失敗し、ノードがクラスターに参加できない。

**実装例**:
1. SageMaker コンソールの「View lifecycle script logs」ボタンをクリック
2. CloudWatch ログ ストリームに直接ナビゲート
3. ログ内のマーカーを確認して、問題の原因を特定
4. スクリプトまたは設定を修正して再度ノードを追加

**効果**: コンソールから直接ログにアクセスでき、迅速な問題解決が可能になります。

### ユースケース 3: スクリプトダウンロードの問題の特定

**シナリオ**: ライフサイクルスクリプトが S3 バケットからダウンロードされない。

**実装例**:
1. CloudWatch ログで `[SCRIPT_DOWNLOAD_START]` と `[SCRIPT_DOWNLOAD_COMPLETE]` マーカーを確認
2. ダウンロードが完了していない場合、S3 バケットへのアクセス権限や S3 パスを確認
3. 問題を修正して再度クラスターを作成

**効果**: スクリプトダウンロードの問題を迅速に特定し、解決できます。

## 料金

この機能自体に追加料金はかかりません。ただし、CloudWatch Logs の使用に対しては通常の CloudWatch Logs の料金が適用されます。

詳細については、[CloudWatch Logs の料金](https://aws.amazon.com/cloudwatch/pricing/) を参照してください。

## 利用可能リージョン

この機能は、Amazon SageMaker HyperPod をサポートするすべての AWS リージョンで利用可能です。

## 関連サービス・機能

- **Amazon CloudWatch Logs**: ライフサイクルスクリプトのログを保存および分析するサービス
- **Amazon SageMaker**: 機械学習モデルの構築、トレーニング、デプロイを行うための統合開発環境
- **AWS Lambda**: イベント駆動型のサーバーレスコンピューティングサービス

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/amazon-sagemaker-hyperpod-lcs-enhanced-debug/)
- [SageMaker HyperPod cluster management ドキュメント](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-hyperpod-cluster-management-slurm.html)
- [Amazon SageMaker Developer Guide](https://docs.aws.amazon.com/sagemaker/latest/dg/)

## まとめ

Amazon SageMaker HyperPod のライフサイクルスクリプトデバッグ機能の強化により、クラスターノードのプロビジョニング中の問題を迅速に特定して解決できるようになりました。詳細なエラーメッセージ、コンソールからの直接アクセス、実行進捗マーカーにより、トラブルシューティング時間が大幅に短縮され、HyperPod クラスターを迅速に稼働させることができます。AI/ML ワークロードを SageMaker HyperPod で実行している組織は、この機能を活用してより効率的な運用を実現できます。
