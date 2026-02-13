# Amazon WorkSpaces Applications - 健全性・パフォーマンスメトリクスの追加

**リリース日**: 2025 年 12 月 17 日
**サービス**: Amazon WorkSpaces Applications
**機能**: CloudWatch 健全性・パフォーマンスメトリクス

## 概要

Amazon WorkSpaces Applications に、フリート、セッション、インスタンス、ユーザーの健全性とパフォーマンスを監視するための新しい Amazon CloudWatch メトリクスが追加されました。管理者とサポート運用担当者は、CloudWatch コンソールからフリート全体の監視を簡単に有効化できます。

これらのメトリクスにより、トラブルシューティングが簡素化され、重要なパフォーマンスメトリクスの最新状態が動的に反映されます。

**アップデート前の課題**

- フリートやセッションの健全性を監視するための詳細なメトリクスが不足していた
- エンドユーザーのストリーミングセッションに関する問題のトラブルシューティングが困難だった
- インスタンスサイジングの判断に必要なパフォーマンスデータが限られていた

**アップデート後の改善**

- フリート、セッション、インスタンス、ユーザーレベルの詳細なメトリクスを提供
- パフォーマンスしきい値を設定して、サイジングと予算の最適化が可能
- CloudWatch コンソールから簡単に監視を有効化

## サービスアップデートの詳細

### 主要機能

1. **フリートレベルのメトリクス**
   - フリート全体の健全性状態を監視
   - 利用可能なインスタンス数、使用中のインスタンス数を追跡
   - キャパシティ使用率の可視化

2. **セッションレベルのメトリクス**
   - アクティブセッション数の監視
   - セッション期間とパフォーマンスの追跡
   - セッション関連の問題の特定

3. **インスタンスレベルのメトリクス**
   - 個々のインスタンスのパフォーマンス監視
   - CPU、メモリ、ネットワーク使用率
   - インスタンスの健全性状態

4. **ユーザーレベルのメトリクス**
   - ユーザーごとのストリーミング品質
   - 接続状態とレイテンシー
   - ユーザー体験の可視化

## 技術仕様

### 前提条件

| 要件 | 詳細 |
|------|------|
| エージェントバージョン | 2025 年 12 月 6 日以降にリリースされた最新エージェント |
| マネージドイメージ更新 | 2025 年 12 月 5 日以降にリリースされた更新 |

### 主要メトリクス

| メトリクス | 説明 | ディメンション |
|-----------|------|---------------|
| AvailableCapacity | 利用可能なインスタンス数 | Fleet |
| InUseCapacity | 使用中のインスタンス数 | Fleet |
| ActiveSessions | アクティブセッション数 | Fleet |
| SessionDuration | セッション期間 | Fleet, User |
| CPUUtilization | CPU 使用率 | Instance |
| MemoryUtilization | メモリ使用率 | Instance |

## 設定方法

### 前提条件

1. Amazon WorkSpaces Applications フリート
2. 最新のエージェントを使用したイメージ（2025 年 12 月 6 日以降）
3. CloudWatch へのアクセス権限

### 手順

#### ステップ 1: イメージの更新

最新のエージェントを含むイメージを使用していることを確認します。

**オプション A: 新しいイメージの使用**
- 2025 年 12 月 6 日以降にリリースされた最新エージェントを含むイメージを選択

**オプション B: マネージドイメージ更新の適用**
- 2025 年 12 月 5 日以降にリリースされたマネージドイメージ更新を適用

#### ステップ 2: CloudWatch メトリクスの有効化

1. Amazon CloudWatch コンソールを開く
2. 「メトリクス」→「すべてのメトリクス」を選択
3. 「WorkSpaces Applications」名前空間を選択
4. 監視するメトリクスを選択

#### ステップ 3: アラームの設定

```bash
# CloudWatch アラームの作成例
aws cloudwatch put-metric-alarm \
    --alarm-name "HighCPUUtilization" \
    --metric-name "CPUUtilization" \
    --namespace "AWS/WorkSpacesApplications" \
    --statistic Average \
    --period 300 \
    --threshold 80 \
    --comparison-operator GreaterThanThreshold \
    --evaluation-periods 2 \
    --alarm-actions arn:aws:sns:ap-northeast-1:123456789012:alerts
```

