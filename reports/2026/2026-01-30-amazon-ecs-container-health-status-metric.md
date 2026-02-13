# Amazon ECS - コンテナ健全性ステータスの CloudWatch メトリクス

**リリース日**: 2026 年 1 月 30 日
**サービス**: Amazon Elastic Container Service (ECS)
**機能**: コンテナ健全性ステータスの CloudWatch メトリクス

## 概要

Amazon ECS がコンテナの健全性ステータスを CloudWatch メトリクスとして発行するようになり、コンテナ環境の監視可能性が大幅に向上しました。顧客は CloudWatch Container Insights を通じてコンテナの運用健全性を追跡でき、不健全なコンテナに対してアラームを設定し、プロアクティブに対応することが可能になりました。

このメトリクスは UnHealthyContainerHealthStatus という新しいメトリクスで ECS/ContainerInsights ネームスペースで発行され、クラスタ、サービス、タスク、コンテナレベルの複数の粒度で監視できます。

**アップデート前の課題**

- コンテナの健全性ステータスの可視性が限定的で、問題検出に遅延が発生していた
- CloudWatch メトリクスでコンテナ健全性を追跡できず、アラームを設定できなかった
- 不健全なコンテナの検出と対応に手動監視が必要だった

**アップデート後の改善**

- CloudWatch メトリクスでコンテナ健全性を自動的に追跡可能に
- コンテナ健全性に基づいてアラームを設定でき、自動化された対応が実現可能
- 複数の粒度でコンテナ健全性を監視でき、問題の原因特定が容易に

## サービスアップデートの詳細

### 主要機能

1. **UnHealthyContainerHealthStatus メトリクス**
   - 値: 0 (HEALTHY) または 1 (UNHEALTHY)
   - ECS/ContainerInsights ネームスペースで発行
   - リアルタイムの健全性ステータス報告

2. **複数粒度での監視**
   - クラスタレベル: クラスタ全体の健全性サマリー
   - サービスレベル: 特定サービスの健全性追跡
   - タスクレベル: 個別タスクの健全性監視
   - コンテナレベル: 個別コンテナの詳細な健全性把握

3. **EMF ログ統合**
   - 組み込みメトリクス形式 (EMF) ログで詳細コンテキスト提供
   - 健全性チェック評価中の UNKNOWN 状態での詳細情報取得可能
   - ログとメトリクスの統合分析が可能

4. **CloudWatch アラーム対応**
   - メトリクスに基づいてアラームを設定可能
   - 不健全なコンテナ検出時に通知・自動対応を実装可能
   - チーム即座のアクションが実現可能に

## 技術仕様

### メトリクス仕様

| 項目 | 詳細 |
|------|------|
| メトリクス名 | UnHealthyContainerHealthStatus |
| ネームスペース | ECS/ContainerInsights |
| 値の範囲 | 0 (HEALTHY), 1 (UNHEALTHY) |
| ログ形式 | CloudWatch Logs、EMF ログ |
| 更新頻度 | リアルタイム |

### 対応リージョン

Amazon ECS Container Insights がサポートされているすべての AWS リージョン

## 設定方法

### 前提条件

1. ECS クラスタへの管理者アクセス
2. Container Insights with enhanced observability の有効化
3. タスク定義でのコンテナヘルスチェック設定

### 手順

#### ステップ1: Container Insights with enhanced observability の有効化

ECS クラスタ設定で Container Insights を有効化し、enhanced observability オプションを選択します。

#### ステップ2: タスク定義でヘルスチェックを設定

タスク定義でコンテナのヘルスチェックを定義します。ヘルスチェックコマンド、インターバル、タイムアウトなどを設定します。

#### ステップ3: CloudWatch メトリクス確認

CloudWatch コンソールで UnHealthyContainerHealthStatus メトリクスが発行されていることを確認します。

#### ステップ4: アラーム設定

CloudWatch アラームを設定し、コンテナが UNHEALTHY 状態になった場合に通知または自動対応を実装します。

## メリット

### 運用面

- **可視性向上**: リアルタイムでコンテナ健全性が把握可能
- **問題検出の迅速化**: メトリクスとアラームで即座に問題を検出
- **自動化対応**: アラーム連携で自動的に対応ワークフローをトリガー可能

### 信頼性面

- **アプリケーション信頼性向上**: 不健全なコンテナを素早く検出・対応できる
- **ダウンタイム削減**: プロアクティブな監視により障害時間を短縮

### コスト効率

- **手動監視削減**: 自動化された監視により運用コストを削減

## 制限事項

- Container Insights with enhanced observability の有効化に伴うコスト増加
- メトリクスはコンテナヘルスチェックが定義されている場合のみ発行

## ユースケース

### ユースケース1: 本番環境の高可用性アプリケーション

**シナリオ**: 重要なビジネスアプリケーションが複数の ECS サービスで稼働している場合、各コンテナの健全性をリアルタイムで監視し、問題時に自動化された対応が必要。

**効果**: 不健全なコンテナを素早く検出し、自動的に新しいコンテナに置き換えることで、アプリケーション可用性を 99.9% 以上に維持

### ユースケース2: マイクロサービスアーキテクチャの監視

**シナリオ**: 複数のマイクロサービスが多くのコンテナで構成されている場合、サービスレベルまたはコンテナレベルでの健全性把握と、サービス間のヘルスチェック連携が必要。

**効果**: サービス全体の健全性を一元監視でき、障害の初期検出と複数サービス間の連鎖障害防止が実現可能

### ユースケース3: 自動スケーリング連携

**シナリオ**: ECS オートスケーリングと連携して、不健全なタスクを自動的に終了し、新しいタスクを起動したい場合。

**効果**: メトリクスに基づいた自動スケーリングポリシーが実装でき、容量管理の自動化が実現可能

## 利用可能リージョン

Amazon ECS Container Insights がサポートされているすべての AWS リージョン

## 関連サービス・機能

- **CloudWatch Container Insights**: コンテナ運用の可視化基盤
- **CloudWatch アラーム**: メトリクスに基づいた通知
- **AWS Lambda**: アラーム連携による自動対応実装
- **Amazon SNS**: アラーム通知の配信

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/amazon-ecs-container-health-status-metric/)
- [Amazon ECS コンテナヘルスチェック ドキュメント](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/healthcheck.html)
- [CloudWatch Container Insights ドキュメント](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Container-Insights-enhanced-observability-metrics-ECS.html)
- [Amazon ECS](https://aws.amazon.com/ecs/)

## まとめ

コンテナ健全性ステータスの CloudWatch メトリクス化により、ECS ユーザーは運用可視性を大幅に向上させ、コンテナ環境の信頼性をさらに高めることができます。本番環境でのマイクロサービス運用において、この機能は障害検出と自動対応の自動化を実現する重要な手段となります。
