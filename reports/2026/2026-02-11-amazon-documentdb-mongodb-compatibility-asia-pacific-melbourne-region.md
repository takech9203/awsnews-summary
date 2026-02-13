# Amazon DocumentDB - アジアパシフィック (メルボルン) およびヨーロッパ (チューリッヒ) リージョンで利用可能に

**リリース日**: 2026年2月11日
**サービス**: Amazon DocumentDB (with MongoDB compatibility)
**機能**: 新リージョン対応 - アジアパシフィック (メルボルン)、ヨーロッパ (チューリッヒ)

## 概要

Amazon DocumentDB (with MongoDB compatibility) が、アジアパシフィック (メルボルン) リージョンおよびヨーロッパ (チューリッヒ) リージョンで新たに利用可能になった。これにより、オーストラリアおよびスイスのユーザーは、より低レイテンシーで DocumentDB を利用できるようになる。

Amazon DocumentDB は、フルマネージドのネイティブ JSON データベースであり、MongoDB 互換のワークロードをインフラストラクチャ管理なしで実行できる。ストレージは最大 128 TiB まで自動スケーリングし、毎秒数百万リクエストの処理に対応する。

**アップデート前の課題**

- メルボルンやチューリッヒに近い地域のユーザーは、他のリージョンの DocumentDB を利用する必要があり、レイテンシーが高かった
- データレジデンシー要件 (特にオーストラリアやスイスの規制) に対応するために、DocumentDB を選択肢に含められないケースがあった
- スイスの金融機関やオーストラリアの企業にとって、データの地理的配置が課題となっていた

**アップデート後の改善**

- アジアパシフィック (メルボルン) リージョンでの DocumentDB 利用が可能になり、オーストラリア国内でのデータ保持が実現
- ヨーロッパ (チューリッヒ) リージョンでの DocumentDB 利用が可能になり、スイス国内でのデータレジデンシー要件に対応可能
- 既存の DocumentDB 機能 (自動スケーリング、AWS サービスとの統合、リードレプリカ) がすべて新リージョンで利用可能

## サービスアップデートの詳細

### 主要機能

1. **フルマネージド MongoDB 互換データベース**
   - インフラストラクチャ管理不要で MongoDB ワークロードを実行
   - ストレージは最大 128 TiB まで自動スケーリング
   - アプリケーションへの影響なし

2. **高可用性とスケーラビリティ**
   - 毎秒数百万リクエストの処理に対応
   - 最大 15 の低レイテンシーリードレプリカを数分で追加可能
   - アプリケーションのダウンタイムなし

3. **AWS サービスとのネイティブ統合**
   - AWS Database Migration Service (DMS) によるデータ移行
   - Amazon CloudWatch によるモニタリング
   - AWS CloudTrail による監査ログ
   - AWS Lambda によるイベント駆動処理
   - AWS Backup によるバックアップ管理

## 技術仕様

### 新規対応リージョン

| リージョン | リージョンコード | 発表日 |
|------|------|------|
| アジアパシフィック (メルボルン) | ap-southeast-4 | 2026年2月11日 |
| ヨーロッパ (チューリッヒ) | eu-central-2 | 2026年2月11日 |

### 利用可能な機能

| 機能 | 対応状況 |
|------|------|
| インスタンスベースクラスター | ✅ 対応 |
| エラスティッククラスター | 各リージョンの対応状況を確認 |
| グローバルクラスター | 各リージョンの対応状況を確認 |
| 自動スケーリング (ストレージ) | ✅ 最大 128 TiB |
| リードレプリカ | ✅ 最大 15 |
| DMS 統合 | ✅ 対応 |

## 設定方法

### 前提条件

1. AWS アカウント
2. 対象リージョンへのアクセス権限
3. VPC およびサブネットの設定

### 手順

#### ステップ 1: コンソールからクラスターを作成

AWS マネジメントコンソールで Amazon DocumentDB にアクセスし、リージョンを `ap-southeast-4` (メルボルン) または `eu-central-2` (チューリッヒ) に切り替えて、新しいクラスターを作成する。

#### ステップ 2: AWS CLI で作成する場合

```bash
# メルボルンリージョンで DocumentDB クラスターを作成
aws docdb create-db-cluster \
  --db-cluster-identifier my-docdb-cluster \
  --engine docdb \
  --master-username masteruser \
  --master-user-password <password> \
  --region ap-southeast-4
```

AWS CLI を使用して、指定リージョンに DocumentDB クラスターを作成する。`--region` パラメータで対象リージョンを指定する。

#### ステップ 3: インスタンスの追加

```bash
# クラスターにインスタンスを追加
aws docdb create-db-instance \
  --db-instance-identifier my-docdb-instance \
  --db-cluster-identifier my-docdb-cluster \
  --db-instance-class db.r6g.large \
  --engine docdb \
  --region ap-southeast-4
```

クラスター作成後、インスタンスを追加してデータベースを利用可能な状態にする。

## メリット

### ビジネス面

