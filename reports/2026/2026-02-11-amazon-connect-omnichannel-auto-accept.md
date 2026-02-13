# Amazon Connect - オムニチャネル自動承認設定

**リリース日**: 2026年2月11日
**サービス**: Amazon Connect
**機能**: チャット、タスク、メール、コールバック向け自動承認 (Auto-Accept) 設定

📊 [このアップデートのインフォグラフィックを見る](https://takech9203.github.io/awsnews-summary/20260211-amazon-connect-omnichannel-auto-accept.html)

## 概要

Amazon Connect がチャット、タスク、メール、コールバックの各チャネルに対して自動承認 (Auto-Accept) 設定をサポートしました。Auto-Accept を有効にすると、着信コンタクトが利用可能なエージェントに自動的に接続され、エージェントが手動で承認・拒否する手順が不要になります。

これまで Auto-Accept 設定はインバウンド音声通話のみで利用可能でしたが、今回のアップデートにより、すべてのチャネルでチャネルレベルの粒度で設定できるようになりました。

**アップデート前の課題**

- Auto-Accept 設定はインバウンド音声通話のみで利用可能だった
- チャット、タスク、メール、コールバックではエージェントが手動で各コンタクトを承認する必要があった
- チャネルごとに異なる自動承認ポリシーを設定する柔軟性がなかった

**アップデート後の改善**

- チャット、タスク、メール、コールバックを含むすべてのチャネルで Auto-Accept が設定可能に
- チャネルレベルでエージェントごとに個別の Auto-Accept 設定を構成可能
- 例: タスクは自動承認にして、音声通話は手動承認のままにする、といった使い分けが可能

## サービスアップデートの詳細

### 主要機能

1. **チャネルレベルの Auto-Accept 設定**
   - 各チャネル (VOICE、CHAT、TASK、EMAIL) に対して個別に Auto-Accept を有効/無効化
   - エージェントごとに異なる設定を適用可能
   - コンタクトが自動的にエージェントに接続されるため、応答時間が短縮

2. **コールバック向け Auto-Accept 設定**
   - エージェントが最初のコールバックを受ける際の Auto-Accept も個別に設定可能
   - `AgentFirstCallbackAutoAccept` パラメータで制御

3. **UpdateUserConfig API での管理**
   - 同日リリースの新しい API で Auto-Accept 設定もプログラムで管理可能
   - CreateUser API にも AutoAcceptConfigs パラメータが追加

## 技術仕様

### API 変更履歴

| 日付 | サービス | 変更内容 |
|------|----------|----------|
| 2026/02/10 | [Amazon Connect Service](https://awsapichanges.com/archive/changes/56b6d8-connect.html) | 1 new 3 updated api methods - UpdateUserConfig API の追加、CreateUser/DescribeUser/UpdateUser の更新 |

### AutoAcceptConfigs パラメータ

```json
{
  "AutoAcceptConfigs": [
    {
      "Channel": "VOICE | CHAT | TASK | EMAIL",
      "AutoAccept": true,
      "AgentFirstCallbackAutoAccept": true
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

#### ステップ 1: コンソールでの設定

Amazon Connect 管理コンソールにログインし、ユーザー管理ページからエージェントの Auto-Accept 設定をチャネルごとに有効化します。

#### ステップ 2: API による設定

```bash
aws connect update-user-config \
  --instance-id "your-instance-id" \
  --user-id "agent-user-id" \
  --auto-accept-configs '[
    {
      "Channel": "TASK",
      "AutoAccept": true
    },
    {
      "Channel": "CHAT",
      "AutoAccept": true
    },
    {
      "Channel": "EMAIL",
      "AutoAccept": true
    },
    {
      "Channel": "VOICE",
      "AutoAccept": false
    }
  ]'
```

チャネルごとに Auto-Accept の有効/無効を設定します。この例ではタスク、チャット、メールは自動承認にし、音声通話は手動承認のままにしています。

## メリット

### ビジネス面

- **顧客待ち時間の短縮**: コンタクトが自動的にエージェントに接続されるため、応答までの時間が短縮
- **エージェント稼働率の向上**: 手動承認の手間が省かれ、より多くのコンタクトを処理可能
- **チャネル別の最適化**: タスクやメールなど、即時対応が望ましいチャネルで特に効果的

### 技術面

- **API による一括管理**: UpdateUserConfig API で大規模なエージェント設定を自動化
- **既存ワークフローとの互換性**: 音声チャネルの既存 Auto-Accept 設定を維持しながら、他チャネルを追加設定可能
- **きめ細かい制御**: チャネルとコールバックの組み合わせで柔軟な設定が可能

## デメリット・制約事項

### 制限事項

- Auto-Accept を有効にすると、エージェントがコンタクトを選択的に拒否する機能が無効化される
- エージェントの準備状態を適切に管理する運用プロセスが必要

### 考慮すべき点

- 音声通話の Auto-Accept は、エージェントが通話準備ができていない状態で接続されるリスクがあるため、慎重に検討が必要
- チャネルごとの設定を変更する際は、エージェントへの周知と研修が推奨される

## ユースケース

### ユースケース 1: タスク処理の高速化

**シナリオ**: バックオフィス業務で大量のタスクを処理するエージェントが、手動承認なしで次々とタスクに取り組みたい場合

**実装例**:
```json
{
  "AutoAcceptConfigs": [
    {"Channel": "TASK", "AutoAccept": true},
    {"Channel": "VOICE", "AutoAccept": false}
  ]
}
```

**効果**: タスクが自動的にエージェントに割り当てられ、処理スループットが向上。音声通話はエージェントの準備ができてから接続

### ユースケース 2: メール対応の効率化

**シナリオ**: メール専任のエージェントチームで、受信メールを即座にエージェントに振り分けたい場合

**実装例**:
```json
{
  "AutoAcceptConfigs": [
    {"Channel": "EMAIL", "AutoAccept": true},
    {"Channel": "CHAT", "AutoAccept": true}
  ]
}
```

**効果**: メールとチャットが自動承認されることで、エージェントの応答開始時間が短縮され、SLA 遵守率が向上

## 料金

Auto-Accept 設定機能の利用に追加料金は発生しません。Amazon Connect の標準料金が適用されます。

## 利用可能リージョン

Amazon Connect が提供されているすべての AWS リージョンで利用可能です。

## 関連サービス・機能

- **Amazon Connect ACW タイムアウト**: 同日リリースされた、チャネルごとの ACW タイムアウト設定機能
- **Amazon Connect Audio Enhancement**: エージェント音声のノイズ抑制機能
- **Amazon Connect ルーティングプロファイル**: コンタクトルーティングの優先順位設定

## 参考リンク

- 📊 [インフォグラフィック](https://takech9203.github.io/awsnews-summary/20260211-amazon-connect-omnichannel-auto-accept.html)
- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-connect-omnichannel-auto-accept/)
- [Amazon Connect 管理者ガイド - Auto-Accept 設定](https://docs.aws.amazon.com/connect/latest/adminguide/enable-auto-accept.html)
- [Amazon Connect ウェブサイト](https://aws.amazon.com/connect/)

## まとめ

Amazon Connect のオムニチャネル Auto-Accept 設定は、コンタクトセンターの応答効率を大幅に改善する機能です。特にタスクやメールなどの非音声チャネルで Auto-Accept を有効にすることで、エージェントの手動操作を削減し、より迅速な顧客対応を実現できます。ACW タイムアウト設定と組み合わせて活用することを推奨します。
