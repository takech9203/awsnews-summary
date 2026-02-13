# AWS 週次アップデートサマリー (2026年1月16日〜22日)

**期間**: 2026年1月16日 (木) 〜 2026年1月22日 (水)
**総アップデート数**: 約 40 件 (除外ルール適用後の主要アップデート)

## 今週のハイライト

### 🤖 AI/ML

1. **Amazon Bedrock AgentCore Browser - カスタムブラウザ拡張機能サポート** (1/22)
   - Chrome 互換の拡張機能を S3 経由で自動インストール可能に
   - カスタム認証フロー、自動テスト、パフォーマンス最適化のユースケースに対応
   - セキュアな AgentCore 環境内でサードパーティツールを統合可能
   - [詳細レポート](2026-01-22-amazon-bedrock-agentcore-browser-custom-extensions.md)

2. **Amazon Bedrock Reserved Tier 拡大** (1/16, 1/21)
   - Claude Opus 4.5 および Haiku 4.5 で Reserved Tier が利用可能に (1/16)
   - Claude Sonnet 4.5 が AWS GovCloud (US-West) で Reserved Tier 利用可能に (1/21)
   - 予測可能なパフォーマンスと保証された tokens-per-minute 容量を提供
   - 入力と出力トークンの容量を個別に設定可能

### 💾 データベース・ストレージ

3. **Amazon RDS Blue/Green Deployments - 高速切り替え** (1/20)
   - 切り替え時のダウンタイムを通常 5 秒以下に短縮
   - AWS Advanced JDBC Driver 使用時は約 2 秒以下
   - 単一リージョン構成で、メジャーバージョンアップグレードやスケーリング時の影響を最小化

4. **Amazon RDS for SQL Server - 差分およびトランザクションログ復元の強化** (1/21)
   - Multi-AZ およびリードレプリカ構成のインスタンスで直接復元が可能に
   - 以前は Single-AZ への変換が必要だったが、変換不要で復元時間を短縮

5. **Amazon EMR Serverless - AWS KMS カスタマー管理キーサポート** (1/21)
   - ローカルディスクの暗号化に AWS KMS カスタマー管理キー (CMK) を使用可能
   - 規制要件への対応が容易になり、暗号化戦略の制御を強化

6. **Amazon RDS および Aurora - 新しいインスタンスタイプのリージョン拡大** (1/20)
   - R8g (Graviton4) インスタンスが追加のアジア太平洋リージョンで利用可能に
   - R7g, R7i インスタンスが追加リージョンで利用可能に
   - Graviton4 ベースの R8g は Graviton3 比で最大 40% のパフォーマンス向上

### 📊 分析・BI

7. **Amazon QuickSight - ダッシュボードのカスタマイズ機能拡張** (1/20)
   - テーブルおよびピボットテーブルで、読者がフィールドの追加・削除、集計の変更、書式変更が可能に
   - ダッシュボード作成者による更新なしで、読者が独自にデータビューをカスタマイズ可能

8. **Amazon Quick Suite SPICE - データセット機能強化** (1/20)
   - データセットサイズの上限が 1 TB から 2 TB に倍増
   - 取り込み速度が最適化され、データ読み込みが高速化
   - 文字列長の上限が 2K から 64K Unicode 文字に拡大
   - タイムスタンプのサポート範囲が年 1400 から年 0001 に拡大

9. **AWS Clean Rooms - SQL ヒントサポート** (1/21)
   - ジョインおよびパーティションヒントをサポート
   - ブロードキャストジョインヒントで大規模テーブルのジョインを最適化
   - パーティションヒントで並列処理のためのデータ分散を改善

10. **AWS Glue - ニュージーランドリージョンで利用可能** (1/20)
    - Asia Pacific (New Zealand) リージョンで AWS Glue が利用可能に
    - データソースに近い場所で ETL ワークロードを実行可能

### 🖥️ コンピューティング

11. **Amazon EC2 G7e インスタンス - GA** (1/20)
    - NVIDIA RTX PRO 6000 Blackwell Server Edition GPU 搭載
    - G6e と比較して最大 2.3 倍の推論パフォーマンス
    - LLM、マルチモーダル生成 AI、空間コンピューティングワークロードに最適
    - US East (N. Virginia) および US East (Ohio) で利用可能

