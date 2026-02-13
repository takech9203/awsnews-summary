# Amazon Application Recovery Controller - Region Switch 新機能

**リリース日**: 2025 年 12 月 19 日
**サービス**: Amazon Application Recovery Controller (ARC)
**機能**: Region Switch の 3 つの新機能

## 概要

Amazon Application Recovery Controller (ARC) の Region Switch に 3 つの新機能が追加されました。AWS GovCloud (US) サポート、プラン実行レポート、Amazon DocumentDB グローバルクラスター実行ブロックが利用可能になります。

Region Switch は、マルチリージョンアプリケーションを別の AWS リージョンで運用するための具体的なステップをオーケストレーションし、リージョン障害時に限定された復旧時間を達成できるようにします。フェイルオーバーステップの完了、カスタムダッシュボードの作成、復旧成功の証拠収集に必要だった数時間のエンジニアリング作業と運用オーバーヘッドを排除します。

**アップデート前の課題**

- AWS GovCloud (US) リージョンでは Region Switch が利用できなかった
- 復旧操作の証拠とドキュメントを手動でコンパイルする必要があった
- Amazon DocumentDB グローバルクラスターのフェイルオーバーを手動でオーケストレーションする必要があった

**アップデート後の改善**

- AWS GovCloud (US-East および US-West) リージョンで Region Switch が一般提供
- プラン実行レポートが自動生成され、S3 バケットに保存
- DocumentDB グローバルクラスターの自動フェイルオーバーをサポート

## サービスアップデートの詳細

### 主要機能

1. **AWS GovCloud (US) サポート**
   - AWS GovCloud (US-East) リージョンで一般提供
   - AWS GovCloud (US-West) リージョンで一般提供
   - 政府機関や規制対象ワークロードの DR 要件に対応

2. **プラン実行レポート**
   - 各プラン実行から包括的なレポートを自動生成
   - 指定した Amazon S3 バケットにレポートを保存
   - 復旧操作の詳細なタイムライン
   - Region Switch 対象リソースの一覧
   - オプションのアプリケーションステータスアラームの状態
   - 復旧時間目標 (RTO) の計算

3. **DocumentDB グローバルクラスター実行ブロック**
   - 9 つの実行ブロックカタログに追加
   - Amazon DocumentDB グローバルクラスターのフェイルオーバー操作をオーケストレーション
   - スイッチオーバー操作のサポート
   - マルチリージョンデータベース復旧の自動化

## 技術仕様

### 実行ブロックカタログ

| 実行ブロック | 説明 |
|-------------|------|
| DocumentDB グローバルクラスター | 新規追加 - フェイルオーバー/スイッチオーバー |
| その他 9 つの既存ブロック | 既存のサービス統合 |

### プラン実行レポートの内容

| 項目 | 説明 |
|------|------|
| タイムライン | 復旧操作の詳細なイベント履歴 |
| リソース一覧 | Region Switch 対象リソース |
| アラーム状態 | アプリケーションステータスアラームの状態 |
| RTO 計算 | 復旧時間目標の実績値 |

### API 変更履歴

