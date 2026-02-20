# Amazon RDS for Oracle - 2026 年 1 月 Release Update および Spatial Patch Bundle

**リリース日**: 2026 年 2 月 20 日
**サービス**: Amazon Relational Database Service (Amazon RDS) for Oracle
**機能**: Oracle January 2026 Release Update (RU) および Spatial Patch Bundle (SPB)

📊 [このアップデートのインフォグラフィックを見る](https://takech9203.github.io/aws-news-summary/20260220-amazon-rd-for-oracle-jan-release-update-spatial-patch-bundle.html)

## 概要

Amazon RDS for Oracle が Oracle January 2026 Release Update (RU) をサポートした。対象は Oracle Database バージョン 19c および 21c で、対応する Spatial Patch Bundle (SPB) も Oracle Database 19c 向けに提供されている。Oracle の四半期セキュリティパッチに対応するこの RU には、Oracle データベース製品のセキュリティ更新が含まれており、アップグレードが推奨される。

Spatial Patch Bundle は Oracle Spatial and Graph 機能の重要な修正を含み、空間操作の信頼性と最適なパフォーマンスを提供する。RDS マネジメントコンソール、AWS SDK、CLI からアップグレードを適用でき、Automatic Minor Version Upgrade を有効にすることでメンテナンスウィンドウ中の自動適用も可能。

**アップデート前の課題**

- 前四半期の RU (2025 年 10 月) が最新であり、2026 年 1 月に公開されたセキュリティ修正を適用できなかった
- Oracle Spatial and Graph 機能を使用するユーザーは、最新の Spatial パッチを適用できなかった
- セキュリティ脆弱性への対応が遅れるリスクがあった

**アップデート後の改善**

- 2026 年 1 月のセキュリティ修正を含む最新 RU を RDS 上で適用可能になった
- Spatial Patch Bundle により Oracle Spatial and Graph 機能の信頼性とパフォーマンスが向上した
- AWS Organizations upgrade rollout policy を活用して、非本番環境から段階的にアップグレードを展開可能

## サービスアップデートの詳細

### 主要機能

1. **January 2026 Release Update (RU)**
   - Oracle Database 19c および 21c に対応
   - セキュリティ修正、バグ修正を含む四半期パッチ
   - エンジンバージョン: `19.0.0.0.ru-2026-01.rur-2026-01.r1` (19c の場合)

2. **Spatial Patch Bundle (SPB)**
   - Oracle Database 19c のみ対応
   - RU のすべてのパッチに加え、Oracle Spatial 固有のパッチを含む
   - エンジンバージョン: `19.0.0.0.ru-2026-01.spb-1.r1`
   - Spatial and Graph 機能の信頼性と空間操作パフォーマンスを改善

3. **AWS Organizations upgrade rollout policy 対応**
   - 複数の AWS アカウントおよびデータベースリソース全体でアップグレードを管理
   - 非本番環境で先に適用し、検証後に本番環境に展開する段階的ロールアウトが可能
   - 「First」「Second」「Last」の 3 段階でアップグレード順序を制御

## 技術仕様

### 対応バージョン

| 項目 | 詳細 |
|------|------|
| 対象データベースバージョン | Oracle Database 19c, 21c |
| RU エンジンバージョン (19c) | `19.0.0.0.ru-2026-01.rur-2026-01.r1` |
| SPB エンジンバージョン (19c) | `19.0.0.0.ru-2026-01.spb-1.r1` |
| セキュリティパッチソース | [Oracle January 2026 CPU](https://www.oracle.com/security-alerts/cpujan2026.html#AppendixDB) |

### RU と SPB のアップグレードパス

| 現在のバージョン | アップグレード先 | 可否 |
|---|---|---|
| RU 2025-10 | RU 2026-01 | ✅ 可能 |
| RU 2025-10 | SPB 2026-01 | ✅ 可能 |
| SPB 2025-10 | SPB 2026-01 | ✅ 自動アップグレード対象 |
| SPB 2025-10 | RU 2026-01 | ✅ 可能 (上位バージョンのため) |

### AWS Organizations upgrade rollout policy

| アップグレード順序 | 用途 | 適用タイミング |
|---|---|---|
| First | 開発・テスト環境 | 最初に適用 |
| Second | ステージング・非クリティカル本番 | 「First」完了後 |
| Last | クリティカルな本番環境 | 「Second」完了後 |

## 設定方法

### 前提条件

1. Amazon RDS for Oracle の DB インスタンスが稼働中であること
2. 対象バージョン (Oracle Database 19c または 21c) を使用していること
3. アップグレード前にテスト環境での検証を推奨

### 手順

#### ステップ 1: Release Update の手動適用

```bash
# RU へのアップグレード (19c の場合)
aws rds modify-db-instance \
  --db-instance-identifier mydbinstance \
  --engine-version 19.0.0.0.ru-2026-01.rur-2026-01.r1 \
  --apply-immediately
```

指定した DB インスタンスのエンジンバージョンを January 2026 RU に更新する。`--apply-immediately` を省略するとメンテナンスウィンドウ中に適用される。

#### ステップ 2: Spatial Patch Bundle の適用 (必要な場合)

```bash
# SPB へのアップグレード (19c のみ)
aws rds modify-db-instance \
  --db-instance-identifier mydbinstance \
  --engine-version 19.0.0.0.ru-2026-01.spb-1.r1 \
  --apply-immediately
```

Oracle Spatial 機能を使用している場合、SPB を適用する。AWS マネジメントコンソールでは「Spatial Patch Bundle Engine Versions」チェックボックスを選択して適用する。

#### ステップ 3: 自動アップグレードの設定

```bash
# Automatic Minor Version Upgrade を有効にする
aws rds modify-db-instance \
  --db-instance-identifier mydbinstance \
  --auto-minor-version-upgrade
```

今後のリリースアップデートがメンテナンスウィンドウ中に自動適用されるよう設定する。自動アップグレードパスは現在のバージョン (RU または SPB) に基づいて決定される。

## メリット

### ビジネス面

- **セキュリティ強化**: Oracle が公開した最新のセキュリティ修正を適用でき、データベースの脆弱性リスクを低減
- **運用負荷の軽減**: AWS Organizations upgrade rollout policy により、マルチアカウント環境でのアップグレード管理を自動化
- **ダウンタイム計画の柔軟性**: 手動適用と自動適用を選択でき、ビジネス要件に応じたアップグレードスケジュールを設定可能

### 技術面

- **Spatial 機能の安定性向上**: SPB により Oracle Spatial and Graph の空間操作パフォーマンスが最適化
- **段階的ロールアウト**: 非本番環境で先に検証してから本番適用でき、リスクを最小化
- **既存ワークフロー互換**: RDS コンソール、AWS CLI、SDK すべてのインターフェースから適用可能

## デメリット・制約事項

### 制限事項

- アップグレードにはダウンタイムが発生する (Multi-AZ 構成でもプライマリとスタンバイの両方がアップグレードされる)
- SPB は Oracle Database 19c のみ対応 (21c は RU のみ)
- RU から SPB への自動アップグレードは行われない (手動で切り替える必要がある)

### 考慮すべき点

- アップグレード前にアプリケーションの互換性をテスト環境で検証すること
- SPB へ手動で切り替えた場合、以降の自動アップグレードパスが SPB パスに変更される
- Multi-AZ 構成では、OS アップデートがデータベースバージョンアップグレードの前に適用される

## ユースケース

### ユースケース 1: セキュリティパッチの迅速な適用

**シナリオ**: 金融機関で Oracle Database を使用しており、コンプライアンス要件により四半期ごとのセキュリティパッチを速やかに適用する必要がある。

**実装例**:
```bash
# テスト環境を「First」、本番を「Last」に設定して段階的に適用
aws rds modify-db-instance \
  --db-instance-identifier test-db \
  --engine-version 19.0.0.0.ru-2026-01.rur-2026-01.r1 \
  --apply-immediately
```

**効果**: AWS Organizations upgrade rollout policy を活用し、テスト環境で先行検証後に本番適用することで、セキュリティ要件を満たしながらリスクを最小化。

### ユースケース 2: 空間データベースのパフォーマンス改善

**シナリオ**: 物流企業で Oracle Spatial を使用して配送ルート最適化を行っており、空間操作のパフォーマンス向上が必要。

**実装例**:
```bash
# SPB を適用して Spatial 機能を最適化
aws rds modify-db-instance \
  --db-instance-identifier logistics-db \
  --engine-version 19.0.0.0.ru-2026-01.spb-1.r1 \
  --apply-immediately
```

**効果**: Spatial Patch Bundle の適用により、空間クエリのパフォーマンスと信頼性が向上し、配送ルート計算の精度と速度が改善。

### ユースケース 3: マルチアカウント環境での統一的なパッチ管理

**シナリオ**: 大規模エンタープライズで複数の AWS アカウントにまたがる Oracle DB インスタンスのパッチ管理を効率化したい。

**実装例**:
```bash
# AWS Organizations upgrade rollout policy を活用
# 開発: First、ステージング: Second、本番: Last
# Automatic Minor Version Upgrade を有効化
aws rds modify-db-instance \
  --db-instance-identifier prod-oracle-db \
  --auto-minor-version-upgrade
```

**効果**: 開発環境での検証を経て段階的に本番へ展開することで、カスタムツールなしにマルチアカウント環境全体のパッチ適用を統一管理。

## 料金

Amazon RDS for Oracle の料金はインスタンスタイプ、リージョン、ライセンスモデル (License Included または Bring Your Own License) に基づく。RU および SPB の適用自体に追加料金は発生しない。

## 利用可能リージョン

Amazon RDS for Oracle が利用可能なすべての AWS リージョンで January 2026 RU および SPB を適用可能。AWS GovCloud (US) リージョンを含む。

## 関連サービス・機能

- **AWS Organizations**: upgrade rollout policy でマルチアカウントのアップグレード管理
- **Amazon RDS Multi-AZ**: 高可用性構成でのアップグレード適用
- **Amazon RDS Performance Insights**: アップグレード前後のパフォーマンス比較に活用
- **AWS Systems Manager Maintenance Windows**: メンテナンスウィンドウの管理と通知設定

## 参考リンク

- 📊 [インフォグラフィック](https://takech9203.github.io/aws-news-summary/20260220-amazon-rd-for-oracle-jan-release-update-spatial-patch-bundle.html)
- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-rd-for-oracle-jan-release-update-spatial-patch-bundle/)
- [Oracle minor version upgrades ドキュメント](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.Oracle.Minor.html)
- [Oracle January 2026 CPU](https://www.oracle.com/security-alerts/cpujan2026.html#AppendixDB)
- [Amazon RDS for Oracle 製品ページ](https://aws.amazon.com/rds/oracle/)
- [Amazon RDS 料金ページ](https://aws.amazon.com/rds/oracle/pricing/)

## まとめ

Amazon RDS for Oracle の January 2026 Release Update は、最新のセキュリティ修正を含む四半期パッチであり、Oracle Database 19c および 21c を使用するすべてのユーザーにアップグレードが推奨される。特に Oracle Spatial 機能を使用している場合は SPB の適用を検討し、AWS Organizations upgrade rollout policy を活用して段階的なロールアウトを計画することを推奨する。
