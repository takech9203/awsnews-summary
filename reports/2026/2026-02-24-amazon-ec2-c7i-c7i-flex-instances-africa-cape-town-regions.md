# Amazon EC2 - C7i および C7i-flex インスタンスがアフリカ (ケープタウン) リージョンで利用可能に

**リリース日**: 2026 年 2 月 24 日
**サービス**: Amazon EC2
**機能**: C7i および C7i-flex インスタンスの Africa (Cape Town) リージョンへの拡大

[このアップデートのインフォグラフィックを見る](https://takech9203.github.io/aws-news-summary/20260224-amazon-ec2-c7i-c7i-flex-instances-africa-cape-town-regions.html)

## 概要

Amazon EC2 C7i-flex および C7i インスタンスが、アフリカ (ケープタウン) リージョンで利用可能になりました。これらのインスタンスは、AWS 専用のカスタム第 4 世代 Intel Xeon Scalable プロセッサ (コードネーム Sapphire Rapids) を搭載したコンピューティング最適化インスタンスです。同等の x86 ベースの Intel プロセッサと比較して最大 15% 優れたパフォーマンスを提供します。

C7i-flex インスタンスは C6i インスタンスと比較して最大 19% 優れた価格パフォーマンスを実現し、large から 16xlarge までのサイズで提供されます。C7i インスタンスは C6i と比較して最大 15% 優れた価格パフォーマンスを実現し、最大 48xlarge と 2 つのベアメタルサイズで提供されます。また、Data Streaming Accelerator (DSA)、In-Memory Analytics Accelerator (IAA)、QuickAssist Technology (QAT) の内蔵 Intel アクセラレータを搭載しています。

**アップデート前の課題**

- C7i および C7i-flex インスタンスがアフリカ (ケープタウン) リージョンで利用できなかった
- アフリカリージョンのお客様は、第 4 世代 Intel Xeon Scalable プロセッサの性能を活用できなかった
- コンピューティング集約型ワークロードで、データレジデンシー要件を満たしながら最新のコンピューティング最適化インスタンスを利用できなかった

**アップデート後の改善**

- アフリカ (ケープタウン) リージョンで C7i および C7i-flex インスタンスを直接起動できるようになった
- C6i インスタンスからの移行で最大 19% の価格パフォーマンス改善を実現できる
- データローカリティとコンプライアンス要件を満たしながら、最新のコンピューティング最適化インスタンスを利用可能になった

## サービスアップデートの詳細

### 主要機能

1. **C7i-flex インスタンス**
   - C6i と比較して最大 19% 優れた価格パフォーマンス
   - large から 16xlarge まで、最も一般的な 7 サイズを提供
   - 最大 64 vCPU、128 GiB メモリ
   - すべてのコンピューティングリソースを完全に活用しないアプリケーションに最適な選択肢
   - Web およびアプリケーションサーバー、データベース、キャッシュ、Apache Kafka、Elasticsearch に適している

2. **C7i インスタンス**
   - C6i と比較して最大 15% 優れた価格パフォーマンス
   - 2 つのベアメタルサイズを含む 11 サイズを提供
   - 最大 192 vCPU、384 GiB メモリ (48xlarge)
   - 継続的な高 CPU 使用率が必要なワークロードに最適
   - バッチ処理、分散分析、HPC、広告配信、動画エンコーディングに適している

3. **カスタム第 4 世代 Intel Xeon Scalable プロセッサ**
   - オールコアターボ周波数 3.2 GHz (最大コアターボ周波数 3.8 GHz)
   - AWS 専用のカスタムプロセッサで、同等の Intel プロセッサと比較して最高のパフォーマンス
   - Intel Total Memory Encryption (TME) による常時メモリ暗号化をサポート

4. **内蔵 Intel アクセラレータ**
   - **Advanced Matrix Extensions (AMX)**: C7i-flex と C7i の両方で利用可能。CPU ベースの ML 向けの行列乗算を高速化
   - **Data Streaming Accelerator (DSA)**: C7i ベアメタルサイズで利用可能。データストリーミング操作を効率化
   - **In-Memory Analytics Accelerator (IAA)**: C7i ベアメタルサイズで利用可能。インメモリ分析を高速化
   - **QuickAssist Technology (QAT)**: C7i ベアメタルサイズで利用可能。暗号化と圧縮処理を高速化

## 技術仕様

### C7i-flex インスタンスサイズ

| インスタンスサイズ | vCPU | メモリ (GiB) | ネットワーク帯域幅 (Gbps) | EBS 帯域幅 (Gbps) |
|-------------------|------|-------------|--------------------------|-------------------|
| c7i-flex.large | 2 | 4 | 最大 12.5 | 最大 10 |
| c7i-flex.xlarge | 4 | 8 | 最大 12.5 | 最大 10 |
| c7i-flex.2xlarge | 8 | 16 | 最大 12.5 | 最大 10 |
| c7i-flex.4xlarge | 16 | 32 | 最大 12.5 | 最大 10 |
| c7i-flex.8xlarge | 32 | 64 | 最大 12.5 | 最大 10 |
| c7i-flex.12xlarge | 48 | 96 | 最大 18.75 | 最大 15 |
| c7i-flex.16xlarge | 64 | 128 | 最大 25 | 最大 20 |

### C7i インスタンスサイズ

| インスタンスサイズ | vCPU | メモリ (GiB) | ネットワーク帯域幅 (Gbps) | EBS 帯域幅 (Gbps) |
|-------------------|------|-------------|--------------------------|-------------------|
| c7i.large | 2 | 4 | 最大 12.5 | 最大 10 |
| c7i.xlarge | 4 | 8 | 最大 12.5 | 最大 10 |
| c7i.2xlarge | 8 | 16 | 最大 12.5 | 最大 10 |
| c7i.4xlarge | 16 | 32 | 最大 12.5 | 最大 10 |
| c7i.8xlarge | 32 | 64 | 12.5 | 10 |
| c7i.12xlarge | 48 | 96 | 18.75 | 15 |
| c7i.16xlarge | 64 | 128 | 25 | 20 |
| c7i.24xlarge | 96 | 192 | 37.5 | 30 |
| c7i.48xlarge | 192 | 384 | 50 | 40 |
| c7i.metal-24xl | 96 | 192 | 37.5 | 30 |
| c7i.metal-48xl | 192 | 384 | 50 | 40 |

### パフォーマンス比較

| 指標 | C7i-flex vs C6i | C7i vs C6i |
|------|----------------|------------|
| 価格パフォーマンス | 最大 19% 向上 | 最大 15% 向上 |
| プロセッサ性能 | 同等 x86 Intel 比 15% 向上 | 同等 x86 Intel 比 15% 向上 |

## 設定方法

### 前提条件

1. AWS アカウントとアフリカ (ケープタウン) リージョンへのアクセス権限
2. C7i/C7i-flex インスタンスタイプのサービスクォータ確認
3. 適切な VPC およびサブネット設定

### 手順

#### ステップ 1: C7i-flex インスタンスの起動

```bash
# AWS CLI を使用して C7i-flex インスタンスを起動
aws ec2 run-instances \
  --image-id ami-xxxxxxxxxxxxxxxxx \
  --instance-type c7i-flex.large \
  --region af-south-1 \
  --subnet-id subnet-xxxxxxxxxxxxxxxxx \
  --security-group-ids sg-xxxxxxxxxxxxxxxxx \
  --key-name my-key-pair
```

アフリカ (ケープタウン) リージョン (af-south-1) で C7i-flex インスタンスを起動するコマンドです。

#### ステップ 2: 利用可能なインスタンスタイプの確認

```bash
# 利用可能な C7i インスタンスタイプを確認
aws ec2 describe-instance-types \
  --filters "Name=instance-type,Values=c7i*" \
  --region af-south-1 \
  --query "InstanceTypes[].{Type:InstanceType,vCPU:VCpuInfo.DefaultVCpus,Memory:MemoryInfo.SizeInMiB}" \
  --output table
```

ケープタウンリージョンで利用可能な C7i インスタンスタイプとスペックを一覧表示するコマンドです。

#### ステップ 3: 購入オプションの選択

C7i および C7i-flex インスタンスは、以下の購入オプションで利用できます。

- **オンデマンドインスタンス**: 使用した分だけ支払い
- **Savings Plans**: 1 年または 3 年のコミットメントで割引
- **スポットインスタンス**: 未使用の EC2 容量を大幅な割引で利用

## メリット

### ビジネス面

- **コスト効率の向上**: C6i と比較して最大 19% (C7i-flex) または 15% (C7i) 優れた価格パフォーマンスにより、コンピューティングコストを削減
- **リージョン拡大**: アフリカ (ケープタウン) リージョンでの提供により、アフリカ地域のお客様がデータレジデンシー要件を満たしながら最新インスタンスを利用可能
- **柔軟なサイジング**: C7i-flex は large から 16xlarge、C7i は最大 48xlarge と 2 つのベアメタルサイズを提供し、ワークロードに最適なサイズを選択可能

### 技術面

- **高性能プロセッサ**: AWS 専用のカスタム第 4 世代 Intel Xeon Scalable プロセッサによる最高のパフォーマンス
- **DDR5 メモリ**: C6i と比較して高いメモリ帯域幅を提供
- **内蔵アクセラレータ**: DSA、IAA、QAT によりデータベース、暗号化、圧縮処理を高速化
- **AWS Nitro System**: 最新の AWS Nitro System により、セキュリティとパフォーマンスを最適化

## デメリット・制約事項

### 制限事項

- DSA、IAA、QAT アクセラレータは C7i ベアメタルサイズでのみ利用可能
- EFA ネットワーキングは C7i の 48xlarge および metal-48xl サイズでのみサポート
- 新規リージョンでの初期のサービスクォータが制限される場合がある

### 考慮すべき点

- C7i-flex はすべてのリソースを完全に活用しないワークロードに最適だが、継続的な高 CPU 使用率が必要な場合は C7i を選択すべき
- 既存の C6i インスタンスからの自動移行はサポートされていないため、新規起動で移行する必要がある
- 価格パフォーマンスを最大化するには、Savings Plans やスポットインスタンスの利用を検討

## ユースケース

### ユースケース 1: Web アプリケーションサーバー

**シナリオ**: アフリカ地域のユーザーにサービスを提供する Web アプリケーションを、低レイテンシーで実行したい。

**実装例**:
```bash
# C7i-flex インスタンスで Web サーバーを起動
aws ec2 run-instances \
  --image-id ami-xxxxxxxxxxxxxxxxx \
  --instance-type c7i-flex.4xlarge \
  --region af-south-1 \
  --subnet-id subnet-xxxxxxxxxxxxxxxxx \
  --security-group-ids sg-xxxxxxxxxxxxxxxxx
```

**効果**: C6i と比較して最大 19% 優れた価格パフォーマンスにより、同じコストでより多くのリクエストを処理可能。アフリカ地域へのレイテンシーも大幅に低減。

### ユースケース 2: バッチ処理と HPC

**シナリオ**: 大規模なデータ処理やシミュレーションをアフリカリージョンで実行する必要がある。

**実装例**:
```bash
# C7i インスタンスでバッチ処理を実行
aws ec2 run-instances \
  --image-id ami-xxxxxxxxxxxxxxxxx \
  --instance-type c7i.48xlarge \
  --region af-south-1 \
  --iam-instance-profile Name=HPC-Processing-Role
```

**効果**: 最大 192 vCPU と 384 GiB メモリにより、大規模なバッチ処理を高速に実行。データレジデンシー要件を満たしながら、高性能コンピューティングを実現。

## 料金

C7i および C7i-flex インスタンスの料金は、インスタンスタイプ、リージョン、購入オプションによって異なります。

購入オプション:
- **オンデマンド**: 時間単位の従量課金
- **Savings Plans**: 1 年または 3 年の利用契約で割引
- **スポットインスタンス**: 余剰キャパシティを活用して大幅な割引で利用

詳細な料金については、[Amazon EC2 料金ページ](https://aws.amazon.com/ec2/pricing/) を参照してください。

## 利用可能リージョン

C7i および C7i-flex インスタンスは、今回のアップデートでアフリカ (ケープタウン) リージョンが追加されました。

**新規対応リージョン (2026 年 2 月 24 日)**:
- アフリカ (ケープタウン) - af-south-1

最新のリージョン情報は [AWS Regional Services List](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) を参照してください。

## 関連サービス・機能

- **Amazon EC2 C6i インスタンス**: C7i/C7i-flex の前世代のコンピューティング最適化インスタンス
- **Amazon EC2 Auto Scaling**: C7i/C7i-flex インスタンスの自動スケーリング
- **Elastic Load Balancing**: 複数の C7i/C7i-flex インスタンス間での負荷分散
- **AWS Compute Optimizer**: ワークロードに最適なインスタンスタイプの推奨
- **AWS Nitro System**: EC2 インスタンスに高パフォーマンスと高セキュリティを提供する基盤

## 参考リンク

- [インフォグラフィック](https://takech9203.github.io/aws-news-summary/20260224-amazon-ec2-c7i-c7i-flex-instances-africa-cape-town-regions.html)
- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-ec2-c7i-c7i-flex-instances-africa-cape-town-regions/)
- [C7i インスタンスタイプページ](https://aws.amazon.com/ec2/instance-types/c7i/)
- [Amazon EC2 料金ページ](https://aws.amazon.com/ec2/pricing/)
- [Amazon EC2 ドキュメント](https://docs.aws.amazon.com/ec2/)

## まとめ

Amazon EC2 C7i および C7i-flex インスタンスがアフリカ (ケープタウン) リージョンで利用可能になりました。カスタム第 4 世代 Intel Xeon Scalable プロセッサ (Sapphire Rapids) を搭載し、C6i インスタンスと比較して C7i-flex は最大 19%、C7i は最大 15% 優れた価格パフォーマンスを提供します。C7i-flex は large から 16xlarge までのサイズで汎用的なコンピューティングワークロードに、C7i は最大 48xlarge と 2 つのベアメタルサイズで大規模なコンピューティング集約型ワークロードに最適です。DSA、IAA、QAT の内蔵アクセラレータにより、データベースや暗号化処理の高速化も実現します。アフリカ地域でコンピューティングワークロードを実行しているお客様は、C7i/C7i-flex インスタンスへの移行を検討し、パフォーマンスとコスト効率の向上を実現してください。
