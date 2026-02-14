# AWS アップデート週次サマリー - 2026 年第 5 週

**期間**: 2026 年 1 月 28 日 - 2026 年 2 月 4 日
**レポート作成日**: 2026 年 2 月 4 日

📊 [このサマリーのインフォグラフィックを見る](https://takech9203.github.io/aws-news-summary/20260204-weekly-summary-week-05.html)

## 週次ハイライト

今週は **37 件** の主要なアップデートが発表されました。特にマルチアカウント戦略、セキュリティ強化、そして AI/ML 関連の機能拡張が目立ちました。

### 🏆 今週のトップアップデート

1. **DynamoDB マルチアカウントグローバルテーブル** - 複数 AWS アカウント間での自動レプリケーションに対応
2. **IAM Identity Center マルチリージョン対応** - 複数リージョンでのアカウントアクセスとアプリケーション利用が可能に
3. **EventBridge ペイロードサイズ拡大** - 256 KB から 1 MB へ増加
4. **S3 暗号化タイプの変更** - データ移動なしで暗号化タイプを変更可能に
5. **CloudFront オリジン mTLS サポート** - オリジン認証が証明書ベースに

## カテゴリ別アップデート

### 🗄️ データベース (7 件)

#### Amazon DynamoDB - マルチアカウントグローバルテーブル 🔥
- **リリース日**: 2026 年 2 月 3 日
- **概要**: DynamoDB グローバルテーブルが複数の AWS アカウント間でのレプリケーションに対応
- **主要機能**:
  - AWS アカウントとリージョンをまたいだ自動レプリケーション
  - アカウントレベルでの障害分離
  - 各アカウントで異なるセキュリティとガバナンス制御
  - Multi-Region Eventual Consistency (MREC) モード
- **対象ユーザー**: マルチアカウント戦略を採用している組織、災害復旧要件のある組織
- **利用可能リージョン**: すべての AWS 商用リージョン
- **詳細レポート**: [2026-02-03-dynamodb-gt-multi-account.md](./2026-02-03-dynamodb-gt-multi-account.md)

#### Amazon RDS - コンソールの接続エクスペリエンス強化
- **リリース日**: 2026 年 2 月 3 日
- **概要**: データベース接続に必要な情報を一箇所に集約し、コードスニペットを自動生成
- **主要機能**:
  - Java、Python、Node.js などのコードスニペット自動生成
  - IAM 認証設定に基づいたコード調整
  - CloudShell 統合によるコンソール内接続
- **対象エンジン**: Aurora PostgreSQL/MySQL、RDS PostgreSQL/MySQL/MariaDB

#### Amazon Aurora DSQL - NUMERIC データタイプのインデックスサポート
- **リリース日**: 2026 年 2 月 3 日
- **概要**: NUMERIC 列のプライマリキーおよびセカンダリインデックス作成が可能に
- **ユースケース**: 通貨、測定値、統計データなど高精度値を扱うワークロード

#### Amazon RDS for Oracle - クロスリージョンレプリカで追加ストレージボリューム対応
- **リリース日**: 2026 年 1 月 30 日
- **概要**: 最大 256 TiB のストレージを持つクロスリージョンレプリカの作成が可能に

#### Amazon RDS - IPv6 for VPC エンドポイント対応
- **リリース日**: 2026 年 1 月 30 日
- **概要**: RDS サービス API の VPC エンドポイントで IPv6 をサポート
- **利用可能リージョン**: すべての商用リージョンおよび GovCloud

#### Amazon Keyspaces - テーブルプリウォーミング
- **リリース日**: 2026 年 1 月 29 日
- **概要**: WarmThroughput を使用した事前ウォーミングにより、トラフィック急増に対応
- **対象**: プロビジョニングとオンデマンドの両方のキャパシティモード

#### Oracle Database@AWS - カナダとシドニーリージョンで利用可能に
- **リリース日**: 2026 年 2 月 3 日
- **利用可能リージョン**: CA-Central-1、AP-Southeast-2

### 🔐 セキュリティ・ID・コンプライアンス (7 件)

#### AWS IAM Identity Center - マルチリージョン対応 🔥
- **リリース日**: 2026 年 2 月 3 日
- **概要**: プライマリリージョンから追加リージョンへの自動レプリケーション
- **主要機能**:
  - ID、権限セット、メタデータの自動レプリケーション
  - プライマリリージョン中断時も追加リージョンでアクセス継続
  - AWS アプリケーションの複数リージョンデプロイ
- **要件**: 外部 IdP (Okta など) 接続、マルチリージョン CMK
- **利用可能リージョン**: デフォルト有効の 17 商用リージョン

#### AWS Management Console - ナビゲーションバーにアカウント名表示
- **リリース日**: 2026 年 2 月 3 日
- **概要**: アカウント ID だけでなくアカウント名をナビゲーションバーに表示
- **メリット**: マルチアカウント環境での視認性向上

#### AWS STS - ID プロバイダー固有のクレーム検証サポート
- **リリース日**: 2026 年 2 月 2 日
- **対象プロバイダー**: Google、GitHub、CircleCI、Oracle Cloud Infrastructure
- **主要機能**: IAM ロール信頼ポリシーでカスタムクレームを条件キーとして参照可能

#### AWS Multi-party Approval - ワンタイムパスワード検証
- **リリース日**: 2026 年 2 月 2 日
- **概要**: 投票時に OTP による本人確認を追加
- **セキュリティ**: IAM Identity Center 管理者によるなりすまし防止

#### Amazon CloudFront - オリジン mTLS サポート 🔥
- **リリース日**: 2026 年 2 月 2 日
- **概要**: オリジン認証に証明書ベースの mTLS を使用可能に
- **主要機能**:
  - CloudFront のみがオリジンにアクセスできることを暗号学的に検証
  - カスタム認証ソリューション (共有シークレット、IP 許可リスト) が不要に
  - AWS Private CA または外部 CA の証明書を使用
- **対象オリジン**: ALB、API Gateway、オンプレミス、カスタムオリジン
- **追加料金**: なし

#### Amazon Cognito - インバウンドフェデレーション Lambda トリガー
- **リリース日**: 2026 年 1 月 29 日
- **概要**: 外部 SAML/OIDC プロバイダーの属性をユーザープール保存前に変換
- **ユースケース**: 2,048 文字を超える大きなグループ属性の処理

#### AWS Network Firewall - GovCloud でのフレキシブルコスト配分
- **リリース日**: 2026 年 2 月 2 日
- **概要**: Transit Gateway ネイティブアタッチメントを通じたコスト配分
- **利用可能リージョン**: GovCloud (US-East)、GovCloud (US-West)

### 🤖 AI/ML (5 件)

#### Amazon SageMaker JumpStart - 新モデル追加
- **リリース日**: 2026 年 2 月 2 日
- **新モデル**:
  - DeepSeek OCR - ドキュメントインテリジェンス
  - MiniMax M2.1 - 多言語コーディング最適化
  - Qwen3-VL-8B-Instruct - 高度なマルチモーダル推論

#### Amazon SageMaker JumpStart - NVIDIA NIMs モデル
- **リリース日**: 2026 年 2 月 2 日
- **新モデル**:
  - ProteinMPNN - タンパク質配列最適化
  - MSA Search NIM - 複数配列アライメント
  - Nemotron-3.5B-Instruct - 推論と tool calling
  - Cosmos Reason - 物理 AI とロボティクス

#### Amazon Bedrock - サーバーサイドカスタムツール (Responses API)
- **リリース日**: 2026 年 1 月 29 日
- **概要**: Responses API でサーバーサイドツールをサポート
- **主要機能**:
  - AWS Lambda 関数または AWS 提供ツールを使用
  - Web 検索、コード実行、データベース更新などを実行
- **対象モデル**: OpenAI GPT OSS 20B/120B

#### Amazon CloudWatch Application Signals - Kiro Powers 統合
- **リリース日**: 2026 年 1 月 30 日
- **概要**: Kiro IDE での AI エージェント支援による調査ワークフロー

#### AWS HealthImaging - JPEG XL サポート
- **リリース日**: 2026 年 2 月 2 日
- **概要**: JPEG XL ロッシー圧縮医療画像の保存と取得をサポート
- **ユースケース**: デジタルパソロジーのホールスライドイメージング

### 📊 分析 (3 件)

#### AWS Lake Formation - ニュージーランドリージョンで利用可能に
- **リリース日**: 2026 年 2 月 3 日
- **利用可能リージョン**: Asia Pacific (New Zealand)

#### Amazon Quick Suite - 曖昧な地理情報の解決
- **リリース日**: 2026 年 2 月 3 日
- **概要**: 複数の地域に存在する同名の場所を正確にマッピング
- **解決方法**: 地理空間フィールドの追加、データベース検索、緯度経度指定

### 🌐 ネットワーキング・コンテンツ配信 (3 件)

#### Amazon EventBridge - ペイロードサイズ拡大 🔥
- **リリース日**: 2026 年 1 月 29 日
- **概要**: イベントペイロードサイズを 256 KB から 1 MB に拡大
- **主要機能**:
  - 大規模言語モデルのプロンプト、テレメトリ信号、複雑な JSON をサポート
  - データ分割や外部ストレージが不要に
- **ユースケース**: リッチなコンテキストデータ、ML 出力、複雑なデータ構造
- **利用可能リージョン**: 一部リージョンを除くすべての商用リージョン

#### CloudFront オリジン mTLS サポート (セキュリティセクションを参照)

### ☁️ コンピューティング (6 件)

#### Amazon EC2 R8a インスタンス - ヨーロッパリージョン拡大
- **リリース日**: 2026 年 1 月 30 日
- **利用可能リージョン**: Europe (Spain)、Europe (Frankfurt)
- **性能**: R7a と比較して最大 30% 高性能、最大 19% 優れた価格性能比

#### Amazon Lightsail - メモリ最適化インスタンスバンドル
- **リリース日**: 2026 年 2 月 2 日
- **概要**: 最大 512 GB メモリのメモリ最適化インスタンスを提供
- **サイズ**: 7 サイズ、Linux/Windows OS およびアプリケーションブループリント
- **ユースケース**: インメモリデータベース、リアルタイムビッグデータ分析、HPC

#### Amazon ECS - コンテナヘルスステータスメトリクス
- **リリース日**: 2026 年 1 月 30 日
- **概要**: CloudWatch Container Insights に UnHealthyContainerHealthStatus メトリクスを追加
- **主要機能**: クラスター、サービス、タスク、コンテナレベルでの健全性モニタリング

#### AWS Lambda - Kafka ESM の拡張オブザーバビリティ
- **リリース日**: 2026 年 1 月 30 日
- **概要**: Kafka イベントソースマッピングの CloudWatch ログとメトリクス
- **対象**: Amazon MSK および self-managed Apache Kafka

#### Amazon EKS - Kubernetes 1.35 サポート
- **リリース日**: 2026 年 1 月 28 日
- **主要機能**:
  - In-Place Pod Resource Updates - Pod 再起動なしで CPU/メモリ調整
  - PreferSameNode Traffic Distribution - ローカルエンドポイント優先
  - Node Topology Labels via Downward API - リージョン/ゾーン情報へのアクセス
  - Image Volumes - OCI コンテナイメージを使用したデータアーティファクト配信

### 💾 ストレージ (2 件)

#### Amazon S3 - サーバーサイド暗号化タイプの変更 🔥
- **リリース日**: 2026 年 1 月 29 日
- **概要**: データ移動なしで暗号化タイプを変更可能に
- **主要機能**:
  - UpdateObjectEncryption API で暗号化キーをアトミックに変更
  - オブジェクトサイズやストレージクラスに関係なく変更可能
  - S3 Batch Operations で大規模な変更をサポート
- **ユースケース**:
  - SSE-S3 から SSE-KMS への移行
  - カスタムキーローテーション標準への準拠
  - S3 Bucket Keys の有効化
- **利用可能リージョン**: すべての AWS リージョン

### 🔧 開発者ツール (2 件)

#### AWS MCP Server - デプロイメント Agent SOPs (プレビュー)
- **リリース日**: 2026 年 1 月 29 日
- **概要**: 自然言語プロンプトで Web アプリケーションを AWS にデプロイ
- **主要機能**:
  - CDK インフラ生成と CloudFormation デプロイ
  - CodePipeline での CI/CD パイプライン作成
  - React、Vue.js、Angular、Next.js をサポート
- **対応 IDE**: Kiro、Kiro CLI、Cursor、Claude Code
- **利用可能リージョン**: US East (N. Virginia)

#### New Partner Revenue Measurement
- **リリース日**: 2026 年 1 月 30 日
- **概要**: AWS Partner が自社ソリューションの AWS サービス消費への影響を可視化
- **利用可能リージョン**: すべての商用リージョン

### 📞 コンタクトセンター (4 件)

#### Amazon Connect - エージェント評価の異議申し立てワークフロー
- **リリース日**: 2026 年 2 月 3 日
- **概要**: エージェントが評価に異議を申し立て、マネージャーが解決するワークフロー

#### Amazon Connect - 待ち時間推定の改善
- **リリース日**: 2026 年 1 月 30 日
- **概要**: キューとエンキューされた連絡先の待ち時間推定メトリクスを改善

#### Amazon Connect - 音声インタラクションのテストと シミュレーション API
- **リリース日**: 2026 年 2 月 2 日
- **概要**: コンタクトセンター体験をテストするための API
- **主要機能**: CI/CD パイプラインへの統合、複数テストの同時実行、自動リグレッションテスト

### 🎮 ゲーム開発 (除外)

Amazon GameLift 関連のアップデートは除外サービスのため、本サマリーには含まれていません。

## API 変更サマリー

今週の主要な API 変更:

| サービス | 変更内容 |
|---------|---------|
| [Amazon DynamoDB](https://awsapichanges.com/archive/changes/104e8e-dynamodb.html) | 9 updated methods - マルチアカウントグローバルテーブルサポート |
| [AWS Single Sign-On Admin](https://awsapichanges.com/archive/changes/104e8e-sso.html) | 4 new + 2 updated methods - マルチリージョンレプリケーション API |
| [Amazon CloudFront](https://awsapichanges.com/archive/changes/12d57f-cloudfront.html) | 14 updated methods - オリジン mTLS サポート |
| [AWS Multi-party Approval](https://awsapichanges.com/archive/changes/12d57f-mpa.html) | 4 updated methods - MFA for voting operations |
| [Amazon Connect Service](https://awsapichanges.com/archive/changes/26a688-connect.html) | 2 updated methods - 待ち時間推定サポート |
| [Amazon EC2](https://awsapichanges.com/archive/changes/0b51ae-ec2.html) | 25 updated methods - G7e インスタンスサポート |

## 今週の傾向

### 🔄 マルチアカウント・マルチリージョン戦略の強化
DynamoDB と IAM Identity Center の両方でマルチアカウント・マルチリージョン機能が強化され、エンタープライズグレードのグローバル展開がより容易になりました。

### 🔐 セキュリティとガバナンスの強化
CloudFront の mTLS、STS のクレーム検証、Multi-party Approval の OTP など、セキュリティ機能が複数強化されました。

### 🤖 AI/ML モデルの拡充
SageMaker JumpStart で複数の新しいモデルが追加され、特にバイオサイエンスとロボティクス向けの NVIDIA NIMs が注目されます。

### 📦 ペイロードとストレージの柔軟性向上
EventBridge のペイロードサイズ拡大と S3 の暗号化タイプ変更により、データ処理の柔軟性が向上しました。

## 注目のユースケース

### エンタープライズ向けマルチアカウント戦略
DynamoDB マルチアカウントグローバルテーブルと IAM Identity Center マルチリージョンを組み合わせることで、事業部門ごとの完全な分離とグローバルなデータ共有を両立できます。

### ゼロトラストアーキテクチャの強化
CloudFront オリジン mTLS と STS クレーム検証により、証明書ベースの認証ときめ細かいアクセス制御が可能になりました。

### イベント駆動アーキテクチャの拡張
EventBridge の 1 MB ペイロードサポートにより、LLM プロンプトや ML 出力などのリッチなデータをイベントに含めることができます。

### コンプライアンス対応の簡素化
S3 の暗号化タイプ変更機能により、SSE-S3 から SSE-KMS への移行が容易になり、規制要件への対応がスムーズになりました。

## 次週の注目ポイント

- DynamoDB マルチアカウントグローバルテーブルの実運用事例
- IAM Identity Center マルチリージョンのフェイルオーバーパターン
- EventBridge 1 MB ペイロードを活用した新しいアーキテクチャパターン
- S3 暗号化タイプ変更の大規模運用事例

## 参考リンク

- [AWS What's New](https://aws.amazon.com/new/)
- [AWS API Changes](https://awsapichanges.com/)
- [AWS Blog](https://aws.amazon.com/blogs/aws/)

## まとめ

今週は、マルチアカウント・マルチリージョン戦略の強化、セキュリティ機能の拡充、そして AI/ML モデルの追加が目立ちました。特に DynamoDB のマルチアカウントグローバルテーブルと IAM Identity Center のマルチリージョン対応は、エンタープライズ環境でのグローバル展開を大幅に容易にする画期的な機能です。また、EventBridge のペイロードサイズ拡大と S3 の暗号化タイプ変更は、運用の柔軟性を大きく向上させます。これらの新機能を活用することで、より堅牢で柔軟なクラウドアーキテクチャを構築できるようになります。