12. **Amazon EC2 C8gn インスタンス - 追加リージョン** (1/21)
    - AWS Graviton4 搭載のネットワーク最適化インスタンス
    - C7gn と比較して最大 30% のコンピュートパフォーマンス向上
    - 最大 600 Gbps のネットワーク帯域幅 (EC2 ネットワーク最適化インスタンスで最高)
    - Asia Pacific (Mumbai), Africa (Cape Town), Europe (Ireland, London), Canada West (Calgary) で利用可能

13. **Amazon EC2 High Memory U7i インスタンス - 追加リージョン** (1/16)
    - u7i-6tb.112xlarge が追加のリージョンで利用可能
    - u7in-16tb.224xlarge が AWS GovCloud (US-East) で利用可能
    - SAP HANA、Oracle、SQL Server などのミッションクリティカルなインメモリデータベースに最適

### 🔒 セキュリティ・ガバナンス

14. **AWS Resource Control Policies (RCPs) - サービスサポート拡大** (1/22)
    - Amazon Cognito および Amazon CloudWatch Logs のサポートを追加
    - データペリメーターの構築と、組織全体でのベースラインセキュリティ基準の適用が容易に

15. **AWS IAM - アクセス拒否エラーメッセージの詳細化** (1/21)
    - アクセス拒否エラーに、IAM および Organizations ポリシーの ARN を含めるように改善
    - 同じタイプの複数ポリシーがある場合でも、どのポリシーが拒否の原因かを迅速に特定可能
    - SCP、RCP、アイデンティティベースポリシー、セッションポリシー、パーミッション境界に対応

### 🔌 統合・メッセージング

16. **Amazon MQ - RabbitMQ での JMS サポート** (1/22)
    - RabbitMQ 4 ブローカーが JMS 1.1、2.0、3.1 アプリケーションをサポート
    - RabbitMQ JMS Topic Exchange プラグインがデフォルトで有効
    - JMS メッセージを AMQP キューに送信、または AMQP キューから消費可能
    - JMS ワークロードから AMQP ワークロードへの移行や相互運用が容易に

### 🚀 インフラストラクチャ

17. **AWS Outposts racks - 第2世代の国際展開** (1/21)
    - 20 カ国で第2世代 Outposts racks が利用可能に
    - アルゼンチン、バングラデシュ、コロンビア、インド、メキシコ、南アフリカ、韓国、台湾、タイなど
    - C7i、M7i、R7i インスタンスをサポートし、第1世代比で最大 40% のパフォーマンス向上

18. **AWS Outposts racks - 複数 LGW ルーティングドメインサポート** (1/16)
    - Outpost あたり最大 10 個の分離されたルーティングドメインを作成可能
    - 各ルーティングドメインは独立したルートテーブルと BGP セッションを持つ
    - 同じ Outpost を共有する異なる部門やビジネスユニット間でトラフィックを分離

19. **AWS Outposts racks - 第2世代の追加リージョンサポート** (1/16)
    - South America (Sao Paulo) および Europe (Stockholm) リージョンでサポート
    - レイテンシーとデータレジデンシーのニーズに応じて、接続先リージョンを最適化可能

20. **Amazon EVS - VCF および VMware ESX バージョン選択サポート** (1/20)
    - 環境作成時に VMware Cloud Foundation (VCF) バージョンを指定可能
    - ホスト追加時に ESX バージョンを選択可能
    - VCF 5.2.2 での新規環境デプロイメントをサポート

### 📦 コンテナ

21. **Amazon ECR - クロスリポジトリレイヤー共有** (1/20)
    - blob mounting により、レジストリ内の複数リポジトリ間でイメージレイヤーを共有可能
    - 既存レイヤーを再利用することで、イメージプッシュの高速化とストレージコストの削減を実現
    - レジストリレベルの設定で有効化

### 🔧 運用管理

22. **Instance Scheduler on AWS - スケーリングと信頼性の強化** (1/21)
    - AWS タグイベントの追跡により、スケジューリング操作のシーケンス化と分散を改善
    - スポークアカウント内でのセルフサービストラブルシューティングをサポート
    - EC2 insufficient capacity エラーの自動リトライフロー (代替インスタンスタイプを使用)
    - スケジューリングイベント用の専用 EventBridge バスを自動作成

