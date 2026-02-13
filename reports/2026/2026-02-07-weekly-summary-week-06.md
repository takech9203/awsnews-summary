# AWS アップデート週次サマリー - 2026 年第 6 週

**期間**: 2026 年 2 月 2 日 - 2026 年 2 月 7 日
**レポート作成日**: 2026 年 2 月 7 日

📊 [このサマリーのインフォグラフィックを見る](https://takech9203.github.io/awsnews-summary/20260207-weekly-summary-week-06.html)

## 週次ハイライト

今週は **43 件** の主要なアップデートが発表されました。特に AI/ML 機能の強化、新しいインスタンスタイプの一般提供、データ統合機能の拡張が目立ちました。

### 🏆 今週のトップアップデート

1. **Claude Opus 4.6 が Amazon Bedrock で利用可能に** - Anthropic の最新・最高知性モデルが Bedrock に登場
2. **Amazon Bedrock Structured Outputs** - JSON スキーマ準拠の出力を 100% 保証
3. **EC2 C8id/M8id/R8id インスタンス一般提供開始** - Intel Xeon 6 搭載、前世代比最大 43% 性能向上
4. **DynamoDB マルチアカウントグローバルテーブル** - 複数 AWS アカウント間での自動レプリケーション
5. **AWS Glue ネイティブ REST API コネクタ** - 任意の REST API からのデータ統合が簡素化

## カテゴリ別アップデート

### 🤖 AI/ML (6 件)

#### Claude Opus 4.6 - Amazon Bedrock で利用可能に 🔥
- **リリース日**: 2026 年 2 月 5 日
- **概要**: Anthropic の最新モデル Claude Opus 4.6 が Amazon Bedrock で利用可能に
- **主要機能**:
  - 業界トップのエージェンティック性能
  - 200K コンテキストトークン (1M プレビュー)
  - 複雑なコーディングプロジェクトのフルライフサイクル管理
  - 数十のツールを使用した複雑なタスクの自律的管理
- **対象ユーザー**: エージェンティックワークフロー構築者、コード支援ツール開発者
- **詳細レポート**: [2026-02-05-claude-opus-4.6-available-amazon-bedrock.md](./2026-02-05-claude-opus-4.6-available-amazon-bedrock.md)

#### Amazon Bedrock - Structured Outputs 一般提供開始 🔥
- **リリース日**: 2026 年 2 月 4 日
- **概要**: モデル出力がユーザー定義の JSON スキーマに 100% 準拠することを保証
- **主要機能**:
  - JSON Schema 出力フォーマット
  - Strict ツール定義 (strict: true)
  - 効率的なキャッシュ機構 (24 時間)
  - Converse API、InvokeModel API の両方でサポート
- **対象モデル**: Claude、Qwen、DeepSeek、Mistral AI、Google Gemma、NVIDIA Nemotron
- **追加料金**: なし
- **詳細レポート**: [2026-02-04-structured-outputs-available-amazon-bedrock.md](./2026-02-04-structured-outputs-available-amazon-bedrock.md)

#### Amazon Bedrock AgentCore Browser - ブラウザプロファイルサポート
- **リリース日**: 2026 年 2 月 6 日
- **概要**: ブラウザプロファイルでユーザー設定、拡張機能、認証情報を管理可能に
- **主要機能**: 複数タスクでの一貫したセッション維持、Bedrock AgentCore Gateway 統合

#### Amazon SageMaker JumpStart - 新モデル追加
- **リリース日**: 2026 年 2 月 2 日
- **新モデル**:
  - DeepSeek OCR - ドキュメントインテリジェンス
  - MiniMax M2.1 - 多言語コーディング最適化
  - Qwen3-VL-8B-Instruct - 高度なマルチモーダル推論

#### Amazon SageMaker JumpStart - NVIDIA NIMs モデル
- **リリース日**: 2026 年 2 月 2 日
- **新モデル**: ProteinMPNN、MSA Search NIM、Nemotron-3.5B-Instruct、Cosmos Reason

#### Cartesia Sonic 3 TTS モデル - SageMaker JumpStart で利用可能に
- **リリース日**: 2026 年 2 月 4 日
- **概要**: 高品質なテキスト読み上げモデルが SageMaker JumpStart で利用可能に

### ☁️ コンピューティング (10 件)

#### Amazon EC2 C8id/M8id/R8id インスタンス一般提供開始 🔥
- **リリース日**: 2026 年 2 月 4 日
- **概要**: カスタム Intel Xeon 6 プロセッサ搭載の新インスタンスファミリー
- **主要機能**:
  - 前世代比最大 43% の性能向上
  - 3.3 倍のメモリ帯域幅
  - 最大 384 vCPU、3TiB メモリ、22.8TB NVMe SSD
  - Instance Bandwidth Configuration で帯域幅の柔軟な割り当て
- **利用可能リージョン**: us-east-1、us-east-2、us-west-2、eu-central-1 (R8id のみ)
- **詳細レポート**: [2026-02-04-amazon-ec2-c8id-m8id-r8id-instances.md](./2026-02-04-amazon-ec2-c8id-m8id-r8id-instances.md)

#### Amazon EC2 G6e インスタンス - UAE リージョンで利用可能に
- **リリース日**: 2026 年 2 月 5 日
- **利用可能リージョン**: Middle East (UAE)

#### Amazon EC2 G7e インスタンス - US West (Oregon) で利用可能に
- **リリース日**: 2026 年 2 月 4 日
- **利用可能リージョン**: us-west-2

#### Amazon EC2 capacity blocks for ML - マルチアカウント共有
- **リリース日**: 2026 年 2 月 5 日
- **概要**: ML 用キャパシティブロックを複数アカウントで共有可能に

#### Amazon WorkSpaces - Graphics G6/Gr6/G6f バンドル
- **リリース日**: 2026 年 2 月 5 日
- **概要**: 最新 GPU バンドルで高性能グラフィックスワークロードに対応

#### Amazon Lightsail - メモリ最適化インスタンスバンドル
- **リリース日**: 2026 年 2 月 2 日
- **概要**: 最大 512 GB メモリのメモリ最適化インスタンス

#### Amazon ECS - NLB での Linear/Canary デプロイメント
- **リリース日**: 2026 年 2 月 4 日
- **概要**: Network Load Balancer で段階的デプロイメントをサポート

#### Amazon ECS Managed Instances - AWS European Sovereign Cloud で利用可能に
- **リリース日**: 2026 年 2 月 6 日

#### AWS Batch - Amazon EKS の Unmanaged Compute Environments
- **リリース日**: 2026 年 2 月 4 日
- **概要**: EKS で Unmanaged Compute Environments をサポート

#### AWS Batch - Array Job Status Summary (ListJobs API)
- **リリース日**: 2026 年 2 月 3 日
- **概要**: アレイジョブのステータスサマリーを ListJobs API で取得可能に

### 🗄️ データベース (5 件)

#### Amazon DynamoDB - マルチアカウントグローバルテーブル 🔥
- **リリース日**: 2026 年 2 月 3 日
- **概要**: グローバルテーブルが複数 AWS アカウント間でのレプリケーションに対応
- **主要機能**:
  - AWS アカウントとリージョンをまたいだ自動レプリケーション
  - アカウントレベルでの障害分離
  - Multi-Region Eventual Consistency (MREC) モード
- **詳細レポート**: [2026-02-03-dynamodb-gt-multi-account.md](./2026-02-03-dynamodb-gt-multi-account.md)

#### Amazon Aurora DSQL - NUMERIC データタイプのインデックスサポート
- **リリース日**: 2026 年 2 月 3 日
- **概要**: NUMERIC 列のプライマリキーおよびセカンダリインデックス作成が可能に

#### Amazon RDS - コンソールの接続エクスペリエンス強化
- **リリース日**: 2026 年 2 月 3 日
- **概要**: 接続に必要な情報を一箇所に集約、コードスニペット自動生成

#### Amazon RDS for MySQL - 新マイナーバージョン 8.0.45 および 8.4.8
- **リリース日**: 2026 年 2 月 2 日

#### Oracle Database@AWS - カナダとシドニーリージョンで利用可能に
- **リリース日**: 2026 年 2 月 3 日

### 📊 データ統合・分析 (4 件)

#### AWS Glue - ネイティブ REST API コネクタ 🔥
- **リリース日**: 2026 年 2 月 5 日
- **概要**: 任意の REST API からデータを取得し、Glue ETL ジョブに統合可能に
- **主要機能**:
  - OAuth 2.0、API キー、Basic 認証など柔軟な認証サポート
  - カスタム JAR ファイル不要
  - ページネーション自動処理
- **追加料金**: なし
- **詳細レポート**: [2026-02-05-aws-glue-rest-api-connector.md](./2026-02-05-aws-glue-rest-api-connector.md)

#### Apache Spark lineage - SageMaker Unified Studio (IDC based domains)
- **リリース日**: 2026 年 2 月 4 日
- **概要**: IDC ベースドメインで Apache Spark のリネージ追跡が可能に

#### Amazon Redshift - マルチクラスター環境の Autonomics
- **リリース日**: 2026 年 2 月 4 日
- **概要**: マルチクラスター環境での自動最適化機能

#### AWS Lake Formation - ニュージーランドリージョンで利用可能に
- **リリース日**: 2026 年 2 月 3 日

### 🔐 セキュリティ・ID・コンプライアンス (5 件)

#### AWS IAM Identity Center - マルチリージョン対応 🔥
- **リリース日**: 2026 年 2 月 3 日
- **概要**: 複数 AWS リージョンでのアカウントアクセスとアプリケーション利用が可能に
- **主要機能**:
  - ID、権限セット、メタデータの自動レプリケーション
  - プライマリリージョン中断時も追加リージョンでアクセス継続
  - データレジデンシー要件への対応
- **要件**: 外部 IdP 接続、マルチリージョン CMK
- **詳細レポート**: [2026-02-03-aws-iam-identity-center-multi-region-aws-account-access-and-application-deployment.md](./2026-02-03-aws-iam-identity-center-multi-region-aws-account-access-and-application-deployment.md)

#### AWS Builder ID - Sign in with Apple サポート
- **リリース日**: 2026 年 2 月 5 日
- **概要**: Apple ID での AWS Builder ID サインインに対応

#### AWS STS - ID プロバイダー固有のクレーム検証サポート
- **リリース日**: 2026 年 2 月 2 日
- **対象**: Google、GitHub、CircleCI、Oracle Cloud Infrastructure

#### AWS Multi-party Approval - ワンタイムパスワード検証
- **リリース日**: 2026 年 2 月 2 日
- **概要**: 投票時に OTP による本人確認を追加

#### Amazon CloudFront - オリジン mTLS サポート
- **リリース日**: 2026 年 2 月 2 日
- **概要**: オリジン認証に証明書ベースの mTLS を使用可能に
- **詳細レポート**: [2026-02-02-amazon-cloudfront-mutual-tls-for-origins.md](./2026-02-02-amazon-cloudfront-mutual-tls-for-origins.md)

### 🔧 管理・ガバナンス (4 件)

#### AWS Config - 30 の新リソースタイプをサポート
- **リリース日**: 2026 年 2 月 6 日
- **概要**: AWS Config がサポートするリソースタイプが拡大

#### AWS Network Firewall - 価格引き下げ
- **リリース日**: 2026 年 2 月 6 日
- **概要**: Network Firewall の料金が引き下げ

#### AWS Management Console - ナビゲーションバーにアカウント名表示
- **リリース日**: 2026 年 2 月 3 日
- **概要**: アカウント識別を容易にするアカウント名表示

#### Amazon EC2/VPC - セキュリティグループの関連リソース表示
- **リリース日**: 2026 年 2 月 4 日
- **概要**: セキュリティグループに関連するリソースをコンソールで表示

### 📞 コンタクトセンター (3 件)

#### Amazon Connect - エージェント評価の異議申し立てワークフロー
- **リリース日**: 2026 年 2 月 3 日
- **概要**: エージェントが評価に異議を申し立て可能に

#### Amazon Connect - 音声インタラクションのテスト・シミュレーション API
- **リリース日**: 2026 年 2 月 2 日
- **概要**: CI/CD パイプラインへの統合が可能に

#### Amazon Connect Cases - CSV アップロードによる関連フィールドオプションマッピング
- **リリース日**: 2026 年 2 月 6 日

### 🏥 ヘルスケア (1 件)

#### AWS HealthImaging - JPEG XL サポート
- **リリース日**: 2026 年 2 月 2 日
- **概要**: JPEG XL ロッシー圧縮医療画像の保存と取得をサポート

### 💼 エンドユーザーコンピューティング (2 件)

#### Amazon WorkSpaces Secure Browser - カスタムドメインサポート
- **リリース日**: 2026 年 2 月 6 日
- **概要**: 組織のブランディングに合わせたカスタムドメイン設定が可能に

#### Amazon WorkSpaces - Graphics G6/Gr6/G6f バンドル
- **リリース日**: 2026 年 2 月 5 日

### その他 (3 件)

#### Amazon Quick Suite - 曖昧な地理情報の解決
- **リリース日**: 2026 年 2 月 3 日

#### AWS Marketplace - EMEA からのプロフェッショナルサービスの現地化請求
- **リリース日**: 2026 年 2 月 3 日

#### AWS EKS - GovCloud (US) での IAM 権限簡素化
- **リリース日**: 2026 年 2 月 4 日

## 今週の傾向

### 🤖 AI/ML 機能の成熟化
Claude Opus 4.6 の登場と Structured Outputs の GA は、Amazon Bedrock を使用した本番環境アプリケーションの信頼性と予測可能性を大幅に向上させます。特に Structured Outputs は、カスタムパーサーやバリデーションロジックを不要にし、開発効率を高めます。

### 💻 第 8 世代 Intel インスタンスの拡充
C8id、M8id、R8id インスタンスの一般提供開始により、I/O 集約型ワークロードやインメモリデータベースの性能が大幅に向上します。Instance Bandwidth Configuration による柔軟な帯域幅割り当ても注目です。

### 🔄 マルチアカウント・マルチリージョン機能の強化
DynamoDB マルチアカウントグローバルテーブルと IAM Identity Center マルチリージョン対応は、エンタープライズグレードのグローバル展開を容易にする重要なアップデートです。

### 🔌 データ統合の簡素化
AWS Glue のネイティブ REST API コネクタは、SaaS やマイクロサービスからのデータ統合を大幅に簡素化し、カスタム開発の負担を軽減します。

## 注目のユースケース

### エージェンティック AI アプリケーションの構築
Claude Opus 4.6 と Structured Outputs を組み合わせることで、信頼性の高いエージェンティックワークフローを構築できます。Opus 4.6 のエージェンティック性能と Structured Outputs の出力保証により、本番環境での複雑な自動化が実現します。

### 高性能データベースワークロードの最適化
C8id、M8id、R8id インスタンスと DynamoDB マルチアカウントグローバルテーブルを活用することで、I/O 集約型のデータベースワークロードとグローバルなデータレプリケーションを最適化できます。

### ユニバーサルデータ統合パイプライン
Glue REST API コネクタを使用して、任意の SaaS やマイクロサービスからデータを取得し、データレイクやデータウェアハウスに統合する包括的なデータパイプラインを構築できます。

## 次週の注目ポイント

- Claude Opus 4.6 の実運用事例とベストプラクティス
- Structured Outputs を活用したエージェントアプリケーションのパターン
- C8id/M8id/R8id インスタンスのベンチマーク結果
- Glue REST API コネクタを使用した SaaS 統合事例

## 参考リンク

- [AWS What's New](https://aws.amazon.com/new/)
- [AWS API Changes](https://awsapichanges.com/)
- [AWS Blog](https://aws.amazon.com/blogs/aws/)

## まとめ

今週は、AI/ML 機能の成熟化、高性能インスタンスの一般提供、データ統合機能の拡張が目立ちました。特に Claude Opus 4.6 と Structured Outputs の組み合わせは、本番環境での AI アプリケーション開発を大きく前進させます。また、C8id/M8id/R8id インスタンスと AWS Glue REST API コネクタは、それぞれコンピューティング性能とデータ統合の柔軟性を大幅に向上させます。これらの新機能を活用することで、より高性能で柔軟なクラウドアーキテクチャを構築できるようになります。
