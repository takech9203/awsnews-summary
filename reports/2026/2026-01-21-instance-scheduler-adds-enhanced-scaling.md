# Instance Scheduler on AWS - 機能強化

**リリース日**: 2026 年 1 月 21 日
**サービス**: Instance Scheduler on AWS
**機能**: スケーリング、信頼性、イベント駆動型自動化の強化

## 概要

Instance Scheduler on AWS が、AWS タグイベントの追跡、情報タグによるセルフサービストラブルシューティング、代替インスタンスタイプを使用した EC2 容量不足エラーのリトライフロー (オプション)、スケジューリングイベント用の専用 EventBridge バスの自動作成の機能を追加しました。

オーケストレーションおよびファンアウトメカニズムが再設計され、AWS タグイベントを追跡できるようになり、スケジューリング操作をより賢くシーケンスおよび分散できるようになりました。これにより、スケーリングパフォーマンスが向上し、コストスケーリングの懸念に対処します。分散クラウドエンジニアは、中央のクラウド管理者に依存することなく、スポークアカウントでリソースに適用される情報タグを通じてセルフサービストラブルシューティングを実行できるようになりました。

**アップデート前の課題**

- AWS タグイベントを追跡できず、新しくタグ付けされたリソースをスケジューリングに自動的に含めることができませんでした
- 分散クラウドエンジニアがトラブルシューティングを行うには、中央のクラウド管理者に依存する必要がありました
- EC2 の容量不足エラーが発生した場合、インスタンスの起動が失敗し、ワークロードが開始できませんでした
- スケーリング操作が増加すると、コストとパフォーマンスに影響がありました

**アップデート後の改善**

- AWS タグイベントを追跡し、新しくタグ付けされたリソースを自動的にスケジューリングに含めることができるようになりました
- 分散クラウドエンジニアは、リソースに適用される情報タグを通じてセルフサービストラブルシューティングを実行できるようになりました
- EC2 の容量不足エラーが発生した場合、代替インスタンスタイプで自動的にリトライし、ワークロードを開始できるようになりました
- 専用の EventBridge バスがスケジューリングイベント用に自動的に作成され、統合と自動化ワークフローが簡素化されました

## サービスアップデートの詳細

### 主要機能

1. **AWS タグイベントの追跡**
   - リソースにスケジュールタグが追加されると、Instance Scheduler が自動的に検出します
   - タグイベントに基づいて、スケジューリング操作をより賢くシーケンスおよび分散します
   - 新しくタグ付けされたリソースを手動で登録する必要がなくなりました

2. **セルフサービストラブルシューティング**
   - リソースに情報タグが適用され、スケジューリングのステータスや問題を表示します
   - 分散クラウドエンジニアは、中央のクラウド管理者に依存せずにトラブルシューティングを実行できます
   - スポークアカウントで直接問題を特定して解決できます

3. **EC2 容量不足エラーのリトライフロー (オプション)**
   - EC2 インスタンスの起動時に容量不足エラーが発生した場合、代替インスタンスタイプで自動的にリトライします
   - Availability Zone やリージョンで容量が制約されている場合でも、ワークロードを確実に開始できます
   - オプション機能として有効化できます

4. **専用 EventBridge バスの自動作成**
   - スケジューリングイベント用の専用 EventBridge バス (`IS-LocalEvents` および `IS-GlobalEvents`) が自動的に作成されます
   - 統合と自動化ワークフローが簡素化されます
   - スケジューリングイベントを監視し、他の AWS サービスと統合できます

## 技術仕様

### EventBridge バスの構造

| EventBridge バス | 説明 |
|------------------|------|
| IS-LocalEvents | 各管理リージョンにデプロイされ、そのリージョンのスケジューリングイベントを受信します |
| IS-GlobalEvents | ハブアカウントにデプロイされ、すべての `IS-LocalEvents` バスからイベントのコピーを受信します |

### イベントの種類

1. **スケジューリングイベント**
   - インスタンスの起動/停止操作
   - `requested_action` および `action_taken` の値が含まれます
   - 例: `start`, `stop`, `no_action`, `already_running`, `already_stopped`

2. **登録イベント**
   - リソースがスケジューリングに登録されたときに発行されます
   - タグイベントに基づいて自動的に発行されます

### 情報タグの例

```
Key: IS-SchedulingStatus
Value: Scheduled to start at 09:00 UTC
```

```
Key: IS-LastAction
Value: Started successfully at 2026-01-21 09:00:00 UTC
```

```
Key: IS-LastError
Value: InsufficientInstanceCapacity - Retrying with alternate instance type
```

## 設定方法

### 前提条件

1. Instance Scheduler on AWS がデプロイされていること
2. EC2 または RDS インスタンスにスケジュールタグが設定されていること
3. EventBridge へのアクセス権限があること

### 手順

#### ステップ 1: Instance Scheduler のデプロイまたはアップグレード

最新バージョンの Instance Scheduler をデプロイまたはアップグレードします。

```bash
# CloudFormation スタックの更新
aws cloudformation update-stack \
  --stack-name instance-scheduler \
  --template-url https://s3.amazonaws.com/solutions-reference/instance-scheduler-on-aws/latest/instance-scheduler-on-aws.template \
  --capabilities CAPABILITY_IAM
```

#### ステップ 2: EC2 容量不足エラーのリトライフローの有効化 (オプション)

CloudFormation スタックのパラメータで、容量不足エラーのリトライフローを有効化します。

```yaml
Parameters:
  EnableInsufficientCapacityRetry: true
  AlternateInstanceTypes: t3.medium,t3a.medium,t2.medium
```

