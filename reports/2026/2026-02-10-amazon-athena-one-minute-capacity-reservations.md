# Amazon Athena - 1 分間キャパシティ予約と 4 DPU 最小キャパシティ

**リリース日**: 2026 年 2 月 10 日
**サービス**: Amazon Athena
**機能**: 1 分間キャパシティ予約、4 DPU 最小キャパシティ

📊 [このアップデートのインフォグラフィックを見る](https://takech9203.github.io/aws-news-summary/20260210-amazon-athena-one-minute-capacity-reservations.html)

## 概要

Amazon Athena で 1 分間のキャパシティ予約と、すべての予約に対する 4 DPU (Data Processing Units) の最小キャパシティがサポートされました。これにより、より少ないキャパシティから開始し、ワークロードパターンに合わせてより頻繁できめ細かな調整が可能になりました。長期コミットメントなしで、短時間クエリワークロードで最大 95% のコスト削減を実現できます。

キャパシティ予約は専用のサーバーレスコンピューティングを提供し、クエリの優先順位付けと同時実行制御が必要なワークロードに最適です。予約したキャパシティのみに課金され、データスキャン料金は発生しません。

**アップデート前の課題**

- キャパシティ予約の最小単位が大きく、小規模ワークロードには過剰だった
- 予約時間の粒度が粗く、短時間のバーストワークロードに対応しにくかった
- 小規模なテストや開発環境でのコスト効率が悪かった

**アップデート後の改善**

- 4 DPU という低い最小キャパシティから開始可能
- 1 分単位の予約で短時間ワークロードに最適化
- 短時間クエリワークロードで最大 95% のコスト削減

## サービスアップデートの詳細

### 主要機能

1. **1 分間キャパシティ予約**
   - 最小 1 分からの予約が可能
   - ワークロードパターンに合わせた頻繁な調整
   - 長期コミットメント不要

2. **4 DPU 最小キャパシティ**
   - すべての予約で 4 DPU から開始可能
   - 小規模ワークロードのコスト最適化
   - 開発・テスト環境に最適

3. **既存機能との互換性**
   - 既存の Athena クエリとワークグループに変更不要
   - 予約にワークグループをアタッチするだけで利用可能
   - SQL クエリやアプリケーションコードの変更不要

## 技術仕様

### キャパシティ予約の設定項目

| 項目 | 以前 | 現在 |
|------|------|------|
| 最小キャパシティ | より大きな値 | 4 DPU |
| 最小予約時間 | より長い時間 | 1 分 |
| 料金モデル | 予約時間 | 予約時間 (より柔軟) |

### DPU とは

| 項目 | 説明 |
|------|------|
| DPU | Data Processing Unit |
| 構成 | 4 vCPU + 16 GB メモリ |
| 用途 | クエリ処理能力の単位 |

## 設定方法

### 前提条件

1. Amazon Athena が有効な AWS アカウント
2. S3 にクエリ対象のデータ
3. 適切な IAM 権限

### 手順

#### ステップ 1: キャパシティ予約の作成

```bash
aws athena create-capacity-reservation \
  --name my-reservation \
  --target-dpus 4
```

4 DPU のキャパシティ予約を作成します。

#### ステップ 2: ワークグループの作成とアタッチ

```bash
aws athena create-work-group \
  --name my-workgroup \
  --configuration '{
    "CapacityConfiguration": {
      "CapacityReservationName": "my-reservation"
    }
  }'
```

ワークグループを作成し、キャパシティ予約にアタッチします。

#### ステップ 3: キャパシティの調整

```bash
# キャパシティを増やす
aws athena update-capacity-reservation \
  --name my-reservation \
  --target-dpus 8

# 1 分後にキャパシティを減らす
aws athena update-capacity-reservation \
  --name my-reservation \
  --target-dpus 4
```

ワークロードに合わせてキャパシティを動的に調整します。

## メリット

### ビジネス面

- **コスト削減**: 短時間クエリで最大 95% のコスト削減
- **柔軟性**: 長期コミットメントなしで利用可能
- **スケーラビリティ**: ワークロードに合わせた動的調整

### 技術面

- **シンプルな移行**: 既存のクエリとコードの変更不要
- **きめ細かな制御**: 1 分単位での予約調整
- **予測可能なパフォーマンス**: 専用コンピューティングリソース

## デメリット・制約事項

### 制限事項

- キャパシティ予約中は予約したDPU数分の料金が発生
- データスキャン料金モデルと比較して、大量データスキャンには割高になる場合がある

### 考慮すべき点

- ワークロードパターンを分析してコスト効果を評価
- オンデマンド料金との比較を推奨

## ユースケース

### ユースケース 1: バッチ処理ワークロード

**シナリオ**: 毎時のバッチ処理で短時間に集中したクエリを実行

**実装例**:
```bash
# バッチ処理開始時にキャパシティを増加
aws athena update-capacity-reservation \
  --name batch-reservation \
  --target-dpus 24

# バッチ処理終了後にキャパシティを削減
aws athena update-capacity-reservation \
  --name batch-reservation \
  --target-dpus 4
```

**効果**: バッチ処理時のみ高キャパシティを使用し、コストを最適化

### ユースケース 2: 開発・テスト環境

**シナリオ**: 開発チームがクエリのテストと検証を実施

**実装例**:
```bash
# 開発環境用の最小キャパシティ予約
aws athena create-capacity-reservation \
  --name dev-reservation \
  --target-dpus 4
```

**効果**: 最小 4 DPU で開発環境のコストを削減

### ユースケース 3: インタラクティブ分析

**シナリオ**: データアナリストが ad-hoc クエリを実行

**実装例**:
```bash
# 営業時間中は高キャパシティ
aws events put-rule \
  --name "increase-athena-capacity" \
  --schedule-expression "cron(0 9 ? * MON-FRI *)"

# 営業時間外は最小キャパシティ
aws events put-rule \
  --name "decrease-athena-capacity" \
  --schedule-expression "cron(0 18 ? * MON-FRI *)"
```

**効果**: 利用パターンに合わせてキャパシティを自動調整

## 料金

キャパシティ予約では、予約したキャパシティのみに課金され、データスキャン料金は発生しません。

| モデル | 課金対象 | 特徴 |
|--------|---------|------|
| キャパシティ予約 | DPU × 時間 | 予測可能、短時間ワークロード向け |
| オンデマンド | スキャンしたデータ量 | 大量データスキャン向け |

### コスト比較例

| ワークロード | オンデマンド | キャパシティ予約 |
|-------------|-------------|-----------------|
| 短時間バースト | 高コスト | 低コスト (最大 95% 削減) |
| 大量データスキャン | 低コスト | 高コストの可能性 |

## 利用可能リージョン

Amazon Athena が提供されているすべてのリージョンで利用可能です。

## 関連サービス・機能

- **AWS Glue Data Catalog**: Athena のメタデータストア
- **Amazon S3**: クエリ対象のデータストレージ
- **Amazon QuickSight**: BI ダッシュボードとの連携

## 参考リンク

- 📊 [インフォグラフィック](https://takech9203.github.io/aws-news-summary/20260210-amazon-athena-one-minute-capacity-reservations.html)
- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-athena-one-minute-capacity-reservations/)
- [Athena キャパシティ予約ドキュメント](https://docs.aws.amazon.com/athena/latest/ug/capacity-management-creating-capacity-reservations.html)
- [Athena 料金](https://aws.amazon.com/athena/pricing/)

## まとめ

Amazon Athena の 1 分間キャパシティ予約と 4 DPU 最小キャパシティにより、短時間クエリワークロードのコスト最適化が大幅に改善されました。バッチ処理や開発環境など、短時間で集中的なクエリを実行するワークロードでは、この新機能を活用して最大 95% のコスト削減を実現できます。ワークロードパターンを分析し、最適な料金モデルを選択してください。
