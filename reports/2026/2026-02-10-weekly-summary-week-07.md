# AWS アップデート週次サマリー - 2026 年第 7 週

**期間**: 2026 年 2 月 3 日 - 2026 年 2 月 9 日
**レポート作成日**: 2026 年 2 月 10 日

📊 [このサマリーのインフォグラフィックを見る](https://takech9203.github.io/awsnews-summary/20260210-weekly-summary-week-07.html)

## 週次ハイライト

今週は **21 件** の主要なアップデートが発表されました。特に AI/ML 機能の強化、Amazon Redshift の自動最適化機能、開発者体験の向上が目立ちました。

### 🏆 今週のトップアップデート

1. **Claude Opus 4.6 が Amazon Bedrock で利用可能に** - Anthropic の最新・最高知性モデルが Bedrock に登場
2. **Amazon Bedrock Structured Outputs** - JSON スキーマ準拠の出力を 100% 保証
3. **EC2 C8id/M8id/R8id インスタンス一般提供開始** - Intel Xeon 6 搭載、前世代比最大 43% 性能向上
4. **AWS Glue ネイティブ REST API コネクタ** - 任意の REST API からのデータ統合が簡素化
5. **Amazon Redshift 自動最適化機能の強化** - 追加コンピュートリソース割り当てとマルチクラスター対応

## カテゴリ別アップデート

### 🤖 AI/ML (5 件)

#### Claude Opus 4.6 - Amazon Bedrock で利用可能に 🔥
- **リリース日**: 2026 年 2 月 5 日
- **概要**: Anthropic の最新モデル Claude Opus 4.6 が Amazon Bedrock で利用可能に。コーディング、エンタープライズエージェント、プロフェッショナルワークに最適化された業界最高知性モデル
- **主要機能**:
  - 業界トップのエージェンティック性能 - 数十のツールを使用した複雑なタスクの自律的管理
  - 200K コンテキストトークン (1M プレビュー)
  - 複雑なコーディングプロジェクトのフルライフサイクル管理
  - サブエージェントの自動生成と監視削減
- **ユースケース**: 金融分析、サイバーセキュリティ、コンピュータ操作ワークフロー
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/2/claude-opus-4.6-available-amazon-bedrock/)

#### Amazon Bedrock - Structured Outputs 一般提供開始 🔥
- **リリース日**: 2026 年 2 月 4 日
- **概要**: モデル出力がユーザー定義の JSON スキーマに 100% 準拠することを保証。本番ワークフローの予測可能性と信頼性を向上
- **主要機能**:
  - JSON Schema による出力フォーマット定義
  - Strict ツール定義でツール呼び出しの仕様準拠を保証
  - Converse、ConverseStream、InvokeModel、InvokeModelWithResponseStream API でサポート
- **対象モデル**: Anthropic Claude 4.5 モデル、一部のオープンウェイトモデル
- **追加料金**: なし
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/structured-outputs-available-amazon-bedrock/)

#### Amazon Bedrock AgentCore Browser - ブラウザプロファイルサポート
- **リリース日**: 2026 年 2 月 6 日
- **概要**: ブラウザプロファイルで認証状態を複数セッション間で再利用可能に。セッションセットアップ時間を数分から数十秒に短縮
- **主要機能**:
  - Cookie とローカルストレージの永続化と再利用
  - 読み取り専用モードと永続モードの選択
  - 同一プロファイルを使用した並列処理
- **利用可能リージョン**: 14 リージョン（東京含む）
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-bedrock-agentcore-browser-profiles/)

#### Cartesia Sonic 3 TTS モデル - SageMaker JumpStart で利用可能に
- **リリース日**: 2026 年 2 月 4 日
- **概要**: 高自然性、高精度のストリーミングテキスト読み上げモデル。42 言語対応、100ms 未満のレイテンシ
- **主要機能**:
  - API パラメータと SSML タグによる音量、速度、感情の制御
  - 自然な笑い声サポート
  - ボイスエージェント向け安定ボイスと表現力豊かなキャラクターボイス
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/cartesia-sonic-3-on-sagemaker-jumpstart/)

#### AWS HealthOmics - Kiro Power と Kiro IDE 拡張機能
- **リリース日**: 2026 年 2 月 9 日
- **概要**: AI エージェント支援開発でバイオインフォマティクスワークフローの作成、実行、デバッグ、最適化を高速化
- **主要機能**:
  - Nextflow と WDL のシンタックスハイライト、コード補完
  - HealthOmics エンジン互換性チェック
  - パフォーマンス最適化レコメンデーション
  - 障害診断付き自動ラン分析
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/01/aws-healthomics-introduces-kiro-plugin-for-bioinformatics-workflow-development/)