| 日付 | サービス | 変更内容 |
|------|----------|----------|
| 2025/12/19 | [ARC - Region switch](https://awsapichanges.com/archive/changes/970b99-arc-region-switch.html) | 6 updated api methods - プラン実行レポートの自動生成機能を追加 |

## 設定方法

### 前提条件

1. AWS アカウント
2. マルチリージョンアプリケーションの構成
3. 適切な IAM 権限
4. (オプション) Amazon S3 バケット (レポート保存用)

### 手順

#### ステップ 1: Region Switch プランの作成

```bash
aws arc-region-switch create-plan \
  --plan-name "my-dr-plan" \
  --source-region us-east-1 \
  --target-region us-west-2
```

Region Switch プランを作成します。ソースリージョンとターゲットリージョンを指定します。

#### ステップ 2: DocumentDB 実行ブロックの追加

```bash
aws arc-region-switch add-execution-block \
  --plan-name "my-dr-plan" \
  --block-type "DocumentDBGlobalCluster" \
  --global-cluster-identifier "my-global-cluster"
```

DocumentDB グローバルクラスターの実行ブロックをプランに追加します。

#### ステップ 3: レポート出力先の設定

```bash
aws arc-region-switch update-plan \
  --plan-name "my-dr-plan" \
  --report-configuration '{
    "s3BucketName": "my-dr-reports-bucket",
    "s3KeyPrefix": "region-switch-reports/"
  }'
```

プラン実行レポートの出力先 S3 バケットを設定します。

## メリット

### ビジネス面

- **コンプライアンス対応**: 自動生成レポートにより監査対応が容易に
- **運用効率化**: 手動でのドキュメント作成作業を排除
- **政府機関対応**: GovCloud サポートにより規制対象ワークロードに対応

### 技術面

- **自動化**: DocumentDB フェイルオーバーの自動オーケストレーション
- **可視性**: 詳細なタイムラインと RTO 計算
- **統合**: 既存の Region Switch プランに簡単に追加可能

## デメリット・制約事項

### 制限事項

- DocumentDB グローバルクラスターの設定が必要
- S3 バケットへのアクセス権限が必要
- 一部のリージョンでは利用できない場合がある

### 考慮すべき点

- レポートの保存コスト (S3 ストレージ)
- DocumentDB グローバルクラスターのフェイルオーバー時間
- 定期的な DR テストの実施を推奨

## ユースケース

### ユースケース 1: 政府機関の DR 対応

**シナリオ**: 政府機関のワークロードで GovCloud リージョン間の DR を実装

**実装例**:
```bash
aws arc-region-switch create-plan \
  --plan-name "gov-dr-plan" \
  --source-region us-gov-west-1 \
  --target-region us-gov-east-1
```

**効果**: GovCloud リージョンでの自動化された DR オーケストレーション

### ユースケース 2: コンプライアンス監査対応

**シナリオ**: DR テストの証拠を監査人に提出

**実装例**:
```bash
# プラン実行後、S3 からレポートを取得
aws s3 cp s3://my-dr-reports-bucket/region-switch-reports/latest-report.json ./
```

**効果**: 自動生成レポートにより監査対応の工数を削減

### ユースケース 3: DocumentDB マルチリージョン復旧

**シナリオ**: DocumentDB グローバルクラスターを含むアプリケーションの DR

**実装例**:
```json
{
  "executionBlocks": [
    {
      "type": "DocumentDBGlobalCluster",
      "globalClusterIdentifier": "my-global-cluster",
      "targetRegion": "us-west-2"
    }
  ]
}
```

**効果**: データベースフェイルオーバーを含む統合的な DR オーケストレーション

## 料金

Amazon Application Recovery Controller の料金は、Region Switch プランの実行回数に基づきます。

### 料金例

| 項目 | 料金 |
|------|------|
| Region Switch プラン実行 | 実行あたりの料金 |
| S3 ストレージ (レポート) | S3 標準料金 |

詳細な料金については、[Amazon Application Recovery Controller 料金ページ](https://aws.amazon.com/application-recovery-controller/pricing/)を参照してください。

## 利用可能リージョン

AWS GovCloud (US-East)、AWS GovCloud (US-West) を含む、ARC Region Switch が利用可能なリージョンで利用できます。詳細は AWS Regional Services List を参照してください。

## 関連サービス・機能

- **Amazon Application Recovery Controller**: アプリケーション復旧オーケストレーション
- **Amazon DocumentDB**: MongoDB 互換ドキュメントデータベース
- **Amazon Route 53**: DNS とヘルスチェック
- **Amazon S3**: レポート保存

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/12/amazon-application-recovery-controller-region-switch-new-capabilities/)
- [Amazon Application Recovery Controller](https://aws.amazon.com/application-recovery-controller/)
- [Region Switch ドキュメント](https://docs.aws.amazon.com/r53recovery/latest/dg/region-switch.html)

## まとめ

Amazon Application Recovery Controller の Region Switch に 3 つの新機能が追加されました。AWS GovCloud (US) サポートにより政府機関や規制対象ワークロードの DR 要件に対応し、プラン実行レポートの自動生成によりコンプライアンス監査対応が容易になりました。DocumentDB グローバルクラスター実行ブロックにより、データベースを含む統合的な DR オーケストレーションが可能になります。
