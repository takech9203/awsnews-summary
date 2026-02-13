# AWS 週次アップデートサマリー - 2026年第4週

**期間**: 2026 年 1 月 18 日〜1 月 24 日
**総アップデート数**: 41 件 (過去 7 日間)

## 今週のハイライト

### 🔥 注目のアップデート

1. **Amazon Route 53 Domains - 10 種類の新 TLD サポート**
   - .ai、.nz、.shop、.bot など業界特化型ドメインを Route 53 で直接登録可能に
   - AI 企業や E コマース向けのブランディング強化に最適

2. **Amazon RDS for Oracle - マルチテナント構成でのレプリカサポート**
   - CDB/PDB 構成でリードレプリカを作成可能に
   - 読み取りスケーリングと DR 戦略を両立

3. **Amazon MQ for RabbitMQ - JMS 仕様サポート**
   - RabbitMQ 4 で JMS 1.1/2.0/3.1 アプリケーションを実行可能
   - 既存 JMS アプリケーションをコード変更なしで移行可能

4. **AWS IAM - アクセス拒否エラーメッセージの改善**
   - エラーメッセージにポリシー ARN を含めることでトラブルシューティングを迅速化
   - SCP、RCP、アイデンティティベースポリシーなどに対応

5. **Amazon EC2 M4 Max Mac インスタンス GA**
   - Apple M4 Max チップ搭載の新しい Mac インスタンスが一般提供開始
   - macOS ワークロードのパフォーマンスが大幅向上

## カテゴリ別アップデート

### 🖥️ Compute (11 件)

**EC2 インスタンス**
- **EC2 Auto Scaling**: グループ削除保護の新メカニズム導入
- **M4 Max Mac インスタンス**: 一般提供開始 (Apple M4 Max チップ)
- **C8i インスタンス**: ロンドン、シドニー、フランクフルトリージョンで利用可能に
- **Graviton4 EBS 最適化インスタンス**: 48xlarge および metal-48xl サイズが利用可能に

### 🗄️ Database (4 件)

**RDS & データベース**
- **Amazon Neptune Analytics**: 7 つの追加リージョンで利用可能に
- **RDS for SQL Server**: 差分およびトランザクションログリストアのサポート強化
- **RDS Blue/Green Deployments**: ダウンタイムを 5 秒未満に短縮
- **CloudWatch Database Insights**: 4 つの追加リージョンでオンデマンド分析が利用可能に

### 🤖 AI & Machine Learning (7 件)

**Amazon Bedrock & SageMaker**
- **Amazon Bedrock AgentCore Browser**: カスタムブラウザ拡張機能をサポート
- **Amazon SageMaker HyperPod**: ライフサイクルスクリプトのデバッグ機能強化
- **AWS Security Agent**: GitHub Enterprise Cloud をサポート

### 🌐 Networking (1 件)

**Route 53**
- **Route 53 Domains**: 10 種類の新しい TLD (.ai、.nz、.shop、.bot など) をサポート

### 🔐 Security (2 件)

**IAM & セキュリティ**
- **AWS IAM**: アクセス拒否エラーメッセージにポリシー ARN を追加
- **AWS Security Agent**: GitHub Enterprise Cloud サポート

### 🔄 Application Integration (1 件)

**メッセージング**
- **Amazon MQ for RabbitMQ**: JMS 仕様サポート (JMS 1.1/2.0/3.1)

### 🏢 End User Computing & Analytics (2 件)

**ワークスペースとアナリティクス**
- **Amazon WorkSpaces**: Microsoft Office、Visio、Project 2024 アプリが利用可能に
- **Amazon Connect**: エージェントコンタクトのランダムサンプリング自動選択機能

### 🏭 Hybrid & Edge (3 件)

**AWS Outposts**
- **AWS Outposts**: ルワンダでラックが利用可能に
- **第 2 世代 Outposts ラック**: 20 か国以上で利用可能に
- **AWS Outposts**: 複数の LGW ルーティングドメインをサポート

### 🔧 その他のサービス (10 件)

**管理ツール & その他**
- **AWS Config**: 13 の新しいマネージドルールを追加
- **AWS Clean Rooms**: SQL での結合とパーティションヒントをサポート
- **Amazon EVS**: 複数の VMware NSX Edge Gateway をサポート
- **Instance Scheduler on AWS**: 拡張スケーリング、信頼性、イベント駆動自動化を追加

## リージョン拡大

今週は複数のサービスとインスタンスタイプがリージョン拡大しました。

### 新規インスタンスタイプの展開
- **C8i/C8i-flex インスタンス**: ロンドン、シドニー、フランクフルト
- **C8gn インスタンス**: 追加リージョン
- **G7e インスタンス**: 一般提供開始

### サービスのリージョン拡大
- **Amazon Neptune Analytics**: 7 つの追加リージョン
- **CloudWatch Database Insights**: 4 つの追加リージョン

## 主な技術的改善

### パフォーマンス
- **RDS Blue/Green Deployments**: ダウンタイムを 5 秒未満に短縮
- **Graviton4 インスタンス**: より大きなサイズ (48xlarge、metal-48xl) で EBS 最適化

### 開発者エクスペリエンス
- **IAM エラーメッセージ**: ポリシー ARN の追加でトラブルシューティングを迅速化
- **Amazon MQ**: JMS サポートによる既存アプリケーションの移行が容易に
- **SageMaker HyperPod**: デバッグ機能強化で開発効率向上

### セキュリティ
- **AWS Security Agent**: GitHub Enterprise Cloud サポートでセキュリティ管理を強化
- **IAM**: より詳細なアクセス拒否情報でセキュリティ監査を改善

## 今週の推奨アクション

1. **Route 53 の新 TLD を確認**
   - AI や E コマース関連のプロジェクトで .ai や .shop ドメインの活用を検討

2. **RDS Oracle マルチテナント環境の最適化**
   - 既存の CDB/PDB 構成にリードレプリカを追加して読み取りスケーリングを実装

3. **JMS アプリケーションの移行計画**
   - Amazon MQ for RabbitMQ の JMS サポートを活用した移行戦略を検討

4. **IAM トラブルシューティングプロセスの見直し**
   - 新しいポリシー ARN 情報を活用したトラブルシューティング手順を更新

5. **最新インスタンスタイプの評価**
   - M4 Max Mac、C8i、Graviton4 などの新インスタンスでワークロードをテスト

## 詳細レポート

今週作成された詳細レポート:
- [Amazon Route 53 Domains - 10 種類の新しい TLD サポート](2026-01-23-amazon-route-53-domains-adds-support-for-.ai-and-other-top-level-domains.md)
- [Amazon RDS for Oracle - マルチテナント構成でのレプリカサポート](2026-01-23-amazon-rds-for-oracle-replica-multi-tenant-configuration-support.md)
- [Amazon MQ for RabbitMQ - JMS 仕様サポート](2026-01-22-amazon-mq-jms-spec-rabbitmq.md)

## 参考リンク

- [AWS What's New](https://aws.amazon.com/new/)
- [AWS ブログ](https://aws.amazon.com/blogs/)
- [AWS ドキュメント](https://docs.aws.amazon.com/)

---

**次回の週次サマリー**: 2026 年 1 月 31 日 (第 5 週)
