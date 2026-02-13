# AWS Lambda Managed Instances - サーバーレスの簡素性と EC2 の柔軟性を両立

**リリース日**: 2025 年 12 月 2 日  
**サービス**: AWS Lambda  
**機能**: Lambda Managed Instances


## 概要

AWS は AWS Lambda Managed Instances を発表しました。これは、Lambda 関数を Amazon EC2 インスタンス上で実行しながら、サーバーレスの運用簡素性を維持できる新機能です。特殊なコンピュート構成へのアクセスと EC2 の価格モデルによるコスト最適化を、Lambda の利点を犠牲にすることなく実現します。

Lambda Managed Instances では、AWS がインスタンスライフサイクル管理、OS パッチ適用、ロードバランシング、オートスケーリングなどの運用複雑性を処理します。各実行環境は複数のリクエストを処理でき、コンピュート消費を削減し、Amazon EC2 のコミットメントベースの価格モデルへのアクセスを提供します。

**アップデート前の課題**

- Lambda の標準実行環境では、特殊なハードウェア構成 (GPU、特定の CPU アーキテクチャなど) にアクセスできなかった
- EC2 の Savings Plans や Reserved Instances の価格メリットを Lambda ワークロードに適用できなかった
- 高トラフィックアプリケーションでは、Lambda の従量課金モデルがコスト効率的でない場合があった


**アップデート後の改善**

- Lambda 関数を EC2 インスタンス上で実行しながら、サーバーレスの運用簡素性を維持
- Compute Savings Plans や Reserved Instances を活用したコスト最適化が可能に
- 特殊なハードウェア構成や最新のプロセッサアーキテクチャにアクセス可能


## サービスアップデートの詳細

### 主要機能

1. **フルマネージドインフラストラクチャ**
   - インスタンスライフサイクル管理を AWS が自動処理
   - OS パッチ適用とセキュリティ更新を自動化
   - ロードバランシングとオートスケーリングを自動管理

2. **キャパシティプロバイダー**
   - VPC、サブネット構成、セキュリティグループを指定
   - EC2 インスタンスタイプを選択可能
   - プリプロビジョニングされた実行環境でコールドスタートを排除

3. **マルチリクエスト処理**
   - 各実行環境が複数のリクエストを処理
   - リソース使用率を最大化
   - コンピュート消費を削減

4. **EC2 価格モデルへのアクセス**
   - Compute Savings Plans の適用
   - Reserved Instances の活用
   - 予測可能なワークロードでのコスト最適化


## 技術仕様

### サポートされるランタイム

| ランタイム | サポート状況 |
|-----------|-------------|
| Node.js (最新版) | ✅ |
| Java (最新版) | ✅ |
| Python (最新版) | ✅ |
| .NET (最新版) | ✅ |

### 統合サービス

| サービス | 説明 |
|---------|------|
| Amazon CloudWatch | メトリクスとログの監視 |
| AWS X-Ray | 分散トレーシング |
| AWS Config | 構成管理 |
| すべての Lambda イベントソース | 既存のイベントソースと互換 |

### 料金構成

| 項目 | 説明 |
|------|------|
| Lambda リクエスト料金 | 標準の Lambda リクエスト料金 |
| EC2 インスタンス料金 | 標準の Amazon EC2 インスタンス料金 |
| コンピュート管理料金 | マネージドサービスの管理料金 |


## 設定方法

### 前提条件

1. AWS アカウント
2. VPC とサブネットの設定
3. 適切な IAM 権限

### 手順

#### ステップ 1: キャパシティプロバイダーの作成

Lambda コンソール、AWS CLI、または IaC ツールでキャパシティプロバイダーを作成します。

```bash
# AWS CLI でキャパシティプロバイダーを作成
aws lambda create-capacity-provider \
  --name my-capacity-provider \
  --vpc-config SubnetIds=subnet-xxx,SecurityGroupIds=sg-xxx \
  --instance-types m6i.large,m6i.xlarge
```

#### ステップ 2: Lambda 関数をキャパシティプロバイダーにアタッチ

