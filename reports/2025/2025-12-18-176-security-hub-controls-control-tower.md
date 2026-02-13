# AWS Control Tower - 176 の新しい Security Hub コントロールを追加

**リリース日**: 2025 年 12 月 18 日
**サービス**: AWS Control Tower
**機能**: Control Catalog への 176 の Security Hub コントロール追加

## 概要

AWS Control Tower が Control Catalog に 176 の新しい AWS Security Hub コントロールを追加しました。これにより、セキュリティ、コスト、耐久性、運用などの様々なユースケースに対応するコントロールを AWS Control Tower から直接検索、発見、有効化、管理できるようになりました。

AWS Control Tower は、マルチアカウント AWS 環境のセットアップとガバナンスを簡素化するサービスです。今回のアップデートにより、Security Hub の豊富なセキュリティコントロールを Control Tower の統合管理フレームワーク内で活用できるようになり、組織全体のセキュリティポスチャをより包括的に管理できます。

**アップデート前の課題**

- Security Hub コントロールと Control Tower コントロールを別々に管理する必要があった
- マルチアカウント環境での Security Hub コントロールの一括有効化が複雑だった
- セキュリティ以外のユースケース（コスト、運用など）のコントロールが限定的だった

**アップデート後の改善**

- 176 の追加コントロールを Control Tower から直接管理可能
- セキュリティ、コスト、耐久性、運用の各ユースケースをカバー
- API を使用したプログラマティックな管理が可能

## サービスアップデートの詳細

### 主要機能

1. **統合コントロール管理**
   - Control Catalog から Security Hub コントロールを検索・発見
   - Control Tower コンソールから直接有効化
   - 組織全体への一括適用

2. **多様なユースケース対応**
   - セキュリティコントロール
   - コスト最適化コントロール
   - 耐久性コントロール
   - 運用コントロール

3. **API サポート**
   - ListControls API でコントロール一覧を取得
   - GetControl API でコントロール詳細を取得
   - EnableControl API でコントロールを有効化

## 技術仕様

### コントロールカテゴリ

| カテゴリ | 説明 |
|---------|------|
| セキュリティ | セキュリティベストプラクティスの遵守 |
| コスト | コスト最適化と無駄の削減 |
| 耐久性 | データ保護と可用性 |
| 運用 | 運用効率とベストプラクティス |

### API 操作

| API | 説明 |
|-----|------|
| ListControls | 利用可能なコントロールの一覧取得 |
| GetControl | 特定のコントロールの詳細取得 |
| EnableControl | コントロールの有効化 |

## 設定方法

### 前提条件

1. AWS Control Tower が有効化されていること
2. AWS Organizations の管理アカウントへのアクセス権限
3. Control Tower 管理者権限

### 手順

#### ステップ 1: Control Catalog へのアクセス

AWS Control Tower コンソールにアクセスし、Control Catalog を開きます。

#### ステップ 2: Security Hub コントロールのフィルタリング

```
Control owner フィルターを「AWS Security Hub」に設定
```

Control Catalog で「Control owner」フィルターを「AWS Security Hub」に設定すると、利用可能な Security Hub コントロールが表示されます。

#### ステップ 3: コントロールの有効化（コンソール）

必要なコントロールを選択し、「Enable」ボタンをクリックして有効化します。

#### ステップ 4: コントロールの有効化（API）

```bash
# 利用可能なコントロールを一覧表示
aws controltower list-controls \
    --filter "controlOwner=AWS Security Hub"

# 特定のコントロールの詳細を取得
aws controltower get-control \
    --control-identifier "arn:aws:controltower:us-east-1::control/SECURITY_HUB_CONTROL_ID"

# コントロールを有効化
aws controltower enable-control \
    --control-identifier "arn:aws:controltower:us-east-1::control/SECURITY_HUB_CONTROL_ID" \
    --target-identifier "arn:aws:organizations::123456789012:ou/o-example/ou-example"
```

API を使用してプログラマティックにコントロールを管理できます。

## メリット

### ビジネス面

- **ガバナンス強化**: 組織全体のセキュリティポスチャを統一的に管理
- **コンプライアンス**: 規制要件への対応を効率化
- **可視性向上**: セキュリティ状態の一元的な把握

### 技術面

- **統合管理**: Control Tower と Security Hub の統合による管理の簡素化
- **自動化**: API を使用した大規模なコントロール管理
- **スケーラビリティ**: マルチアカウント環境への一括適用

## デメリット・制約事項

### 制限事項

- 各コントロールのリージョンサポートを確認する必要がある
- 一部のコントロールは特定のリージョンでのみ利用可能
- コントロールの有効化には AWS Config が必要

### 考慮すべき点

- コントロールの有効化前にサポートリージョンを確認
- 既存の Security Hub 設定との整合性を確認
- コスト影響（AWS Config ルールの評価料金）を考慮

## ユースケース

### ユースケース 1: セキュリティベースラインの確立

**シナリオ**: 新しい AWS アカウントに対して、組織のセキュリティベースラインを自動的に適用したい

**効果**: Control Tower のアカウントファクトリーと組み合わせて、新規アカウント作成時に自動的にセキュリティコントロールを適用

### ユースケース 2: コンプライアンス監査の効率化

**シナリオ**: PCI DSS や SOC 2 などの規制要件に対応するコントロールを一括で有効化したい

**効果**: 関連するコントロールをまとめて有効化し、コンプライアンス状態を継続的に監視

### ユースケース 3: コスト最適化の自動化

**シナリオ**: 未使用リソースや非効率な設定を自動的に検出したい

**効果**: コスト関連のコントロールを有効化し、最適化の機会を継続的に特定

## 料金

AWS Control Tower 自体は追加料金なしで利用できますが、以下の関連サービスの料金が発生します。

| サービス | 料金 |
|---------|------|
| AWS Config | ルール評価ごとの料金 |
| AWS Security Hub | 検出結果ごとの料金 |

## 利用可能リージョン

AWS Control Tower が利用可能なすべてのリージョン（AWS GovCloud (US) を含む）で利用できます。ただし、各コントロールのサポートリージョンは異なる場合があるため、デプロイ前に確認が必要です。

## 関連サービス・機能

- **AWS Security Hub**: セキュリティ検出結果の集約と管理
- **AWS Config**: リソース設定の評価と監視
- **AWS Organizations**: マルチアカウント管理

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/12/176-security-hub-controls-control-tower/)
- [AWS Control Tower ユーザーガイド](https://docs.aws.amazon.com/controltower/latest/controlreference/config-controls.html)
- [Security Hub コントロールリファレンス](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-controls-reference.html)
- [Service-Managed Standard: AWS Control Tower](https://docs.aws.amazon.com/securityhub/latest/userguide/service-managed-standard-aws-control-tower.html)

## まとめ

AWS Control Tower に 176 の新しい Security Hub コントロールが追加されました。セキュリティ、コスト、耐久性、運用の各ユースケースに対応するコントロールを Control Tower から直接管理できるようになり、マルチアカウント環境のガバナンスがさらに強化されました。組織全体のセキュリティポスチャを統一的に管理したい場合は、この機能を活用することをお勧めします。