### ☁️ コンピューティング (6 件)

#### Amazon EC2 C8id/M8id/R8id インスタンス一般提供開始 🔥
- **リリース日**: 2026 年 2 月 4 日
- **概要**: カスタム Intel Xeon 6 プロセッサ搭載の新インスタンスファミリー
- **主要機能**:
  - 前世代比最大 43% の性能向上、3.3 倍のメモリ帯域幅
  - 最大 384 vCPU、3TiB メモリ、22.8TB NVMe SSD（前世代の 3 倍）
  - I/O 集約型データベースワークロードで最大 46% 性能向上
  - Instance Bandwidth Configuration で帯域幅の 25% 柔軟割り当て
- **利用可能リージョン**: us-east-1、us-east-2、us-west-2、eu-central-1（R8id のみ）
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-ec2-c8id-m8id-r8id-instances/)

#### Amazon EC2 capacity blocks for ML - マルチアカウント共有 GA
- **リリース日**: 2026 年 2 月 5 日
- **概要**: ML 用キャパシティブロックを AWS Resource Access Manager 経由で複数アカウントで共有可能に
- **メリット**: GPU キャパシティの利用率最適化、コスト削減、ML インフラ投資の調整
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-capacity-blocks-multiple-accounts/)

#### Amazon WorkSpaces - Graphics G6/Gr6/G6f バンドル
- **リリース日**: 2026 年 2 月 5 日
- **概要**: EC2 G6 ファミリーベースの 12 の新しいグラフィックスバンドル
- **バンドル種類**:
  - G6: 1:4 vCPU-メモリ比率、グラフィックデザイン、CAD/CAM、ML モデルトレーニング向け
  - Gr6: 1:8 vCPU-メモリ比率、3D レンダリング、GIS 処理向け
  - G6f: フラクショナル GPU（1/8、1/4、1/2）、コスト効率の良い GPU アクセス
- **利用可能リージョン**: 13 リージョン（東京含む）
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-workspaces-personal-core-graphics-g6-gr6-g6f-bundles/)

#### Amazon ECS - NLB での Linear/Canary デプロイメント
- **リリース日**: 2026 年 2 月 4 日
- **概要**: Network Load Balancer を使用する ECS サービスで段階的トラフィックシフトをサポート
- **主要機能**:
  - 増分または小規模パーセンテージからのトラフィックシフト
  - CloudWatch アラームとの統合による自動ロールバック
  - TCP/UDP 接続、低レイテンシ、静的 IP アドレスが必要なワークロードに最適
- **ユースケース**: オンラインゲームバックエンド、金融取引システム、リアルタイムメッセージングサービス
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-ecs-nlb-linear-canary-deployments/)

#### AWS Batch - Amazon EKS の Unmanaged Compute Environments
- **リリース日**: 2026 年 2 月 4 日
- **概要**: EKS で Unmanaged Compute Environments をサポート。Kubernetes インフラの完全制御を維持しながら Batch のジョブオーケストレーションを活用
- **設定方法**: CreateComputeEnvironment API または Batch コンソールで EKS クラスターと名前空間を指定、kubectl ラベリングでノードを関連付け
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/aws-batch-on-eks-unmanaged-compute-environments/)

#### AWS Batch - Array Job Status Summary（ListJobs API）
- **リリース日**: 2026 年 2 月 3 日
- **概要**: ListJobs API レスポンスにアレイジョブのステータスサマリーを追加
- **新フィールド**:
  - `statusSummary`: 子ジョブの状態別カウント（SUBMITTED、PENDING、RUNNABLE、STARTING、RUNNING、SUCCEEDED、FAILED）
  - `statusSummaryLastUpdatedAt`: ステータス情報の鮮度を示すタイムスタンプ
- **メリット**: 単一 API コールで複数のアレイジョブの進捗監視が可能
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/01/aws-batch-array-job-status-summary/)

### 📊 データ統合・分析 (4 件)

#### AWS Glue - ネイティブ REST API コネクタ 🔥
- **リリース日**: 2026 年 2 月 5 日
- **概要**: 任意の REST API からデータを取得し、Glue ETL ジョブに統合可能に。カスタム JAR 不要
- **主要機能**:
  - 100 以上の非 AWS データソースへの既存接続を拡張
  - カスタムライブラリのインストール、更新、管理が不要
  - 新しいデータソースへの迅速な適応
- **利用可能リージョン**: AWS Glue が利用可能な全商用リージョン
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/aws-glue-rest-api-connector/)

