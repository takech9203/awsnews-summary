# AWS & Kiro 週次サマリー - 2026 年 第 6 週

**期間**: 2026 年 2 月 2 日 〜 2026 年 2 月 8 日

📊 [このサマリーのインフォグラフィックを見る](https://takech9203.github.io/awsnews-summary/20260208-weekly-summary-week-06.html)

## 今週のハイライト

今週は **AI/ML**、**コンピュート**、**データベース**、**セキュリティ** 分野で重要なアップデートが多数発表されました。特に Amazon Bedrock の Structured Outputs と DynamoDB Global Tables のマルチアカウントレプリケーションは、エンタープライズワークロードに大きな影響を与える機能です。また、Kiro 0.9 のリリースにより、開発者向け AI ツールの機能が大幅に強化されました。

## カテゴリ別アップデート一覧

### 🤖 AI/ML

| 日付 | サービス | アップデート | 重要度 |
|------|----------|-------------|--------|
| 2/4 | Amazon Bedrock | [Structured Outputs (構造化出力) GA](./2026-02-04-structured-outputs-available-amazon-bedrock.md) | ⭐⭐⭐ |
| 2/6 | Amazon Bedrock AgentCore | [Browser Profiles サポート](./2026-02-06-amazon-bedrock-agentcore-browser-profiles.md) | ⭐⭐ |
| 2/4 | Amazon Redshift | Autonomics for Multi-cluster | ⭐⭐ |
| 2/4 | Amazon SageMaker | Apache Spark Lineage for IDC Domains | ⭐ |
| 2/4 | Amazon SageMaker JumpStart | Cartesia Sonic 3 text-to-speech モデル | ⭐ |
| 2/2 | Amazon SageMaker JumpStart | DeepSeek OCR, MiniMax M2.1, Qwen3-VL-8B-Instruct | ⭐ |
| 2/2 | Amazon SageMaker JumpStart | NVIDIA NIMs for Drug Discovery & Robotics | ⭐ |

### 💻 コンピュート

| 日付 | サービス | アップデート | 重要度 |
|------|----------|-------------|--------|
| 2/4 | Amazon EC2 | [C8id, M8id, R8id インスタンス GA](./2026-02-04-amazon-ec2-c8id-m8id-r8id-instances.md) | ⭐⭐⭐ |
| 2/4 | Amazon ECS | [NLB 対応 Linear/Canary デプロイメント](./2026-02-04-amazon-ecs-nlb-linear-canary-deployments.md) | ⭐⭐⭐ |
| 2/4 | AWS Batch | EKS 向け Unmanaged Compute Environments | ⭐⭐ |
| 2/5 | Amazon WorkSpaces | Graphics G6, Gr6, G6f バンドル | ⭐⭐ |
| 2/5 | Amazon EC2 | G6e インスタンス UAE リージョン | ⭐ |
| 2/5 | Amazon EC2 | I7ie インスタンス Canada Central | ⭐ |
| 2/5 | Amazon EC2 | Capacity Blocks for ML マルチアカウント共有 | ⭐⭐ |
| 2/4 | Amazon EC2 | G7e インスタンス US West (Oregon) | ⭐ |
| 2/2 | Amazon Lightsail | Memory-optimized インスタンスバンドル | ⭐ |

### 🗄️ データベース

| 日付 | サービス | アップデート | 重要度 |
|------|----------|-------------|--------|
| 2/3 | Amazon DynamoDB | [Global Tables マルチアカウントレプリケーション](./2026-02-03-dynamodb-gt-multi-account.md) | ⭐⭐⭐ |
| 2/3 | Amazon Aurora DSQL | NUMERIC データ型のインデックスサポート | ⭐⭐ |
| 2/3 | Amazon RDS | 強化されたコンソール接続体験 | ⭐ |
| 2/2 | Amazon RDS for MySQL | 新マイナーバージョン 8.0.45, 8.4.8 | ⭐ |
| 2/3 | AWS Lake Formation | Asia Pacific (New Zealand) リージョン | ⭐ |
| 2/3 | Oracle Database@AWS | Canada Central, Sydney リージョン | ⭐ |

### 🔒 セキュリティ & アイデンティティ

| 日付 | サービス | アップデート | 重要度 |
|------|----------|-------------|--------|
| 2/3 | AWS IAM Identity Center | [マルチリージョンサポート](./2026-02-03-aws-iam-identity-center-multi-region-aws-account-access-and-application-deployment.md) | ⭐⭐⭐ |
| 2/2 | AWS Multi-party Approval | OTP 検証必須化 | ⭐⭐ |
| 2/2 | AWS STS | IdP クレーム検証 (Google, GitHub, CircleCI, OCI) | ⭐⭐ |
| 2/2 | Amazon CloudFront | オリジン向け mTLS サポート | ⭐⭐ |

### 🔧 開発者ツール & 統合

| 日付 | サービス | アップデート | 重要度 |
|------|----------|-------------|--------|
| 2/5 | AWS Glue | [ネイティブ REST API コネクタ](./2026-02-05-aws-glue-rest-api-connector.md) | ⭐⭐⭐ |
| 2/6 | AWS Config | 30 の新リソースタイプサポート | ⭐⭐ |
| 2/5 | AWS Builder ID | Sign in with Apple サポート | ⭐ |
| 2/3 | AWS Batch | Array Job Status Summary in ListJobs API | ⭐ |
| 2/3 | AWS Management Console | ナビゲーションバーにアカウント名表示 | ⭐ |

### 📞 コンタクトセンター & ビジネスアプリ

| 日付 | サービス | アップデート | 重要度 |
|------|----------|-------------|--------|
| 2/6 | Amazon WorkSpaces Secure Browser | カスタムドメインサポート | ⭐⭐ |
| 2/6 | Amazon Connect Cases | CSV による関連フィールドオプションマッピング | ⭐ |
| 2/3 | Amazon Connect | エージェント評価の異議申立てワークフロー | ⭐ |
| 2/2 | Amazon Connect | 音声インタラクションのテスト・シミュレーション API | ⭐⭐ |
| 2/3 | Amazon Quick Suite | 曖昧な地図位置の解決機能 | ⭐ |

### 🛠️ その他

| 日付 | サービス | アップデート | 重要度 |
|------|----------|-------------|--------|
| 2/6 | Amazon ECS Managed Instances | AWS European Sovereign Cloud 対応 | ⭐ |
| 2/6 | AWS Network Firewall | 料金削減発表 | ⭐⭐ |
| 2/4 | Amazon EC2/VPC | Security Group の関連リソース表示 | ⭐ |
| 2/2 | AWS HealthImaging | JPEG XL サポート | ⭐ |

### 🔷 Kiro

| 日付 | サービス | アップデート | 重要度 |
|------|----------|-------------|--------|
| 2/5 | Kiro | [0.9: カスタムサブエージェント・スキル・エンタープライズ制御](./2026-02-05-kiro-0-9-custom-subagents-skills-and-enterprise-controls.md) | ⭐⭐⭐ |
| 2/5 | Kiro | Claude Opus 4.6 サポート | ⭐⭐⭐ |
| 2/4 | Kiro | ACP 統合、Help Agent | ⭐⭐ |

## 注目すべき技術トレンド

### 1. AI エージェントの進化
Amazon Bedrock AgentCore Browser のブラウザプロファイル機能と Kiro のカスタムサブエージェントにより、AI エージェントの実用性が大幅に向上しています。認証状態の永続化やコンテキスト分離により、複雑な自動化タスクがより効率的に実行できるようになりました。

### 2. マルチアカウント・マルチリージョン戦略の強化
DynamoDB Global Tables のマルチアカウントレプリケーションと IAM Identity Center のマルチリージョンサポートにより、エンタープライズのマルチアカウント戦略がさらに強化されました。障害分離とガバナンスの両立が容易になっています。

### 3. 構造化出力による AI アプリケーションの信頼性向上
Amazon Bedrock の Structured Outputs により、生成 AI アプリケーションの本番運用における信頼性が向上しました。JSON スキーマによる応答形式の保証は、AI エージェントやデータ抽出パイプラインの構築を容易にします。

## 今週のおすすめアクション

1. **Amazon Bedrock ユーザー**: Structured Outputs を評価し、既存のアプリケーションでの JSON 検証ロジックの簡素化を検討
2. **マルチアカウント環境の運用者**: DynamoDB Global Tables のマルチアカウントレプリケーションで DR 戦略を強化
3. **NLB を使用する ECS ユーザー**: Linear/Canary デプロイメントを導入し、デプロイリスクを軽減
4. **Kiro ユーザー**: 0.9 にアップデートし、カスタムサブエージェントとスキルを活用

## 参考リンク

- [AWS What's New](https://aws.amazon.com/new/)
- [AWS Blog](https://aws.amazon.com/blogs/aws/)
- [Kiro Blog](https://kiro.dev/blog/)
- [Kiro Changelog](https://kiro.dev/changelog/)