23. **Amazon SageMaker HyperPod - ライフサイクルスクリプトのデバッグ強化** (1/21)
    - ライフサイクルスクリプトのエラーメッセージに CloudWatch ログ情報を含めるように改善
    - SageMaker コンソールから CloudWatch ログストリームへ直接移動可能
    - ライフサイクルスクリプトの実行進捗を追跡するマーカーを CloudWatch ログに追加

### 🌐 ネットワーキング・接続

24. **Amazon Connect - ランダムサンプリング評価** (1/21)
    - エージェントの連絡先からランダムサンプルを自動選択して評価可能
    - 組合協定、規制、内部ガイドラインに従った公平なコーチングフィードバックを提供
    - オーディオ録音、画面録画、トランスクリプトの有無などでフィルタリング可能

25. **AWS Transfer Family - Terraform モジュールの Web アプリサポート** (1/21)
    - Terraform モジュールで Transfer Family Web アプリのデプロイメントをサポート
    - IAM Identity Center を使用したフェデレーション認証と S3 Access Grants による詳細なアクセス制御
    - インフラストラクチャアズコードで一貫性のある反復可能なデプロイメントを実現

### 📊 その他の注目アップデート

- **Amazon CloudWatch Database Insights - オンデマンド分析の追加リージョン** (1/20): ニュージーランド、台北、タイ、メキシコで利用可能
- **SageMaker Unified Studio - クロスリージョンサブスクリプション** (1/20): IDC ベースドメインでサポート
- **AWS IoT Device Management - 中東 (UAE) リージョン** (1/20): マネージド統合機能が利用可能
- **Amazon RDS for Oracle - Standard Edition 2 でベアメタルインスタンスサポート** (1/20): BYOL ライセンスで 25% のコスト削減
- **Amazon Corretto - 2026年1月四半期アップデート** (1/20): セキュリティおよび重要なアップデート
- **Amazon MWAA - タイリージョンで利用可能** (1/16): Apache Airflow のマネージドサービス
- **Amazon S3 Storage Lens - AWS GovCloud (US) で利用可能** (1/16): 組織全体のストレージ可視性を提供

## リージョン拡大 (重要なインスタンスタイプ)

### 東京リージョン含む主要リージョン拡大
特に東京リージョンへの拡大や、新しいインスタンスタイプのリージョン拡大を確認しましたが、今週は該当するアップデートはありませんでした。

### 新インスタンスタイプのリージョン拡大
- **EC2 C8i**: Europe (London) で利用可能 (1/22)
- **EC2 C8gn (Graviton4)**: Mumbai, Cape Town, Ireland, London, Calgary で利用可能 (1/21)
- **EC2 High Memory U7i**: 追加リージョンで利用可能 (1/16)
- **RDS/Aurora R8g (Graviton4)**: Hong Kong, Osaka, Jakarta で利用可能 (1/20)

## 今週の傾向

### AI/ML の継続的な拡張
Amazon Bedrock を中心に、AI エージェントやブラウザ自動化の機能が強化されています。特に AgentCore Browser のカスタム拡張機能サポートは、複雑な自動化ワークフローの実現を可能にする重要なアップデートです。

### データベースの運用効率化
RDS Blue/Green Deployments の高速切り替えや、SQL Server の復元機能強化など、ダウンタイムを最小化し、運用効率を向上させるアップデートが目立ちました。

### Graviton4 の展開拡大
EC2 C8gn や RDS R8g など、Graviton4 ベースのインスタンスが追加リージョンで利用可能になり、パフォーマンスとコストの最適化の選択肢が広がっています。

### セキュリティとガバナンスの強化
Resource Control Policies のサービスサポート拡大や、IAM エラーメッセージの詳細化など、セキュリティとトラブルシューティングの改善が進んでいます。

## まとめ

今週は、AI/ML、データベース、コンピューティング、セキュリティなど、幅広い分野で重要なアップデートが発表されました。特に、Amazon Bedrock AgentCore Browser のカスタム拡張機能サポートや、RDS Blue/Green Deployments の高速切り替えは、実運用での大きな価値を提供します。Graviton4 ベースのインスタンスの展開も拡大しており、パフォーマンスとコスト効率の向上を実現できます。

各アップデートの詳細については、リンク先のドキュメントや個別レポートを参照してください。

## 参考リンク

- [AWS What's New](https://aws.amazon.com/new/)
- [AWS Blog](https://aws.amazon.com/blogs/)
- [詳細レポート一覧](.)