#### Amazon Redshift - 自動最適化のための追加コンピュートリソース割り当て
- **リリース日**: 2026 年 2 月 9 日
- **概要**: Autonomics（ATO、ATS、Auto Vacuum、Auto Analyze）のために追加コンピュートリソースを割り当て可能に
- **主要機能**:
  - 高負荷時でもユーザーワークロードに影響を与えずに自動最適化を実行
  - プロビジョンドクラスターでのコスト制御機能
  - 新しい SYS_AUTOMATIC_OPTIMIZATION システムテーブルで詳細な可観測性
- **利用可能リージョン**: Amazon Redshift がサポートされる全リージョン
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-redshift-allocate-extra-compute-for-automatic-optimizations/)

#### Amazon Redshift - マルチクラスター環境の Autonomics
- **リリース日**: 2026 年 2 月 4 日
- **概要**: マルチクラスター環境での自動最適化機能。全コンシューマークラスターのクエリパターンを考慮
- **主要機能**:
  - ATO、ATS、Auto Vacuum、Auto Analyze がマルチクラスター対応
  - 特定のエンドポイントや AWS アカウントを除外する拒否リスト機能
  - 組織間データ共有シナリオに有用
- **追加料金**: なし
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-redshift-autonomics-for-multi-cluster/)

#### Apache Spark lineage - SageMaker Unified Studio（IDC ベースドメイン）GA
- **リリース日**: 2026 年 2 月 4 日
- **概要**: EMR-EC2、EMR-Serverless、EMR-EKS、AWS Glue での Spark ジョブのデータリネージをキャプチャ
- **主要機能**:
  - スキーマと変換のリネージキャプチャ
  - SageMaker Unified Studio でのビジュアルグラフ表示
  - API によるリネージクエリ
  - Spark ジョブ履歴間の変換比較
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/apache-spark-lineage-amazon-sageMaker-unified-studio/)

### 🔧 管理・ガバナンス (3 件)

#### AWS Config - 30 の新リソースタイプをサポート
- **リリース日**: 2026 年 2 月 6 日
- **概要**: Amazon EKS、Amazon Q、AWS IoT など主要サービスの 30 の追加リソースタイプをサポート
- **新リソースタイプの例**:
  - AWS::EKS::Nodegroup
  - AWS::QBusiness::Application
  - AWS::QuickSight::DataSet、Dashboard
  - AWS::IoT::TopicRule、BillingGroup
  - AWS::Glue::Crawler
  - AWS::SSM::PatchBaseline
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/aws-config-new-resource-types/)

#### AWS Network Firewall - 価格引き下げ
- **リリース日**: 2026 年 2 月 6 日
- **概要**: 2 つの価格改善を発表
- **改善内容**:
  - NAT Gateway 割引がセカンダリファイアウォールエンドポイントとのサービスチェーンにも適用
  - Advanced Inspection（TLS 検査）の追加データ処理料金（$0.001-$0.009/GB）を 13 リージョンで廃止
- **対象リージョン**: 東京、大阪、ソウル、シンガポール、シドニー、ムンバイ、香港、メルボルンなど
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/aws-network-firewall-new-price-reduction/)

#### Amazon EC2/VPC - セキュリティグループの関連リソース表示 GA
- **リリース日**: 2026 年 2 月 4 日
- **概要**: セキュリティグループに依存する全リソースを「Related resources」タブで一覧表示
- **メリット**:
  - 設定変更前に影響を受けるリソースを一目で把握
  - EC2、ENI、ElastiCache、RDS など複数サービスの個別確認が不要
  - 大規模デプロイメントでのセキュリティグループ管理を効率化
- **追加料金**: なし
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/aws-console-related-resources-generally-available/)

### 🔐 セキュリティ・ID (1 件)

#### AWS Builder ID - Sign in with Apple サポート
- **リリース日**: 2026 年 2 月 5 日
- **概要**: AWS Builder ID で Apple ID によるサインインに対応。既存の Google サインインに加え、Apple ユーザーにシームレスなアクセスを提供
- **対象サービス**: AWS Builder Center、AWS Training and Certification、AWS re:Post、AWS Startups、Kiro
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/aws-builder-id-sign-in-apple/)

### 📞 コンタクトセンター (1 件)

#### Amazon Connect Cases - CSV アップロードによる関連フィールドオプションマッピング
- **リリース日**: 2026 年 2 月 6 日
- **概要**: CSV ファイルで他のフィールド値に基づいて表示されるフィールドオプションを定義可能に
- **メリット**: 手動定義の代わりにファイルアップロードで関係性を大規模に定義、オンボーディングと設定時間を削減
- **利用可能リージョン**: 10 リージョン（東京含む）
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-connect-cases-csv-related-field-options/)

### 💼 エンドユーザーコンピューティング (1 件)

