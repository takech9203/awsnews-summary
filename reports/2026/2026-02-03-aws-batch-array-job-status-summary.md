# AWS Batch - ListJobs API での Array Job ステータスサマリー

**リリース日**: 2026年2月3日
**サービス**: AWS Batch
**機能**: Array Job Status Summary in ListJobs API

## 概要

AWS Batch が ListJobs API レスポンスに Array Jobs のステータスサマリーを追加しました。Array Job の子ジョブのステータス分布を追加 API 呼び出しなしで即座に確認できます。これまで DescribeJobs API でのみ利用可能だった statusSummary フィールドが ListJobs でも提供されるようになり、大規模バッチワークロードの監視が効率化されます。

**アップデート前の課題**

- Array Job のステータス確認に DescribeJobs API のみを利用していた
- 複数 Array Job のステータスを確認する際、複数回の DescribeJobs 呼び出しが必要だった
- ListJobs で Array Job 全体の進捗を把握する場合、詳細情報が不足していた
- 大規模バッチ処理(数千ジョブ規模)の監視時に複数 API 呼び出しが必要で、負荷が高かった

**アップデート後の改善**

- ListJobs 1 回の API 呼び出しで複数 Array Job のステータスサマリーを取得可能
- 各ステータス(SUBMITTED、PENDING、RUNNABLE、STARTING、RUNNING、SUCCEEDED、FAILED)の子ジョブ数が即座に確認できる
- statusSummaryLastUpdatedAt タイムスタンプでステータス情報の鮮度を判断可能
- キューに複数 Array Job がある場合、1 回の API で全体進捗を把握可能

## 主要機能

1. **statusSummary フィールド**
   - ListJobs レスポンスに子ジョブのステータス分布を表示
   - 各ステータスの子ジョブ数をカウント

2. **ステータス項目**
   - SUBMITTED: 投入済み
   - PENDING: 保留中
   - RUNNABLE: 実行可能
   - STARTING: 開始中
   - RUNNING: 実行中
   - SUCCEEDED: 成功
   - FAILED: 失敗

3. **ステータス鮮度情報**
   - statusSummaryLastUpdatedAt: ステータス情報の最終更新時刻
   - ステータス情報がどの程度最新かを判定可能

## 技術仕様

### ListJobs API レスポンス

| フィールド | 型 | 説明 |
|---|---|---|
| statusSummary | Object | 子ジョブのステータス分布 |
| statusSummary.SUBMITTED | Integer | SUBMITTED 状態の子ジョブ数 |
| statusSummary.PENDING | Integer | PENDING 状態の子ジョブ数 |
| statusSummary.RUNNABLE | Integer | RUNNABLE 状態の子ジョブ数 |
| statusSummary.STARTING | Integer | STARTING 状態の子ジョブ数 |
| statusSummary.RUNNING | Integer | RUNNING 状態の子ジョブ数 |
| statusSummary.SUCCEEDED | Integer | SUCCEEDED 状態の子ジョブ数 |
| statusSummary.FAILED | Integer | FAILED 状態の子ジョブ数 |
| statusSummaryLastUpdatedAt | Timestamp | ステータス情報の最終更新時刻 |

## メリット

### ビジネス面

- **運用効率化**: API 呼び出し回数削減による監視コスト低減
- **可視性向上**: ジョブキューの進捗を一目で把握
- **スケーラビリティ**: 大規模バッチジョブの管理が容易

### 技術面

- **API 効率化**: 単一 API 呼び出しで複数ジョブの進捗を確認
- **リアルタイム監視**: ステータス情報の鮮度管理が可能
- **応答性向上**: キュー全体の状態を素早く判定

## ユースケース

### ユースケース1: 金融サービスのバッチ処理監視

**シナリオ**: 数千件の取引データを処理する Array Job を複数実行。各ジョブの進捗を監視する場合

**効果**: ListJobs 1 回の API で全ジョブの進捗を把握。監視処理のコスト削減

### ユースケース2: 自動車産業のシミュレーション処理

**シナリオ**: 複数の設計シミュレーション Array Job を並行実行し、全体の処理状況を把握する場合

**効果**: ステータスサマリーで各ジョブの進捗状況を統合監視。成功・失敗の内訳を即座に把握

### ユースケース3: データ処理パイプラインの自動スケーリング

**シナリオ**: ジョブキューの進捗に応じて、ダウンストリームの処理をトリガーする場合

**効果**: statusSummaryLastUpdatedAt で情報鮮度を確認しながら、キュー状態に基づいた意思決定が可能

## 利用可能リージョン

AWS Batch をサポートするすべての AWS リージョンで利用可能です。

## 関連サービス・機能

- **AWS Batch Array Job**: 複数の関連ジョブを一括実行する機能
- **Amazon CloudWatch**: バッチジョブのメトリクス監視
- **AWS Lambda**: ジョブ完了時のトリガー処理

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/aws-batch-array-job-status-summary/)
- [AWS Batch API Reference - ListJobs](https://docs.aws.amazon.com/batch/latest/APIReference/API_ListJobs.html)

## まとめ

ListJobs API への statusSummary 追加により、大規模バッチワークロード監視の効率性が大幅に向上します。特に金融、自動車産業など大規模並列処理が必須の分野で、運用負荷削減と可視性向上が同時に実現されます。
