# AWS 週次アップデートサマリー (2026年第3週)

**対象期間**: 2026 年 1 月 12 日 (月) 〜 2026 年 1 月 19 日 (日)

## エグゼクティブサマリー

2026 年第 3 週は、コンピューティング、ネットワーク、データベース、AI/ML の各分野で重要なアップデートがありました。特に、Amazon EC2 の新インスタンスタイプ、AWS Outposts のネットワークセグメンテーション機能、Amazon Lambda のクロスアカウント機能が注目されます。

## 主要アップデート

### 1. コンピューティング

#### Amazon EC2 X8i インスタンス (GA)
- **発表日**: 2026 年 1 月 15 日
- **概要**: カスタム Intel Xeon 6 プロセッサを搭載したメモリ最適化インスタンス
- **主な特徴**:
  - X2i インスタンスと比較して最大 43% 高性能
  - メモリ容量 1.5 倍 (最大 6TB)
  - メモリ帯域幅 3.4 倍
  - SAP HANA、大規模データベース、EDA ワークロード向け
- **リージョン**: US East (N. Virginia)、US East (Ohio)、US West (Oregon)、Europe (Frankfurt)
- **参考**: [What's New](https://aws.amazon.com/about-aws/whats-new/2026/01/aws-amazon-ec2-x8i-generally-available)

### 2. ハイブリッドクラウド

#### AWS Outposts - 複数 LGW ルーティングドメインサポート
- **発表日**: 2026 年 1 月 16 日
- **概要**: Outpost あたり最大 10 個の独立したルーティングドメインを作成可能
- **主な特徴**:
  - 各ルーティングドメインに独立したルートテーブルと BGP セッション
  - ドメイン間のトラフィック分離
  - CoIP と DVR モードの同一 Outpost 上での共存
  - 追加料金なし
- **ユースケース**: マルチテナント環境、コンプライアンス要件
- **参考**: [Blog](https://aws.amazon.com/blogs/compute/simplify-network-segmentation-for-aws-outposts-racks-with-multiple-local-gateway-routing-domains/)

#### Amazon S3 on Outposts - 第 2 世代 Outposts racks 対応
- **発表日**: 2026 年 1 月 15 日
- **概要**: 第 2 世代 AWS Outposts racks で S3 on Outposts が利用可能に
- **ストレージティア**: 196 TB、490 TB、786 TB

### 3. サーバーレス

#### AWS Lambda - DynamoDB Streams クロスアカウントアクセス
- **発表日**: 2026 年 1 月 15 日
- **概要**: 別アカウントの DynamoDB Streams から Lambda 関数をトリガー可能に
- **主な特徴**:
  - リソースベースポリシーによるクロスアカウントアクセス制御
  - データレプリケーションソリューションの簡素化
  - マルチアカウントアーキテクチャの運用オーバーヘッド削減
- **参考**: [Documentation](https://docs.aws.amazon.com/lambda/latest/dg/services-dynamodb-eventsourcemapping.html#services-dynamodb-eventsourcemapping-cross-account)

### 4. データベース

#### Amazon RDS for SQL Server - 最新 GDR アップデート
- **発表日**: 2026 年 1 月 15 日
- **対象バージョン**:
  - SQL Server 2016 SP3+GDR (KB5068401)
  - SQL Server 2017 CU31+GDR (KB5068402)
  - SQL Server 2019 CU32+GDR (KB5068404)
  - SQL Server 2022 CU22 (KB5068450)
- **セキュリティ**: CVE-2025-59499 の脆弱性に対応

#### Amazon RDS Custom for SQL Server - GDR アップデート
- **発表日**: 2026 年 1 月 15 日
- **対象バージョン**:
  - SQL Server 2019 CU32+GDR (KB5068404)
  - SQL Server 2022 CU21+GDR (KB5068406)

#### Amazon Neptune Database - R7g/R8g インスタンス追加
- **発表日**: 2026 年 1 月 13 日
- **主な特徴**:
  - Graviton3 ベース R7g および Graviton4 ベース R8g インスタンス
  - R6g と比較して価格 -16%
  - Asia Pacific (Hong Kong)、Asia Pacific (Osaka)、Asia Pacific (Singapore)、Canada (Central)、US West (N. California) で利用可能

#### Amazon Redshift Serverless - キューベースのクエリリソース管理
- **発表日**: 2026 年 1 月 15 日
- **主な特徴**:
  - 専用クエリキューとカスタマイズ可能な監視ルール
  - ワークロード別のリソース使用量の詳細な制御
  - メトリクスベースの述語と自動化された応答

### 5. ネットワーク

#### Amazon VPC IPAM ポリシー - RDS と ALB サポート
- **発表日**: 2026 年 1 月 14 日
- **概要**: RDS インスタンスと Application Load Balancer の IP 割り当て戦略を一元管理
- **主な特徴**:
  - パブリック IP 割り当てルールの集中定義
  - アプリケーションチームによる上書き不可
  - ネットワークおよびセキュリティ管理の簡素化

#### Amazon VPC Route Server - 16 の新リージョン対応
- **発表日**: 2026 年 1 月 14 日
- **概要**: 合計 30 リージョンで利用可能に
- **主な機能**: VPC 内の仮想アプライアンス間の動的ルーティング簡素化

### 6. AI/ML

#### Amazon Lex - 改善された音声認識モデル (英語)
- **発表日**: 2026 年 1 月 13 日
- **主な特徴**:
  - ニューラル ASR モデル
  - 会話的な話し方パターンの認識精度向上
  - 非ネイティブスピーカーや地域アクセントへの対応

#### Amazon Lex - 音声アクティビティ検出感度の設定
- **発表日**: 2026 年 1 月 12 日
- **感度レベル**: Default、High、Maximum
- **ユースケース**: 製造現場、建設現場、騒音の多い環境

### 7. 開発者ツール

#### AWS Transform custom - AWS PrivateLink サポート
- **発表日**: 2026 年 1 月 14 日
- **主な特徴**:
  - VPC から PrivateLink 経由でアクセス可能
  - パブリックインターネット経由のトラフィックを回避
  - Europe (Frankfurt) リージョンで利用可能に
- **用途**: コード変換の自動化、技術的負債の削減

### 8. コンタクトセンター

#### Amazon Connect - エージェントスクリーン録画ステータス追跡
- **発表日**: 2026 年 1 月 12 日
- **概要**: EventBridge 経由で CloudWatch でスクリーン録画のステータスをリアルタイム監視
- **追跡情報**: 成功/失敗、失敗コード、クライアントバージョン、ブラウザバージョン、OS

#### Amazon Connect - 営業時間の定期的なオーバーライド管理
- **発表日**: 2026 年 1 月 14 日
- **主な特徴**:
  - ビジュアルカレンダーでの管理
  - 週次、月次、隔週金曜日などの定期パターン設定
  - 休日やメンテナンスウィンドウの自動処理

#### Amazon Connect Cases - AWS CloudFormation サポート
- **発表日**: 2026 年 1 月 13 日
- **概要**: Infrastructure as Code で Cases リソースを管理可能

### 9. データ分析

#### AWS Clean Rooms - PySpark 分析テンプレートのパラメータサポート
- **発表日**: 2026 年 1 月 15 日
- **主な特徴**:
  - 単一テンプレートで異なる値を動的に提供
  - ジョブ実行時にパラメータ値を送信
  - 広告キャンペーン分析などのユースケース

### 10. ストレージ

#### Amazon EBS - Elastic Volumes の変更上限を 24 時間で 4 回に拡大
- **発表日**: 2026 年 1 月 15 日
- **主な特徴**:
  - ローリング 24 時間ウィンドウ内で最大 4 回の変更
  - 前の変更完了直後に新しい変更を開始可能
  - サイズ、タイプ、パフォーマンスの変更をサポート

### 11. セキュリティ

#### Amazon Inspector - Java Gradle サポートとエコシステム拡張
- **発表日**: 2026 年 1 月 12 日
- **新規サポート**:
  - Java Gradle (gradle.lockfile ベース)
  - MySQL、MariaDB、PHP、Jenkins-core
  - 7zip (Windows)、Elasticsearch、Curl/LibCurl

### 12. IoT

#### AWS IoT Device Management - Wi-Fi Simple Setup (WSS)
- **発表日**: 2026 年 1 月 14 日
- **主な特徴**:
  - QR コードスキャンによる Wi-Fi プロビジョニング簡素化
  - デバイスセットアップ時間の短縮
  - カスタマーサポートの必要性削減

### 13. コスト管理

#### AWS Billing Console - 強化されたトランザクションビュー
- **発表日**: 2026 年 1 月 14 日
- **主な改善**:
  - ページ読み込み時間が分単位からミリ秒単位に
  - 残高追跡の改善
  - トランザクションステータスインジケータ
  - Billing Transfer の "Usage Consolidation Account" カラム

## API 変更履歴

### 今週の主要な API 変更

1. **[Amazon Connect Service](https://awsapichanges.com/archive/changes/279b5b-connect.html)** (2026-01-16)
   - 6 つのメソッド更新
   - Dispute 設定を持つフォームの作成サポート

2. **[AWS Clean Rooms Service](https://awsapichanges.com/archive/changes/b64df2-cleanrooms.html)** (2026-01-15)
   - 3 つのメソッド更新
   - PySpark 分析テンプレートのパラメータサポート

3. **[Amazon EC2 Container Service](https://awsapichanges.com/archive/changes/b64df2-ecs.html)** (2026-01-15)
   - 4 つのメソッド更新
   - AWS GovCloud (US) での FIPS 設定サポート

4. **[OpenSearch Service Serverless](https://awsapichanges.com/archive/changes/b64df2-aoss.html)** (2026-01-15)
   - 5 つの新規メソッド、3 つのメソッド更新
   - コレクショングループによるコンピュートリソース共有

5. **[Amazon Connect Service](https://awsapichanges.com/archive/changes/10cfc3-connect.html)** (2026-01-14)
   - 3 つの新規メソッド、9 つのメソッド更新
   - 営業時間の定期的なイベント自動スケジューリング

6. **[Amazon Redshift](https://awsapichanges.com/archive/changes/10cfc3-redshift.html)** (2026-01-14)
   - 20 のメソッド更新
   - 自動最適化のための追加コンピュートリソースサポート

7. **[Managed integrations for AWS IoT Device Management](https://awsapichanges.com/archive/changes/526fea-api.iotmanagedintegrations.html)** (2026-01-12)
   - 11 のメソッド更新
   - Wi-Fi Simple Setup (WSS) と 2P デバイス機能再検出

## リージョン拡大

以下のサービスが新しいリージョンで利用可能になりました:

- **Amazon MSK Connect**: Asia Pacific (New Zealand)、AWS GovCloud (US-East)、AWS GovCloud (US-West)
- **Amazon Redshift Serverless**: Asia Pacific (New Zealand)
- **Amazon VPC Route Server**: 16 の新リージョン (合計 30 リージョン)
- **AWS Transform custom**: Europe (Frankfurt)

## 今週のハイライト

### 1. メモリ集約型ワークロードの性能向上
Amazon EC2 X8i インスタンスの GA により、SAP HANA、大規模データベース、EDA ワークロードのパフォーマンスが大幅に向上しました。

### 2. ハイブリッドクラウドのネットワークセグメンテーション簡素化
AWS Outposts の複数 LGW ルーティングドメインサポートにより、マルチテナント環境での運用が大幅に簡素化されました。

### 3. マルチアカウントアーキテクチャの運用改善
AWS Lambda の DynamoDB Streams クロスアカウントアクセスにより、複雑なデータレプリケーションソリューションが不要になりました。

### 4. セキュリティアップデート
Amazon RDS for SQL Server および RDS Custom for SQL Server で重要なセキュリティパッチ (CVE-2025-59499) が適用されました。

## 推奨アクション

1. **セキュリティパッチの適用**: SQL Server を使用している場合は、最新の GDR アップデートへのアップグレードを検討してください
2. **パフォーマンス評価**: メモリ集約型ワークロードの場合、Amazon EC2 X8i インスタンスでのパフォーマンステストを実施してください
3. **コスト最適化**: Amazon Neptune を使用している場合、R7g/R8g インスタンスへの移行で 16% のコスト削減が見込めます
4. **マルチアカウント戦略の見直し**: Lambda と DynamoDB を使用している場合、クロスアカウントアクセス機能でアーキテクチャを簡素化できないか検討してください

## まとめ

2026 年第 3 週は、パフォーマンス、セキュリティ、運用効率の各分野で重要なアップデートがありました。特に、Amazon EC2 X8i インスタンス、AWS Outposts の複数 LGW ルーティングドメイン、AWS Lambda の DynamoDB Streams クロスアカウントアクセスは、エンタープライズワークロードの運用に大きな影響を与える可能性があります。セキュリティパッチの適用と新機能の評価を優先して進めることをお勧めします。

## 参考リンク

- [AWS What's New](https://aws.amazon.com/new/)
- [AWS Blog](https://aws.amazon.com/blogs/aws/)
- [AWS API Changes](https://awsapichanges.com/)