#### Amazon WorkSpaces Secure Browser - カスタムドメインサポート
- **リリース日**: 2026 年 2 月 6 日
- **概要**: 組織のブランディングに合わせたカスタムドメイン設定が可能に
- **設定方法**: WorkSpaces Secure Browser ポータルでカスタムドメインを追加し、リバースプロキシ（例: CloudFront）を設定
- **認証**: AWS Identity Center または独自 IdP（IdP 開始フローと SP 開始フローの両方をサポート）
- **利用可能リージョン**: 10 リージョン（東京含む）
- **追加料金**: なし
- **参考リンク**: [公式発表](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-workspaces-secure-browser-custom-domains/)

## 今週の傾向

### 🤖 AI/ML の本番環境対応強化
Claude Opus 4.6 と Structured Outputs の組み合わせにより、エージェンティック AI アプリケーションの本番環境での信頼性が大幅に向上しました。Structured Outputs は JSON スキーマ準拠を 100% 保証し、カスタムパーサーやバリデーションロジックを不要にします。また、AgentCore Browser のブラウザプロファイルサポートにより、自動化ブラウザセッションのセットアップ時間が数分から数十秒に短縮されます。

### 📊 Amazon Redshift の自動最適化機能強化
Amazon Redshift の Autonomics 機能が 2 つの重要なアップデートを受けました。追加コンピュートリソース割り当てにより、高負荷時でもユーザーワークロードに影響を与えずに自動最適化を実行でき、マルチクラスター対応により、複数のコンシューマークラスターのクエリパターンを考慮した包括的な最適化が可能になりました。

### 💻 高性能インスタンスと GPU リソース共有
C8id、M8id、R8id インスタンスの一般提供開始に加え、EC2 capacity blocks for ML のマルチアカウント共有が GA となりました。これにより、組織全体での GPU キャパシティの利用率最適化とコスト削減が可能になります。

### 🔌 データ統合と開発者体験の向上
AWS Glue のネイティブ REST API コネクタにより、任意の REST API からのデータ統合がカスタム JAR なしで可能になりました。また、EC2/VPC コンソールのセキュリティグループ関連リソース表示や、AWS Batch の Array Job Status Summary など、開発者の日常業務を効率化する機能も追加されました。

## 注目のユースケース

### エージェンティック AI アプリケーションの本番運用
Claude Opus 4.6 のエージェンティック性能と Structured Outputs を組み合わせることで、信頼性の高いエージェンティックワークフローを構築できます。Opus 4.6 は数十のツールを使用した複雑なタスクを自律的に管理し、Structured Outputs は出力の JSON スキーマ準拠を保証します。AgentCore Browser のブラウザプロファイルと組み合わせることで、認証が必要な Web サイトでの自動化タスクも効率的に実行できます。

### マルチクラスター Redshift 環境の自動最適化
Amazon Redshift の Autonomics マルチクラスター対応により、複数の事業部門がデータを共有する環境での最適化が自動化されます。拒否リスト機能を使用して、特定のエンドポイントやアカウントを最適化の考慮から除外することで、組織間データ共有シナリオにも対応できます。追加コンピュートリソース割り当てにより、ビジネスアワー中でもユーザーワークロードに影響を与えずに最適化を実行できます。

### GPU リソースのマルチアカウント最適化
EC2 capacity blocks for ML のマルチアカウント共有により、組織全体で GPU キャパシティを効率的に活用できます。AWS Resource Access Manager を使用して、購入したキャパシティブロックを複数のアカウントで共有し、異なるワークロードが予約済みキャパシティにアクセスできるようにすることで、GPU リソースの継続的な利用を確保しつつコストを最適化できます。

## 次週の注目ポイント

- Claude Opus 4.6 と Structured Outputs を組み合わせたエージェント開発パターン
- Amazon Redshift Autonomics の追加コンピュート設定ベストプラクティス
- AWS Glue REST API コネクタを使用した SaaS 統合の実装例
- C8id/M8id/R8id インスタンスのベンチマーク結果と移行ガイダンス

## 参考リンク

- [AWS What's New](https://aws.amazon.com/new/)
- [AWS API Changes](https://awsapichanges.com/)
- [AWS Blog](https://aws.amazon.com/blogs/aws/)

## まとめ

今週は、AI/ML 機能の本番環境対応強化、Amazon Redshift の自動最適化機能拡張、開発者体験の向上が目立ちました。特に Claude Opus 4.6 と Structured Outputs の組み合わせは、エージェンティック AI アプリケーションの信頼性を大幅に向上させます。Amazon Redshift の Autonomics 強化により、マルチクラスター環境でのデータウェアハウス運用が効率化され、AWS Glue REST API コネクタはデータ統合の柔軟性を向上させます。これらの新機能を活用することで、より信頼性が高く、運用効率の良いクラウドアーキテクチャを構築できるようになります。