#### ステップ 3: EventBridge ルールの作成

専用 EventBridge バスを使用して、スケジューリングイベントを監視するルールを作成します。

```bash
# EventBridge ルールの作成
aws events put-rule \
  --name instance-scheduler-events \
  --event-bus-name IS-GlobalEvents \
  --event-pattern '{
    "source": ["instance-scheduler"],
    "detail-type": ["Scheduling Event"]
  }' \
  --state ENABLED
```

#### ステップ 4: 情報タグの確認

スケジューリング対象のインスタンスに情報タグが適用されていることを確認します。

```bash
# インスタンスのタグを確認
aws ec2 describe-tags \
  --filters "Name=resource-id,Values=i-1234567890abcdef0"
```

このコマンドは、指定されたインスタンスのすべてのタグを表示し、情報タグが適用されているかを確認できます。

## メリット

### ビジネス面

- **運用効率の向上**: タグイベントの自動追跡により、手動登録が不要になり、運用効率が向上します
- **ダウンタイムの削減**: EC2 容量不足エラーのリトライフローにより、ワークロードを確実に開始できます
- **コスト削減**: スケーリングパフォーマンスが向上し、コストスケーリングの懸念に対処します

### 技術面

- **自動化の向上**: タグイベントに基づいてスケジューリング操作を自動的に実行します
- **セルフサービス**: 分散クラウドエンジニアがトラブルシューティングを独立して実行できます
- **イベント駆動型**: EventBridge バスを使用して、スケジューリングイベントを他の AWS サービスと統合できます

## デメリット・制約事項

### 制限事項

- EC2 容量不足エラーのリトライフローは、オプション機能として有効化する必要があります
- 代替インスタンスタイプは、事前に設定する必要があります
- EventBridge のイベント数に応じて、追加コストが発生する場合があります

### 考慮すべき点

- 代替インスタンスタイプを適切に設定して、ワークロードの要件を満たすようにしてください
- EventBridge ルールを作成して、スケジューリングイベントを監視および統合してください
- 情報タグを確認して、トラブルシューティング情報を取得してください

## ユースケース

### ユースケース 1: 開発環境の自動スケジューリング

**シナリオ**: 開発チームが新しい EC2 インスタンスを起動し、スケジュールタグを追加する。Instance Scheduler は、タグイベントを検出して自動的にスケジューリングに含める。

**実装例**:
1. 開発者が EC2 インスタンスを起動
2. スケジュールタグ `Schedule: dev-hours` を追加
3. Instance Scheduler がタグイベントを検出
4. 自動的にスケジューリングに含まれ、営業時間外に停止

**効果**: 手動登録が不要になり、開発者は自動的にコスト最適化の恩恵を受けられます。

### ユースケース 2: EC2 容量不足時の自動リトライ

**シナリオ**: Availability Zone で EC2 容量が不足しており、インスタンスの起動が失敗する。Instance Scheduler は、代替インスタンスタイプで自動的にリトライする。

**実装例**:
1. Instance Scheduler が t3.medium インスタンスの起動を試みる
2. InsufficientInstanceCapacity エラーが発生
3. 代替インスタンスタイプ (t3a.medium, t2.medium) で自動的にリトライ
4. t3a.medium で起動成功

**効果**: 容量不足エラーでもワークロードを確実に開始でき、信頼性が向上します。

### ユースケース 3: EventBridge を使用した通知

**シナリオ**: スケジューリングイベントを EventBridge で監視し、SNS 経由で通知を送信する。

**実装例**:
1. EventBridge ルールを作成して、スケジューリングイベントを検出
2. SNS トピックをターゲットとして設定
3. インスタンスが起動または停止されると、SNS 通知が送信される

**効果**: スケジューリング操作をリアルタイムで監視し、問題を迅速に検出できます。

## 料金

Instance Scheduler on AWS 自体に追加料金はかかりませんが、以下のサービスの使用に対して料金が発生します。

- AWS Lambda の実行
- Amazon DynamoDB のストレージとリクエスト
- Amazon EventBridge のイベント
- Amazon CloudWatch Logs のストレージ

詳細については、各サービスの料金ページを参照してください。

## 利用可能リージョン

Instance Scheduler on AWS は、すべての商用および オプトイン AWS リージョンで利用可能です。

## 関連サービス・機能

- **AWS Lambda**: Instance Scheduler のオーケストレーションを実行するサーバーレスコンピューティングサービス
- **Amazon DynamoDB**: スケジュール設定とレジストリを保存する NoSQL データベース
- **Amazon EventBridge**: スケジューリングイベントをルーティングするイベントバスサービス
- **Amazon CloudWatch**: スケジューリングログを保存および監視するサービス

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/instance-scheduler-adds-enhanced-scaling/)
- [Instance Scheduler on AWS Product Page](https://aws.amazon.com/solutions/implementations/instance-scheduler-on-aws/)
- [Instance Scheduler on AWS Documentation](https://docs.aws.amazon.com/solutions/latest/instance-scheduler-on-aws/)

## まとめ

Instance Scheduler on AWS の機能強化により、AWS タグイベントの追跡、セルフサービストラブルシューティング、EC2 容量不足エラーの自動リトライ、専用 EventBridge バスの自動作成が可能になりました。これにより、スケーリングパフォーマンスが向上し、運用効率が向上し、ワークロードの信頼性が向上します。Instance Scheduler を使用してコスト最適化を行っている組織は、この機能を活用してより効率的な運用を実現することをお勧めします。
