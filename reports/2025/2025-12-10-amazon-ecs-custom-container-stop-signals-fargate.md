# Amazon ECS - Fargate でのカスタムコンテナ停止シグナルサポート

**リリース日**: 2025 年 12 月 10 日
**サービス**: Amazon Elastic Container Service (Amazon ECS)
**機能**: AWS Fargate でのカスタムコンテナ停止シグナルサポート

## 概要

Amazon ECS が AWS Fargate 上で実行される Linux タスクに対して、カスタムコンテナ停止シグナルをサポートするようになりました。この機能強化により、タスク停止時に Open Container Initiative (OCI) イメージで設定された停止シグナルが尊重されるようになり、各コンテナの優先する終了シグナルに合わせた Fargate タスクの終了が可能になります。

以前は、AWS Fargate 上の Amazon ECS タスクが停止されると、各 Linux コンテナは常に SIGTERM を受信し、設定されたタイムアウト後に SIGKILL が送信されていました。新しい動作では、Amazon ECS コンテナエージェントがコンテナイメージ設定から停止シグナルを読み取り、タスク停止時にそのシグナルを送信します。

**アップデート前の課題**

- Fargate タスク停止時は常に SIGTERM が送信されていた
- SIGQUIT や SIGINT を必要とするコンテナが正しく終了できなかった
- アプリケーション固有のグレースフルシャットダウンが困難だった


**アップデート後の改善**

- OCI イメージで設定された STOPSIGNAL が尊重される
- SIGQUIT、SIGINT などのカスタムシグナルをサポート
- アプリケーション固有のグレースフルシャットダウンが可能

## サービスアップデートの詳細

### 主要機能

1. **カスタム停止シグナルサポート**
   - OCI イメージの STOPSIGNAL 設定を尊重
   - SIGTERM 以外のシグナルをサポート
   - コンテナイメージ設定からシグナルを自動読み取り

2. **グレースフルシャットダウンの改善**
   - アプリケーション固有の終了処理が可能
   - データ損失や破損のリスクを軽減
   - 適切なクリーンアップ処理の実行

3. **後方互換性**
   - STOPSIGNAL 未設定の場合は従来通り SIGTERM を送信
   - 既存のワークロードに影響なし

## 技術仕様

### サポートされる停止シグナル

| シグナル | 説明 |
|---------|------|
| SIGTERM | デフォルト（設定なしの場合） |
| SIGQUIT | コアダンプ付き終了 |
| SIGINT | 割り込み（Ctrl+C 相当） |
| SIGUSR1 | ユーザー定義シグナル 1 |
| SIGUSR2 | ユーザー定義シグナル 2 |

### タスク停止フロー

| ステップ | 以前の動作 | 新しい動作 |
|---------|-----------|-----------|
| 1 | SIGTERM 送信 | 設定されたシグナル送信 |
| 2 | タイムアウト待機 | タイムアウト待機 |
| 3 | SIGKILL 送信 | SIGKILL 送信 |

## 設定方法

### 前提条件

1. OCI 準拠のコンテナイメージ
2. AWS Fargate 上の Amazon ECS タスク
3. Linux コンテナ

### 手順

#### ステップ 1: Dockerfile での STOPSIGNAL 設定

```dockerfile
FROM nginx:latest

# カスタム停止シグナルを設定
STOPSIGNAL SIGQUIT

# アプリケーション設定
COPY nginx.conf /etc/nginx/nginx.conf
```

Dockerfile に STOPSIGNAL 命令を追加して、コンテナの停止シグナルを指定します。

#### ステップ 2: コンテナイメージのビルドとプッシュ

```bash
# イメージをビルド
docker build -t my-app:latest .

# ECR にプッシュ
aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin 123456789012.dkr.ecr.ap-northeast-1.amazonaws.com
docker tag my-app:latest 123456789012.dkr.ecr.ap-northeast-1.amazonaws.com/my-app:latest
docker push 123456789012.dkr.ecr.ap-northeast-1.amazonaws.com/my-app:latest
```

STOPSIGNAL を設定したイメージをビルドし、Amazon ECR にプッシュします。

#### ステップ 3: ECS タスク定義の更新

タスク定義で新しいイメージを参照します。ECS は自動的にイメージから STOPSIGNAL を読み取ります。

## メリット

### ビジネス面

- **サービス品質向上**: 適切なシャットダウンによるユーザー体験の改善
- **データ整合性**: グレースフルシャットダウンによるデータ損失防止
- **運用効率化**: アプリケーション固有の終了処理の自動化

### 技術面

- **柔軟性**: アプリケーションに最適なシグナルを選択可能
- **互換性**: 既存のコンテナイメージ設定をそのまま活用
- **標準準拠**: OCI 標準に準拠した動作

## デメリット・制約事項

### 制限事項

- Linux コンテナのみサポート（Windows コンテナは対象外）
- Fargate プラットフォームのみ（EC2 起動タイプは別途対応）

### 考慮すべき点

- 既存のイメージに STOPSIGNAL が設定されているか確認
- タイムアウト設定との整合性を確認

## ユースケース

### ユースケース 1: Nginx のグレースフルシャットダウン

**シナリオ**: Nginx コンテナを停止する際に、既存の接続を適切に処理したい

**実装例**:
```dockerfile
STOPSIGNAL SIGQUIT
```

**効果**: SIGQUIT により Nginx がグレースフルシャットダウンを実行し、既存の接続を完了してから終了

### ユースケース 2: Java アプリケーションのシャットダウンフック

**シナリオ**: Java アプリケーションでシャットダウンフックを確実に実行したい

**実装例**:
```dockerfile
STOPSIGNAL SIGTERM
```

**効果**: SIGTERM により JVM のシャットダウンフックが実行され、リソースの適切なクリーンアップが可能

### ユースケース 3: カスタムシグナルハンドラー

**シナリオ**: アプリケーション固有のシグナルハンドラーを使用したい

**実装例**:
```dockerfile
STOPSIGNAL SIGUSR1
```

**効果**: SIGUSR1 によりアプリケーション固有の終了処理を実行

## 料金

この機能は追加料金なしで利用できます。通常の AWS Fargate の料金のみが適用されます。

## 利用可能リージョン

すべての AWS リージョンで利用可能です。

## 関連サービス・機能

- **Amazon ECS**: コンテナオーケストレーションサービス
- **AWS Fargate**: サーバーレスコンテナ実行環境
- **Amazon ECR**: コンテナイメージレジストリ

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/12/amazon-ecs-custom-container-stop-signals-fargate/)
- [ECS 開発者ガイド - タスクライフサイクル](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-lifecycle-explanation.html)
- [Docker STOPSIGNAL ドキュメント](https://docs.docker.com/reference/dockerfile/#stopsignal)

## まとめ

Amazon ECS が AWS Fargate 上でカスタムコンテナ停止シグナルをサポートしました。OCI イメージで設定された STOPSIGNAL が尊重されるようになり、SIGQUIT や SIGINT などのシグナルを使用したグレースフルシャットダウンが可能になります。Nginx などの特定のシグナルを必要とするアプリケーションを Fargate で実行する場合に特に有用です。