```bash
# Lambda 関数をキャパシティプロバイダーにアタッチ
aws lambda update-function-configuration \
  --function-name my-function \
  --capacity-provider my-capacity-provider
```

#### ステップ 3: 動作確認

Lambda が自動的にリクエストをプリプロビジョニングされた実行環境にルーティングします。


## メリット

### ビジネス面

- **コスト最適化**: EC2 の Savings Plans や Reserved Instances を活用して最大 70% のコスト削減
- **予測可能なコスト**: 高トラフィックアプリケーションでの予測可能な料金体系
- **柔軟性**: ワークロードに応じて Lambda 標準と Managed Instances を使い分け

### 技術面

- **コールドスタートの排除**: プリプロビジョニングされた実行環境で即座に応答
- **特殊ハードウェアへのアクセス**: GPU、最新 CPU アーキテクチャなどを利用可能
- **運用簡素性の維持**: インフラ管理は AWS が担当


## デメリット・制約事項

### 制限事項

- 一部のリージョンでのみ利用可能
- キャパシティプロバイダーの設定が必要
- EC2 インスタンスの管理料金が追加

### 考慮すべき点

- 低トラフィックワークロードでは標準 Lambda の方がコスト効率的な場合がある
- VPC 設定が必要


## ユースケース

### ユースケース 1: 高トラフィック API

**シナリオ**: 毎秒数千リクエストを処理する API バックエンド

**実装例**:
```
1. キャパシティプロバイダーを作成 (m6i.xlarge インスタンス)
2. API Gateway + Lambda 関数をアタッチ
3. Compute Savings Plans を適用
```

**効果**: 従量課金と比較して最大 70% のコスト削減、コールドスタートなし

### ユースケース 2: GPU を使用した ML 推論

**シナリオ**: リアルタイム画像認識 API

**実装例**:
```
1. GPU インスタンス (g5.xlarge) でキャパシティプロバイダーを作成
2. ML 推論 Lambda 関数をアタッチ
3. GPU アクセラレーションを活用
```

**効果**: Lambda から GPU にアクセスし、高速な ML 推論を実現

### ユースケース 3: 定常的なバッチ処理

**シナリオ**: 毎日実行される大規模データ処理

**実装例**:
```
1. Reserved Instances を購入
2. キャパシティプロバイダーを作成
3. バッチ処理 Lambda 関数をアタッチ
```

**効果**: Reserved Instances の価格メリットを Lambda ワークロードに適用


## 料金

| 項目 | 説明 |
|------|------|
| Lambda リクエスト料金 | 標準の Lambda 料金 |
| EC2 インスタンス料金 | 使用したインスタンスの料金 |
| コンピュート管理料金 | マネージドサービス料金 |

Compute Savings Plans や Reserved Instances を適用することで、オンデマンド料金から最大 70% の割引が可能です。

詳細は [AWS Lambda 料金ページ](https://aws.amazon.com/lambda/pricing/) を参照してください。


## 利用可能リージョン

一部のリージョンで利用可能です。詳細は AWS Lambda コンソールを確認してください。


## 関連サービス・機能

- **AWS Lambda**: サーバーレスコンピューティング
- **Amazon EC2**: 仮想サーバー
- **Compute Savings Plans**: コンピュートの割引プラン
- **AWS Lambda Durable Functions**: マルチステップアプリケーション


## 参考リンク

- [公式発表](https://aws.amazon.com/blogs/aws/introducing-aws-lambda-managed-instances-serverless-simplicity-with-ec2-flexibility/)
- [What's New](https://aws.amazon.com/about-aws/whats-new/2025/11/aws-lambda-managed-instances/)
- [AWS Lambda 料金ページ](https://aws.amazon.com/lambda/pricing/)


## まとめ

AWS Lambda Managed Instances は、サーバーレスの簡素性と EC2 の柔軟性を両立する革新的な機能です。高トラフィックアプリケーション、特殊なハードウェア要件、予測可能なワークロードを持つ組織にとって、コスト最適化と運用効率の両方を実現する強力なオプションとなります。re:Invent 2025 で発表されたこの機能は、Lambda の適用範囲を大幅に拡大します。
