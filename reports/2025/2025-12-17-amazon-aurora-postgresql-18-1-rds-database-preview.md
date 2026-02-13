# Amazon Aurora PostgreSQL - PostgreSQL 18.1 プレビュー環境でのサポート

**リリース日**: 2025 年 12 月 17 日
**サービス**: Amazon Aurora PostgreSQL-Compatible Edition
**機能**: PostgreSQL 18.1 のプレビューサポート

## 概要

Amazon Aurora PostgreSQL-Compatible Edition が Amazon RDS Database Preview Environment で PostgreSQL バージョン 18.1 をサポートしました。これにより、Amazon Aurora PostgreSQL で PostgreSQL 18.1 を評価できるようになります。PostgreSQL 18.1 は 2025 年 9 月 9 日に PostgreSQL コミュニティからリリースされました。

PostgreSQL 18.1 には、マルチカラム B-tree インデックスの「スキップスキャン」サポート、OR および IN 条件の WHERE 句処理の改善、並列 GIN インデックスビルド、結合操作の更新が含まれています。また、クエリ実行中のバッファ使用量カウントとインデックスルックアップを表示するオブザーバビリティの改善や、接続ごとの I/O 使用率メトリクスも追加されています。

**アップデート前の課題**

- PostgreSQL 18.1 の新機能を Aurora で評価する手段がなかった
- 新しいデータベースエンジンのテストには自己インストールが必要だった
- 本番環境への影響なしに新機能を試すことが困難だった

**アップデート後の改善**

- RDS Database Preview Environment で PostgreSQL 18.1 を評価可能
- フルマネージドな環境で新機能をテスト
- 本番環境に影響を与えずに互換性を確認

## サービスアップデートの詳細

### PostgreSQL 18.1 の主要な新機能

1. **スキップスキャン (Skip Scan)**
   - マルチカラム B-tree インデックスでのスキップスキャンサポート
   - 特定のクエリパターンでのパフォーマンス向上

2. **WHERE 句の改善**
   - OR 条件の処理改善
   - IN 条件の処理改善
   - クエリ最適化の向上

3. **並列 GIN インデックスビルド**
   - GIN インデックスの並列構築
   - インデックス作成時間の短縮

4. **結合操作の更新**
   - 結合処理の最適化
   - クエリパフォーマンスの向上

5. **オブザーバビリティの改善**
   - クエリ実行中のバッファ使用量カウント
   - インデックスルックアップの表示
   - 接続ごとの I/O 使用率メトリクス

## 技術仕様

### RDS Database Preview Environment

| 項目 | 詳細 |
|------|------|
| 保持期間 | 最大 60 日 |
| 自動削除 | 保持期間後に自動削除 |
| 料金 | US East (Ohio) リージョンの本番 Aurora インスタンスと同等 |
| デプロイメント | Single-AZ および Multi-AZ |

### PostgreSQL 18.1 の主要改善

| 機能 | 説明 |
|------|------|
| スキップスキャン | マルチカラムインデックスの効率的なスキャン |
| 並列 GIN ビルド | GIN インデックスの並列構築 |
| I/O メトリクス | 接続ごとの I/O 使用率監視 |

## 設定方法

### 前提条件

1. AWS アカウントへのアクセス権限
2. Amazon RDS へのアクセス権限
3. RDS Database Preview Environment へのアクセス

### 手順

#### ステップ 1: RDS Database Preview Environment にアクセス

AWS Management Console から RDS Database Preview Environment にアクセスします。

#### ステップ 2: Aurora PostgreSQL クラスターの作成

```bash
# Preview Environment で Aurora PostgreSQL 18.1 クラスターを作成
aws rds create-db-cluster \
    --db-cluster-identifier my-aurora-pg18-preview \
    --engine aurora-postgresql \
    --engine-version 18.1 \
    --master-username admin \
    --master-user-password <password> \
    --db-subnet-group-name my-subnet-group
```

Preview Environment で PostgreSQL 18.1 を使用した Aurora クラスターを作成します。

#### ステップ 3: 新機能のテスト

作成したクラスターに接続し、PostgreSQL 18.1 の新機能をテストします。

## メリット

### ビジネス面

- **リスク軽減**: 本番環境に影響を与えずに新機能を評価
- **計画的なアップグレード**: 互換性を事前に確認
- **コスト効率**: フルマネージド環境でのテスト

### 技術面

- **パフォーマンス評価**: 新しいクエリ最適化の効果を測定
- **互換性テスト**: アプリケーションとの互換性を確認
- **新機能の習熟**: 本番導入前に新機能を学習

## デメリット・制約事項

### 制限事項

- Preview Environment のクラスターは最大 60 日で自動削除
- Preview Environment のスナップショットは Preview Environment 内でのみ使用可能
- 本番ワークロードには使用不可

### 考慮すべき点

- Preview 機能は変更される可能性がある
- 本番環境への移行には新規クラスター作成が必要

## ユースケース

### ユースケース 1: アプリケーション互換性テスト

**シナリオ**: PostgreSQL 18.1 へのアップグレードを計画しており、アプリケーションの互換性を確認したい

**効果**: Preview Environment で実際のクエリを実行し、互換性の問題を事前に特定

### ユースケース 2: パフォーマンス評価

**シナリオ**: PostgreSQL 18.1 の新しいクエリ最適化がワークロードにどの程度効果があるか評価したい

**効果**: スキップスキャンや並列 GIN ビルドなどの新機能のパフォーマンス効果を測定

### ユースケース 3: 新機能の学習

**シナリオ**: 開発チームが PostgreSQL 18.1 の新機能を学習したい

**効果**: 安全な環境で新機能を試し、本番導入に向けた知識を蓄積

## 料金

RDS Database Preview Environment のインスタンスは、US East (Ohio) リージョンで作成された本番 Aurora インスタンスと同じ料金が適用されます。

## 利用可能リージョン

Amazon RDS Database Preview Environment で利用可能です。

## 関連サービス・機能

- **Amazon Aurora**: 高性能・高可用性のリレーショナルデータベース
- **Amazon RDS for PostgreSQL**: PostgreSQL のマネージドサービス
- **AWS Database Migration Service**: データベース移行サービス

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/12/amazon-aurora-postgresql-18-1-rds-database-preview/)
- [PostgreSQL 18 リリースノート](https://www.postgresql.org/about/news/postgresql-18-released-3142/)
- [RDS Database Preview Environment ドキュメント](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/create-db-instance-in-preview-environment.html)
- [Amazon Aurora 入門ガイド](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_GettingStartedAurora.html)

## まとめ

Amazon Aurora PostgreSQL が RDS Database Preview Environment で PostgreSQL 18.1 をサポートしました。スキップスキャン、並列 GIN インデックスビルド、オブザーバビリティの改善など、PostgreSQL 18.1 の新機能を本番環境に影響を与えずに評価できます。PostgreSQL 18.1 へのアップグレードを計画している場合は、Preview Environment を活用して互換性とパフォーマンスを事前に確認することをお勧めします。
