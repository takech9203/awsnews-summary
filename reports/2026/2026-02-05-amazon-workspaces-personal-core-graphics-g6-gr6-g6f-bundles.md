# Amazon WorkSpaces - Graphics G6, Gr6, G6f バンドル

**リリース日**: 2026 年 2 月 5 日
**サービス**: Amazon WorkSpaces
**機能**: Graphics G6, Gr6, G6f バンドル

📊 [このアップデートのインフォグラフィックを見る](https://takech9203.github.io/aws-news-summary/20260205-amazon-workspaces-personal-core-graphics-g6-gr6-g6f-bundles.html)

## 概要

Amazon WorkSpaces が Amazon EC2 G6 ファミリーをベースにした 12 種類の新しい Graphics G6、Gr6、G6f WorkSpaces バンドルを発表しました。これらのバンドルは、グラフィックス集約型および GPU アクセラレーテッドワークロードの実行オプションを拡充し、Amazon WorkSpaces Personal と Amazon WorkSpaces Core の両方で利用可能です。

新しいバンドルは、幅広いパフォーマンス、メモリ、コスト要件をサポートするよう設計されています。G6 バンドルはグラフィックデザインや CAD/CAM に、Gr6 バンドルは 3D レンダリングや GIS 処理に、G6f バンドルはフラクショナル GPU オプションでコスト効率の高い GPU アクセスを提供します。

**アップデート前の課題**

- GPU ワークロード向けのバンドルオプションが限定的だった
- フル GPU を必要としないワークロードでもフル GPU コストが発生していた
- メモリ最適化された GPU バンドルの選択肢が少なかった

**アップデート後の改善**

- 12 種類の新しい GPU バンドルから最適な構成を選択可能
- フラクショナル GPU (1/8, 1/4, 1/2) オプションでコスト効率向上
- メモリ最適化 (1:8 vCPU-to-memory) バンドルで高メモリワークロードに対応

## サービスアップデートの詳細

### バンドル構成

#### G6 バンドル (1:4 vCPU-to-memory)

| サイズ | vCPU | メモリ | GPU | ユースケース |
|--------|------|--------|-----|--------------|
| xlarge | 4 | 16 GB | 1 | 軽量グラフィックス |
| 2xlarge | 8 | 32 GB | 1 | グラフィックデザイン |
| 4xlarge | 16 | 64 GB | 1 | CAD/CAM |
| 8xlarge | 32 | 128 GB | 1 | ML モデルトレーニング |
| 16xlarge | 64 | 256 GB | 1 | 大規模ワークロード |

#### Gr6 バンドル (1:8 vCPU-to-memory、メモリ最適化)

| サイズ | vCPU | メモリ | GPU | ユースケース |
|--------|------|--------|-----|--------------|
| 4xlarge | 16 | 128 GB | 1 | 3D レンダリング |
| 8xlarge | 32 | 256 GB | 1 | 地震データ可視化、GIS |

#### G6f バンドル (フラクショナル GPU)

| サイズ | vCPU | メモリ | GPU 割り当て | ユースケース |
|--------|------|--------|--------------|--------------|
| large | 2 | 8 GB | 1/8 GPU | 軽量 GPU タスク |
| xlarge | 4 | 16 GB | 1/4 GPU | 一般的な GPU 作業 |
| 2xlarge | 8 | 32 GB | 1/4 GPU | 中規模ワークロード |
| 4xlarge | 16 | 64 GB | 1/2 GPU | 中〜大規模ワークロード |

### サポートされる OS

| OS | サポート |
|----|----------|
| Windows Server 2022 | ✅ |
| Windows 11 (BYOL) | ✅ |

## 技術仕様

### EC2 G6 ファミリーの特徴

| 項目 | 詳細 |
|------|------|
| GPU | NVIDIA L40S Tensor Core GPU |
| GPU メモリ | 48 GB per GPU |
| プロセッサ | AMD EPYC (第 3 世代) |
| ネットワーク | 最大 100 Gbps |

### バンドル比較

| バンドルタイプ | vCPU:メモリ比 | GPU オプション | 最適なワークロード |
|----------------|---------------|----------------|-------------------|
| G6 | 1:4 | フル GPU | 汎用グラフィックス |
| Gr6 | 1:8 | フル GPU | 高メモリワークロード |
| G6f | 可変 | フラクショナル | コスト重視 |

## 設定方法

### 前提条件

1. Amazon WorkSpaces の設定
2. 対応リージョンでの利用
3. 適切なディレクトリ設定

### 手順

#### ステップ 1: WorkSpaces コンソールでバンドル選択

1. Amazon WorkSpaces コンソールにアクセス
2. 「Create WorkSpaces」を選択
3. バンドル選択画面で「Graphics G6」「Graphics Gr6」または「Graphics G6f」を選択

#### ステップ 2: サイズの選択

ワークロードに応じて適切なサイズを選択します。

```
# ワークロード別推奨サイズ
- グラフィックデザイン: G6.2xlarge
- CAD/CAM: G6.4xlarge または G6.8xlarge
- 3D レンダリング: Gr6.4xlarge または Gr6.8xlarge
- 軽量 GPU タスク: G6f.large または G6f.xlarge
```

#### ステップ 3: WorkSpace のプロビジョニング

設定を確認し、WorkSpace を作成します。

## メリット

### ビジネス面

- **コスト最適化**: フラクショナル GPU でフル GPU 不要なワークロードのコスト削減
- **柔軟性**: 12 種類のオプションから最適な構成を選択
- **BYOL サポート**: Windows 11 ライセンスの持ち込みでライセンスコスト削減

### 技術面

- **高性能**: NVIDIA L40S GPU による優れたグラフィックス性能
- **スケーラビリティ**: 小規模から大規模ワークロードまで対応
- **最新アーキテクチャ**: EC2 G6 ファミリーの最新機能を活用

## デメリット・制約事項

### 制限事項

- 13 リージョンでのみ利用可能
- フラクショナル GPU は一部のワークロードには不十分な場合あり

### 考慮すべき点

- GPU ワークロードは CPU ワークロードより高コスト
- ワークロードに適したバンドルサイズの選定が重要

## ユースケース

### ユースケース 1: CAD/CAM 設計

**シナリオ**: 製造業のエンジニアがリモートで 3D CAD を使用

**推奨バンドル**: G6.4xlarge または G6.8xlarge

**効果**: 複雑な 3D モデルをスムーズに操作し、生産性を向上

### ユースケース 2: 3D レンダリング

**シナリオ**: 映像制作会社が高品質な 3D レンダリングを実行

**推奨バンドル**: Gr6.8xlarge

**効果**: 大容量メモリで複雑なシーンを効率的にレンダリング

### ユースケース 3: コスト効率の高い GPU 開発

**シナリオ**: 開発者が機械学習の実験を行う

**推奨バンドル**: G6f.xlarge または G6f.2xlarge

**効果**: フラクショナル GPU でコストを抑えながら GPU を活用

## 料金

従量課金制で提供されます。詳細は以下を参照してください。

- [Amazon WorkSpaces 料金ページ](https://aws.amazon.com/workspaces/desktop-as-a-service/pricing/)
- [Amazon WorkSpaces Core 料金ページ](https://aws.amazon.com/workspaces/vdi-partners/pricing/)

## 利用可能リージョン

以下の 13 リージョンで利用可能です。

- US East (N. Virginia)
- US West (Oregon)
- Canada (Central)
- Europe (Paris, Frankfurt, London)
- Asia Pacific (Tokyo, Mumbai, Sydney, Seoul)
- South America (São Paulo)
- AWS GovCloud (US-West, US-East)

## 関連サービス・機能

- **Amazon EC2 G6**: 基盤となる GPU インスタンスファミリー
- **Amazon WorkSpaces Personal**: 個人用仮想デスクトップ
- **Amazon WorkSpaces Core**: VDI パートナー向けソリューション

## 参考リンク

- 📊 [インフォグラフィック](https://takech9203.github.io/aws-news-summary/20260205-amazon-workspaces-personal-core-graphics-g6-gr6-g6f-bundles.html)
- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-workspaces-personal-core-graphics-g6-gr6-g6f-bundles/)
- [EC2 G6 インスタンスページ](https://aws.amazon.com/ec2/instance-types/g6/)
- [Amazon WorkSpaces 料金](https://aws.amazon.com/workspaces/desktop-as-a-service/pricing/)
- [Amazon WorkSpaces Core 料金](https://aws.amazon.com/workspaces/vdi-partners/pricing/)

## まとめ

Amazon WorkSpaces の新しい Graphics G6、Gr6、G6f バンドルにより、グラフィックス集約型ワークロード向けの選択肢が大幅に拡充されました。特にフラクショナル GPU オプションは、フル GPU を必要としないワークロードでのコスト最適化に有効です。CAD/CAM、3D レンダリング、ML 開発など、GPU を活用するリモートワークを検討している組織は、これらの新しいバンドルの評価を推奨します。