- **データレジデンシーの遵守**: オーストラリアおよびスイスの法規制に準拠したデータ配置が可能
- **低レイテンシーアクセス**: 地理的に近いリージョンを利用することで、エンドユーザーのレスポンス時間を改善
- **グローバル展開の加速**: オセアニアおよびヨーロッパ地域での DocumentDB 活用のハードルが低下

### 技術面

- **既存機能の完全サポート**: 自動スケーリング、リードレプリカ、AWS サービス統合がすべて利用可能
- **MongoDB 互換性**: 既存の MongoDB アプリケーションからの移行がスムーズ
- **フルマネージド**: インフラストラクチャ管理が不要で、運用負荷を軽減

## デメリット・制約事項

### 制限事項

- 新リージョンでの料金は既存リージョンと異なる場合がある (リージョン固有の料金を確認すること)
- エラスティッククラスターやグローバルクラスターの対応状況はリージョンによって異なる場合がある
- 一部のインスタンスタイプが新リージョンでは利用できない可能性がある

### 考慮すべき点

- 既存のクラスターを新リージョンに移行する場合は、DMS またはスナップショットコピーの利用を検討
- マルチリージョン構成を検討する場合は、グローバルクラスターの対応状況を確認

## ユースケース

### ユースケース 1: オーストラリアの金融サービス

**シナリオ**: オーストラリアの金融機関が、顧客データをオーストラリア国内に保持しながら、MongoDB 互換のドキュメントデータベースを利用したい。

**実装例**:
```bash
aws docdb create-db-cluster \
  --db-cluster-identifier finserv-cluster \
  --engine docdb \
  --master-username admin \
  --master-user-password <secure-password> \
  --region ap-southeast-4 \
  --storage-encrypted \
  --kms-key-id <kms-key-arn>
```

**効果**: データレジデンシー要件を満たしながら、低レイテンシーでの DocumentDB 利用が可能。

### ユースケース 2: スイスの医療データ管理

**シナリオ**: スイスの医療機関が、患者の医療記録をスイス国内のリージョンで管理したい。

**実装例**:
```bash
aws docdb create-db-cluster \
  --db-cluster-identifier healthcare-cluster \
  --engine docdb \
  --master-username admin \
  --master-user-password <secure-password> \
  --region eu-central-2 \
  --storage-encrypted
```

**効果**: スイスのデータ保護規制に準拠しながら、フルマネージドの DocumentDB を利用可能。

### ユースケース 3: オセアニア地域向け SaaS アプリケーション

**シナリオ**: グローバル SaaS プロバイダーが、オセアニア地域のユーザー向けにレスポンス時間を改善したい。

**実装例**:
```bash
# メルボルンリージョンにリードレプリカを追加
aws docdb create-db-instance \
  --db-instance-identifier saas-replica-mel \
  --db-cluster-identifier saas-cluster \
  --db-instance-class db.r6g.xlarge \
  --engine docdb \
  --region ap-southeast-4
```

**効果**: オセアニア地域のユーザーに対して、低レイテンシーの読み取りアクセスを提供。

## 料金

Amazon DocumentDB の料金はリージョンごとに異なる。主な課金要素は以下の通り。

### 料金例

| 項目 | 詳細 |
|------|------|
| オンデマンドインスタンス | インスタンスタイプとリージョンにより異なる |
| I/O 最適化ストレージ | ストレージ使用量に基づく課金 |
| バックアップストレージ | 無料枠を超えた分に課金 |
| データ転送 | リージョン間転送に課金 |

最新の料金情報は [Amazon DocumentDB 料金ページ](https://aws.amazon.com/documentdb/pricing/) を参照。

## 利用可能リージョン

今回の追加により、Amazon DocumentDB は以下を含む多数のリージョンで利用可能。

- **新規追加**: アジアパシフィック (メルボルン)、ヨーロッパ (チューリッヒ)
- 利用可能なリージョンの完全なリストは [AWS リージョン表](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) を参照

## 関連サービス・機能

- **AWS Database Migration Service (DMS)**: 既存の MongoDB から DocumentDB への移行をサポート
- **Amazon CloudWatch**: DocumentDB クラスターのモニタリングとアラート設定
- **AWS Backup**: DocumentDB クラスターの自動バックアップ管理

## 参考リンク

- [公式発表 - メルボルンリージョン (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-documentdb-mongodb-compatibility-asia-pacific-melbourne-region/)
- [公式発表 - チューリッヒリージョン (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-documentdb-mongodb-compatibility-europe-zurich-region/)
- [Amazon DocumentDB ドキュメント](https://docs.aws.amazon.com/documentdb/)
- [料金ページ](https://aws.amazon.com/documentdb/pricing/)
- [AWS リージョン表](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)

## まとめ

Amazon DocumentDB がアジアパシフィック (メルボルン) およびヨーロッパ (チューリッヒ) リージョンで利用可能になったことで、オーストラリアおよびスイスのユーザーは、データレジデンシー要件を満たしながら低レイテンシーで DocumentDB を利用できるようになった。特に金融、医療、政府機関など規制の厳しい業界にとって、地域内でのデータ保持が可能になる点は大きなメリットである。
