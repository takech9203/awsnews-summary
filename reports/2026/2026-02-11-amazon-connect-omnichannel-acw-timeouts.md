# Amazon Connect - オムニチャネル ACW タイムアウト設定

**リリース日**: 2026年2月11日
**サービス**: Amazon Connect
**機能**: チャット、タスク、メール、コールバック向け After Contact Work タイムアウト設定

📊 [このアップデートのインフォグラフィックを見る](https://takech9203.github.io/aws-news-summary/20260211-amazon-connect-omnichannel-acw-timeouts.html)

## 概要

Amazon Connect がチャット、タスク、メール、コールバックの各チャネルに対して After Contact Work (ACW) タイムアウト設定をサポートしました。これにより、エージェントがコンタクト終了後の作業に費やす時間をチャネルごとに制御でき、効率的なワークフォース管理が可能になります。

ACW タイムアウトは、コンタクト終了後にエージェントが後処理作業を行える時間を制限する機能です。タイムアウトが経過すると、エージェントは自動的に「準備完了」状態に戻り、次のコンタクトを受けられるようになります。

**アップデート前の課題**

- ACW タイムアウト設定は音声チャネルのみで利用可能だった
- チャット、タスク、メール、コールバックでは個別の ACW タイムアウトを設定できなかった
- チャネルごとに異なる後処理時間を設定する柔軟性がなかった

**アップデート後の改善**

- チャット、タスク、メール、コールバックを含むすべてのチャネルで ACW タイムアウトが設定可能に
- チャネルレベルでエージェントごとに個別の ACW タイムアウトを構成可能
- 例: メール向けには短い ACW タイムアウト、音声向けには長いクールダウン期間を設定可能

## サービスアップデートの詳細

### 主要機能

1. **チャネルレベルの ACW タイムアウト設定**
   - 各チャネル (VOICE、CHAT、TASK、EMAIL) に対して個別にタイムアウト値を設定可能
   - エージェントごとに異なる設定が適用可能
   - タイムアウト経過後、エージェントは自動的に「準備完了」状態に移行

2. **コールバック向け ACW 設定**
   - エージェントが最初のコールバックを処理する際の ACW 設定も個別に構成可能
   - `AgentFirstCallbackAfterContactWorkConfig` パラメータで制御

3. **新しい UpdateUserConfig API**
   - エージェントの ACW 設定をプログラムで管理する新しい API
   - 既存の `CreateUser` API にも新しいパラメータが追加

## 技術仕様

### API 変更履歴

| 日付 | サービス | 変更内容 |
|------|----------|----------|
| 2026/02/10 | [Amazon Connect Service](https://awsapichanges.com/archive/changes/56b6d8-connect.html) | 1 new 3 updated api methods - UpdateUserConfig API の追加、CreateUser/DescribeUser/UpdateUser の更新 |

### UpdateUserConfig API パラメータ

```json
{
  "AfterContactWorkConfigs": [
    {
      "Channel": "VOICE | CHAT | TASK | EMAIL",
      "AfterContactWorkConfig": {
        "AfterContactWorkTimeLimit": 120
      },
      "AgentFirstCallbackAfterContactWorkConfig": {
        "AfterContactWorkTimeLimit": 60
      }
    }
  ],
  "UserId": "agent-user-id",
  "InstanceId": "connect-instance-id"
}
```

## 設定方法

### 前提条件

1. Amazon Connect インスタンスが構成済みであること
2. 管理者権限を持つ IAM ロールまたはユーザー
3. 最新バージョンの AWS CLI / SDK

### 手順

#### ステップ 1: ユーザー管理ページでの設定

AWS マネジメントコンソールから Amazon Connect インスタンスにログインし、ユーザー管理ページでエージェントごとに ACW タイムアウトを設定します。

#### ステップ 2: API による設定

```bash
aws connect update-user-config \
  --instance-id "your-instance-id" \
  --user-id "agent-user-id" \
  --after-contact-work-configs '[
    {
      "Channel": "CHAT",
      "AfterContactWorkConfig": {
        "AfterContactWorkTimeLimit": 60
      }
    },
    {
      "Channel": "EMAIL",
      "AfterContactWorkConfig": {
        "AfterContactWorkTimeLimit": 30
      }
    },
    {
      "Channel": "VOICE",
      "AfterContactWorkConfig": {
        "AfterContactWorkTimeLimit": 120
      }
    }
  ]'
```

チャネルごとに異なる ACW タイムアウト値を秒単位で指定して設定します。

## メリット

### ビジネス面

- **エージェント稼働率の向上**: チャネルごとに最適な後処理時間を設定することで、エージェントの待機時間を削減
- **カスタマーエクスペリエンスの改善**: エージェントがより迅速に次のコンタクトに対応可能になり、顧客の待ち時間を短縮
- **柔軟なワークフォース最適化**: チャネル特性に応じた細かな調整が可能

### 技術面

- **API ベースの自動化**: UpdateUserConfig API により、大規模なエージェント設定の一括管理が可能
- **既存 API との互換性**: CreateUser API にも新パラメータが追加され、ユーザー作成時から設定可能
- **チャネルレベルの粒度**: 各チャネルに独立した設定を適用可能

## デメリット・制約事項

### 制限事項

- タイムアウト値の最小値や最大値は Amazon Connect のサービスクォータに依存
- 既存の音声チャネルの ACW 設定との整合性に注意が必要

### 考慮すべき点

- エージェントの業務内容に応じて適切なタイムアウト値を検討する必要がある
- タイムアウトが短すぎると、後処理作業が完了しないリスクがある

## ユースケース

### ユースケース 1: メールと音声の ACW 最適化

**シナリオ**: コンタクトセンターで、メール対応後の処理は短時間で済むが、音声通話後は CRM 入力やフォローアップメモの作成に時間がかかる場合

**実装例**:
```json
{
  "AfterContactWorkConfigs": [
    {"Channel": "EMAIL", "AfterContactWorkConfig": {"AfterContactWorkTimeLimit": 30}},
    {"Channel": "VOICE", "AfterContactWorkConfig": {"AfterContactWorkTimeLimit": 180}}
  ]
}
```

**効果**: メール対応後はエージェントが 30 秒で次のコンタクトに移行し、音声通話後は 3 分間の後処理時間を確保

### ユースケース 2: タスク処理の効率化

**シナリオ**: バックオフィスタスクの処理後に ACW が不要で、即座に次のタスクに取り組みたい場合

**実装例**:
```json
{
  "AfterContactWorkConfigs": [
    {"Channel": "TASK", "AfterContactWorkConfig": {"AfterContactWorkTimeLimit": 0}},
    {"Channel": "CHAT", "AfterContactWorkConfig": {"AfterContactWorkTimeLimit": 60}}
  ]
}
```

**効果**: タスク処理後は即時に次のタスクが割り当てられ、チャット後は 1 分間の後処理時間を確保

## 料金

ACW タイムアウト設定機能の利用に追加料金は発生しません。Amazon Connect の標準料金が適用されます。

## 利用可能リージョン

Amazon Connect が提供されているすべての AWS リージョンで利用可能です。

## 関連サービス・機能

- **Amazon Connect auto-accept**: 同日リリースされた、チャネルごとの自動承認設定機能
- **Amazon Connect Audio Enhancement**: エージェント音声のノイズ抑制機能
- **Amazon Connect Contact Lens**: リアルタイム分析とエージェントパフォーマンス管理

## 参考リンク

- 📊 [インフォグラフィック](https://takech9203.github.io/aws-news-summary/20260211-amazon-connect-omnichannel-acw-timeouts.html)
- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-connect-omnichannel-acw-timeouts/)
- [Amazon Connect 管理者ガイド - エージェント設定](https://docs.aws.amazon.com/connect/latest/adminguide/configure-agents.html)
- [Amazon Connect ウェブサイト](https://aws.amazon.com/connect/)

## まとめ

Amazon Connect のオムニチャネル ACW タイムアウト設定は、コンタクトセンターの運営効率を大幅に向上させる機能です。チャネルごとに最適な後処理時間を設定することで、エージェントの稼働率向上と顧客体験の改善を同時に実現できます。既存の Amazon Connect ユーザーは、コンソールまたは新しい UpdateUserConfig API を使用して、すぐにこの機能を活用することを推奨します。
