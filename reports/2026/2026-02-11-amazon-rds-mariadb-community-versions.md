# Amazon RDS for MariaDB - コミュニティマイナーバージョン 10.6.25、10.11.16、11.4.10、11.8.6 サポート

**リリース日**: 2026年2月11日
**サービス**: Amazon RDS for MariaDB
**機能**: MariaDB マイナーバージョン 10.6.25、10.11.16、11.4.10、11.8.6 のサポート

📊 [このアップデートのインフォグラフィックを見る](https://takech9203.github.io/awsnews-summary/20260211-amazon-rds-mariadb-community-versions.html)

## 概要

Amazon RDS for MariaDB がコミュニティ MariaDB マイナーバージョン 10.6.25、10.11.16、11.4.10、11.8.6 をサポートしました。これらの最新マイナーバージョンには、セキュリティ脆弱性の修正、バグ修正、パフォーマンス改善、MariaDB コミュニティによる新機能が含まれています。

AWS は、既知のセキュリティ脆弱性への対応とコミュニティが追加したバグ修正の恩恵を受けるために、最新のマイナーバージョンへのアップグレードを推奨しています。

**アップデート前の課題**

- 以前のマイナーバージョンに既知のセキュリティ脆弱性が存在
- 最新のバグ修正やパフォーマンス改善が適用されていない状態

**アップデート後の改善**

- 4 つの MariaDB メジャーバージョン系列 (10.6、10.11、11.4、11.8) の最新マイナーバージョンが利用可能に
- 自動マイナーバージョンアップグレード機能で計画的なアップグレードが可能
- Blue/Green デプロイメントによる安全なアップグレードパスを提供

## サービスアップデートの詳細

### 主要機能

1. **4 系列のマイナーバージョンサポート**
   - MariaDB 10.6.25: 長期サポート (LTS) 系列の最新版
   - MariaDB 10.11.16: LTS 系列の最新版
   - MariaDB 11.4.10: 中期リリース系列の最新版
   - MariaDB 11.8.6: 最新リリース系列

2. **自動マイナーバージョンアップグレード**
   - スケジュールされたメンテナンスウィンドウ中に自動アップグレードが可能
   - 運用負荷を最小限に抑えながら最新のセキュリティパッチを適用

3. **Blue/Green デプロイメント対応**
   - Amazon RDS Managed Blue/Green デプロイメントによる安全なアップグレード
   - ダウンタイムを最小限に抑えたバージョン更新が可能

## 設定方法

### 前提条件

1. Amazon RDS for MariaDB インスタンスが稼働中であること
2. 適切な IAM 権限

### 手順

#### ステップ 1: 自動マイナーバージョンアップグレードの有効化

```bash
aws rds modify-db-instance \
  --db-instance-identifier my-mariadb-instance \
  --auto-minor-version-upgrade \
  --apply-immediately
```

自動マイナーバージョンアップグレードを有効化し、次のメンテナンスウィンドウで最新のマイナーバージョンに自動更新されるようにします。

#### ステップ 2: 手動アップグレード (即時適用)

```bash
aws rds modify-db-instance \
  --db-instance-identifier my-mariadb-instance \
  --engine-version 11.4.10 \
  --apply-immediately
```

特定のマイナーバージョンに即時アップグレードする場合は、`--engine-version` でターゲットバージョンを指定します。

#### ステップ 3: Blue/Green デプロイメントによるアップグレード

```bash
aws rds create-blue-green-deployment \
  --blue-green-deployment-name my-bg-deployment \
  --source "arn:aws:rds:ap-northeast-1:123456789012:db:my-mariadb-instance" \
  --target-engine-version 11.4.10
```

Blue/Green デプロイメントを使用することで、ダウンタイムを最小限に抑えながらバージョンアップグレードを実施します。

## メリット

### ビジネス面

- **セキュリティリスクの低減**: 既知の脆弱性に対するパッチが適用される
- **安定性の向上**: コミュニティによるバグ修正が含まれる

### 技術面

- **複数のアップグレードパス**: 自動アップグレード、手動アップグレード、Blue/Green デプロイメントから選択可能
- **パフォーマンス改善**: コミュニティが追加したパフォーマンス最適化の恩恵

## デメリット・制約事項

### 制限事項

- メジャーバージョンアップグレードは別途対応が必要 (本アップデートはマイナーバージョンのみ)
- 自動マイナーバージョンアップグレードはメンテナンスウィンドウ中に実施されるため、短時間のダウンタイムが発生

### 考慮すべき点

- アップグレード前にステージング環境でのテストを推奨
- カスタムパラメータグループを使用している場合、互換性の確認が必要

## 料金

マイナーバージョンアップグレード自体に追加料金は発生しません。Amazon RDS for MariaDB の標準料金が適用されます。

## 利用可能リージョン

Amazon RDS for MariaDB が提供されているすべての AWS リージョンで利用可能です。

## 関連サービス・機能

- **Amazon RDS Blue/Green デプロイメント**: ダウンタイムを最小限に抑えたデータベースアップグレード
- **Amazon RDS for PostgreSQL マイナーバージョンサポート**: 同日リリースの PostgreSQL 18.2、17.8、16.12、15.16、14.21 サポート
- **AWS Organizations Upgrade Rollout Policy**: 大規模環境でのフェーズドアップグレード管理

## 参考リンク

- 📊 [インフォグラフィック](https://takech9203.github.io/awsnews-summary/20260211-amazon-rds-mariadb-community-versions.html)
- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-rds-mariadb-community-versions/)
- [Amazon RDS for MariaDB アップグレードガイド](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.MariaDB.html)
- [Amazon RDS for MariaDB 料金](https://aws.amazon.com/rds/mariadb/pricing/)

## まとめ

Amazon RDS for MariaDB の最新マイナーバージョンサポートにより、セキュリティ脆弱性の修正とパフォーマンス改善が適用可能になりました。自動マイナーバージョンアップグレードの有効化、または Blue/Green デプロイメントを活用した計画的なアップグレードを推奨します。
