# AWS Backup - Amazon Neptune のサポートを 5 つの追加リージョンに拡大

**リリース日**: 2026 年 2 月 25 日
**サービス**: AWS Backup
**機能**: Amazon Neptune の 5 つの追加リージョンでのサポート

📊 [このアップデートのインフォグラフィックを見る](https://takech9203.github.io/aws-news-summary/20260225-aws-backup-extends-amazon-neptune-five-additional-regions.html)

## 概要

AWS Backup が Amazon Neptune のサポートを 5 つの追加 AWS リージョンに拡大した。新たに対応したリージョンは、ヨーロッパ (チューリッヒ)、アジアパシフィック (メルボルン)、カナダ西部 (カルガリー)、アジアパシフィック (マレーシア)、ヨーロッパ (スペイン) である。

この拡張により、これらのリージョンで Amazon Neptune クラスターに対するポリシーベースのデータ保護とリカバリが利用可能になった。AWS Backup は、フルマネージドかつコスト効率の高いソリューションであり、Amazon Neptune を含む AWS サービス全体のデータ保護を一元化・自動化する。

**アップデート前の課題**

- これら 5 つのリージョンでは AWS Backup による Neptune クラスターの保護ができなかった
- 該当リージョンのユーザーは Neptune データの一元的なバックアップ管理を利用できず、個別の対応が必要だった
- データレジデンシー要件がある地域では、他リージョンへのバックアップに制約があった

**アップデート後の改善**

- 5 つの追加リージョンで Neptune クラスターのポリシーベースバックアップが可能に
- 既存のバックアッププランに Neptune クラスターを追加するだけで保護を開始可能
- データレジデンシー要件に準拠したローカルリージョンでのバックアップ管理を実現

## サービスアップデートの詳細

### 新規対応リージョン

| リージョン | リージョンコード |
|------|------|
| ヨーロッパ (チューリッヒ) | eu-central-2 |
| アジアパシフィック (メルボルン) | ap-southeast-4 |
| カナダ西部 (カルガリー) | ca-west-1 |
| アジアパシフィック (マレーシア) | ap-southeast-5 |
| ヨーロッパ (スペイン) | eu-south-2 |

### AWS Backup for Neptune の主要機能

1. **ポリシーベースのバックアップ**
   - バックアッププランを作成し、Neptune クラスターに適用
   - スケジュールに基づく自動バックアップの実行
   - ライフサイクルポリシーによるバックアップの保持期間管理

2. **一元的なバックアップ管理**
   - AWS Backup コンソールから Neptune を含む複数サービスのバックアップを一元管理
   - バックアップアクティビティの監視とレポート
   - AWS Backup Audit Manager によるコンプライアンス管理

3. **クロスリージョン・クロスアカウントバックアップ**
   - リージョン間でのバックアップコピー
   - AWS Organizations と連携したマルチアカウントバックアップ管理

## 設定方法

### 前提条件

1. AWS アカウント
2. 対象リージョンで稼働中の Amazon Neptune クラスター
3. AWS Backup サービスの有効化
4. 適切な IAM 権限

### 手順

#### ステップ 1: バックアッププランの作成

```bash
aws backup create-backup-plan \
  --backup-plan '{
    "BackupPlanName": "NeptuneBackupPlan",
    "Rules": [
      {
        "RuleName": "DailyBackup",
        "TargetBackupVaultName": "Default",
        "ScheduleExpression": "cron(0 5 ? * * *)",
        "Lifecycle": {
          "DeleteAfterDays": 35
        }
      }
    ]
  }' \
  --region eu-central-2
```

新規対応リージョンで Neptune クラスター用のバックアッププランを作成する。

#### ステップ 2: Neptune クラスターをバックアッププランに割り当て

```bash
aws backup create-backup-selection \
  --backup-plan-id <backup-plan-id> \
  --backup-selection '{
    "SelectionName": "NeptuneSelection",
    "IamRoleArn": "arn:aws:iam::123456789012:role/AWSBackupDefaultServiceRole",
    "Resources": [
      "arn:aws:neptune:eu-central-2:123456789012:cluster:my-neptune-cluster"
    ]
  }' \
  --region eu-central-2
```

Neptune クラスターの ARN を指定してバックアッププランに割り当てる。

#### ステップ 3: バックアップの確認

```bash
aws backup list-backup-jobs \
  --by-resource-type Neptune \
  --region eu-central-2
```

バックアップジョブのステータスを確認する。

## メリット

### ビジネス面

- **データレジデンシー対応**: ヨーロッパ、カナダ、アジアパシフィックの新リージョンでデータ保護規制に準拠したバックアップが可能
- **運用効率の向上**: 既存のバックアッププランに Neptune クラスターを追加するだけで保護を開始
- **コンプライアンス強化**: AWS Backup Audit Manager と組み合わせたバックアップポリシーの一元管理

### 技術面

- **ポリシーベースの自動化**: スケジュールに基づく自動バックアップにより手動作業を排除
- **一元管理**: AWS Backup コンソールから Neptune を含む複数サービスのバックアップを統合管理
- **災害復旧**: クロスリージョンコピーによるリージョン障害への備え

## デメリット・制約事項

### 制限事項

- Neptune のバックアップはクラスター単位で取得され、個別のグラフ単位でのバックアップには対応していない
- クロスリージョンコピーはサポート対象リージョン間でのみ利用可能
- 一部の高度な機能 (クロスアカウントコピーなど) はオプトインリージョンで制限される場合がある

### 考慮すべき点

- クロスリージョンデータ転送にはデータ転送料金が発生する
- バックアップの保持期間とストレージコストのバランスを検討する必要がある

## ユースケース

### ユースケース 1: ヨーロッパのデータ保護規制への準拠

**シナリオ**: スイスまたはスペインで Neptune グラフデータベースを運用しており、データを EU 内に保持する必要がある。

**効果**: チューリッヒまたはスペインリージョンで Neptune クラスターのバックアップをローカルに管理でき、データレジデンシー要件に準拠。

### ユースケース 2: アジアパシフィックでのグラフデータベース保護

**シナリオ**: メルボルンまたはマレーシアリージョンで Neptune を使用したナレッジグラフや推奨エンジンを運用しており、定期的なバックアップが必要。

**効果**: ローカルリージョンでポリシーベースの自動バックアップを設定し、グラフデータの保護を確立。

### ユースケース 3: カナダ西部でのディザスタリカバリ

**シナリオ**: カルガリーリージョンで Neptune クラスターを運用し、ビジネス継続性のためにバックアップ戦略を構築したい。

**効果**: AWS Backup のバックアッププランにより自動化されたバックアップを設定し、障害時の迅速な復旧を実現。

## 料金

AWS Backup for Neptune の料金は、バックアップストレージの使用量に基づく従量課金制である。

| 項目 | 詳細 |
|------|------|
| バックアップストレージ | 使用量に応じた料金 |
| クロスリージョンデータ転送 | リージョン間のデータ転送料金 |
| リストア | リストアされたデータ量に基づく料金 |

料金はリージョンにより異なる。詳細は [AWS Backup 料金ページ](https://aws.amazon.com/backup/pricing/) を参照。

## 利用可能リージョン

今回の追加により、AWS Backup for Neptune は以下を含む多数のリージョンで利用可能になった。

**今回追加**: ヨーロッパ (チューリッヒ)、アジアパシフィック (メルボルン)、カナダ西部 (カルガリー)、アジアパシフィック (マレーシア)、ヨーロッパ (スペイン)

完全なリージョンリストは [AWS Backup 機能の利用可能状況ページ](https://docs.aws.amazon.com/aws-backup/latest/devguide/backup-feature-availability.html) を参照。

## 関連サービス・機能

- **Amazon Neptune**: 接続されたデータのためのサーバーレスグラフデータベース
- **AWS Backup Audit Manager**: バックアップのコンプライアンス監視とレポート
- **AWS Organizations**: マルチアカウントバックアップ戦略の一元管理

## 参考リンク

- 📊 [インフォグラフィック](https://takech9203.github.io/aws-news-summary/20260225-aws-backup-extends-amazon-neptune-five-additional-regions.html)
- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/aws-backup-extends-amazon-neptune-five-additional-regions/)
- [AWS Backup 製品ページ](https://aws.amazon.com/backup/)
- [AWS Backup 料金ページ](https://aws.amazon.com/backup/pricing/)
- [AWS Backup ドキュメント](https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html)
- [AWS Backup 機能の利用可能状況](https://docs.aws.amazon.com/aws-backup/latest/devguide/backup-feature-availability.html)

## まとめ

AWS Backup が Amazon Neptune のサポートを 5 つの追加リージョン (ヨーロッパのチューリッヒとスペイン、アジアパシフィックのメルボルンとマレーシア、カナダ西部のカルガリー) に拡大した。これにより、これらのリージョンで Neptune クラスターに対するポリシーベースのデータ保護とリカバリが利用可能になった。該当リージョンで Neptune を運用しているユーザーは、既存のバックアッププランに Neptune クラスターを追加するか、新しいバックアッププランを作成して保護を開始することを推奨する。