パフォーマンスしきい値を超えた場合に通知を受け取るアラームを設定します。

## メリット

### ビジネス面

- **コスト最適化**: パフォーマンスデータに基づいた適切なインスタンスサイジング
- **ユーザー体験の向上**: 問題の早期検出と迅速な対応
- **運用効率の向上**: トラブルシューティング時間の短縮

### 技術面

- **詳細な可視性**: フリート、セッション、インスタンス、ユーザーレベルのメトリクス
- **プロアクティブな監視**: しきい値ベースのアラートによる問題の早期検出
- **CloudWatch 統合**: 既存の監視インフラストラクチャとの統合

## デメリット・制約事項

### 制限事項

- 最新のエージェント（2025 年 12 月 6 日以降）が必要
- 既存のイメージは更新が必要な場合がある

### 考慮すべき点

- CloudWatch メトリクスの保存には追加コストが発生する可能性
- 詳細なメトリクスを有効にすると、データ量が増加

## ユースケース

### ユースケース 1: キャパシティプランニング

**シナリオ**: フリートのキャパシティ使用率を監視し、適切なサイジングを決定

**実装例**:
```sql
-- CloudWatch Logs Insights クエリ
SELECT AVG(InUseCapacity / AvailableCapacity * 100) as UtilizationPercent
FROM WorkSpacesApplications
WHERE Fleet = 'production-fleet'
GROUP BY bin(1h)
```

**効果**: 過剰プロビジョニングを避け、コストを最適化

### ユースケース 2: ユーザー体験の監視

**シナリオ**: エンドユーザーのストリーミング品質を監視し、問題を早期に検出

**実装例**:
- セッションレイテンシーのしきい値アラームを設定
- ユーザーごとの接続品質を追跡
- 問題発生時に自動通知

**効果**: ユーザーからの報告前に問題を検出し、対応

### ユースケース 3: トラブルシューティング

**シナリオ**: 特定のユーザーのセッション問題を調査

**実装例**:
1. CloudWatch メトリクスでユーザーのセッションを特定
2. インスタンスレベルのメトリクスを確認
3. CPU、メモリ、ネットワーク使用率を分析
4. 問題の根本原因を特定

**効果**: トラブルシューティング時間を大幅に短縮

## 料金

CloudWatch メトリクスの標準料金が適用されます。

| 項目 | 料金 |
|------|------|
| カスタムメトリクス | メトリクスあたり月額料金 |
| アラーム | アラームあたり月額料金 |
| ダッシュボード | ダッシュボードあたり月額料金 |

## 利用可能リージョン

Amazon WorkSpaces Applications が利用可能なすべての AWS 商用リージョンおよび AWS GovCloud (US) リージョンで利用可能です。

## 関連サービス・機能

- **Amazon CloudWatch**: メトリクスの収集と可視化
- **Amazon CloudWatch Alarms**: しきい値ベースのアラート
- **Amazon SNS**: 通知の配信

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/12/amazon-workspaces-applications-health-performance-metrics/)
- [Amazon WorkSpaces Applications メトリクスとディメンション](https://docs.aws.amazon.com/appstream2/latest/developerguide/monitoring-with-cloudwatch.html)
- [ベースイメージエージェント](https://docs.aws.amazon.com/appstream2/latest/developerguide/base-images-agent.html)
- [マネージドイメージ更新](https://docs.aws.amazon.com/appstream2/latest/developerguide/administer-images.html#keep-image-updated-managed-image-updates)

## まとめ

Amazon WorkSpaces Applications の新しい CloudWatch メトリクスにより、フリート、セッション、インスタンス、ユーザーレベルの詳細な監視が可能になりました。これらのメトリクスを活用することで、キャパシティプランニングの最適化、ユーザー体験の向上、トラブルシューティングの効率化を実現できます。最新のエージェントを使用したイメージに更新することで、すぐに利用を開始できます。
