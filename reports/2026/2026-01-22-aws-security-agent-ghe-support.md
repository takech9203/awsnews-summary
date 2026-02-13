# AWS Security Agent - GitHub Enterprise Cloud サポート

**リリース日**: 2026年01月22日
**サービス**: AWS Security Agent
**機能**: GitHub Enterprise Cloud のサポート追加

## 概要

AWS Security Agent が GitHub Enterprise Cloud のサポートを追加しました。これにより、顧客は GitHub Enterprise Organization を接続し、プライベートリポジトリ全体で AI を活用したセキュリティ機能を利用できるようになります。この拡張により、開発チームはセキュリティ分析を GitHub ワークフローに直接統合できます。

顧客は、必要な権限を持つ AWS Security Agent GitHub app をインストールすることで、GitHub Enterprise Organization を AWS Security Agent に接続できます。接続後、プライベートリポジトリに対して 3 つの主要機能が提供されます。

**アップデート前の課題**

- GitHub Enterprise Cloud のプライベートリポジトリで AWS Security Agent を使用できなかった
- エンタープライズレベルの GitHub 環境でのセキュリティ自動化が困難だった
- プライベートリポジトリのセキュリティレビューとペネトレーションテストが手動で実施されていた

**アップデート後の改善**

- GitHub Enterprise Organization を AWS Security Agent に接続可能になった
- プライベートリポジトリで AI を活用したセキュリティ分析を実行できるようになった
- セキュリティレビュー、ペネトレーションテスト、自動修正が統合されたワークフローで実現可能になった

## サービスアップデートの詳細

### 主要機能

1. **自動コードレビュー (Automated Code Reviews)**
   - 新しいプルリクエストに対して包括的なセキュリティレビューを実施
   - コードがマージされる前に脆弱性を特定
   - 内部セキュリティ要件へのコンプライアンスを検証
   - AI を活用した高度な脆弱性検出

2. **ペネトレーションテスト統合 (Penetration Testing Integration)**
   - ペネトレーションテスト活動中に GitHub Enterprise コードリポジトリを活用
   - コードベースを分析して潜在的なセキュリティ脆弱性を特定
   - 攻撃ベクトルを分析し、セキュリティリスクを評価
   - エンタープライズ環境での包括的なセキュリティテストを実現

3. **自動コード修正 (Automated Code Remediation)**
   - ペネトレーションテスト中にセキュリティ問題が特定された際に自動で修正を提案
   - 推奨される修正内容を含むプルリクエストを自動作成
   - 修正ワークフローを加速し、セキュリティ問題の解決時間を短縮
   - 開発者の負担を軽減しながら高いセキュリティ水準を維持

## 技術仕様

### サポート対象

| 項目 | 詳細 |
|------|------|
| サポート環境 | GitHub Enterprise Cloud |
| リポジトリタイプ | プライベートリポジトリ |
| 統合方法 | AWS Security Agent GitHub app のインストール |
| 利用可能リージョン | US East (N. Virginia) |

### 統合要件

- GitHub Enterprise Organization への管理者アクセス
- AWS Security Agent GitHub app のインストール権限
- 必要な権限の付与 (コードレビュー、プルリクエスト作成など)

## 設定方法

### 前提条件

1. GitHub Enterprise Cloud の組織管理者アクセス
2. AWS アカウントと AWS Security Agent へのアクセス
3. US East (N. Virginia) リージョンへのアクセス

### 手順

#### ステップ1: AWS Security Agent コンソールにアクセス

[AWS Security Agent console](https://console.aws.amazon.com/security-agent) にサインインします。

#### ステップ2: GitHub Enterprise Organization の接続

AWS Security Agent GitHub app を GitHub Enterprise Organization にインストールします。

#### ステップ3: 権限の付与

必要な権限 (コードレビュー、プルリクエスト作成、リポジトリアクセスなど) を AWS Security Agent に付与します。

#### ステップ4: プライベートリポジトリの選択

セキュリティ分析を実施するプライベートリポジトリを選択します。

#### ステップ5: 機能の有効化

自動コードレビュー、ペネトレーションテスト統合、自動コード修正の各機能を有効化します。

## メリット

### ビジネス面

- **セキュリティリスクの削減**: コードがマージされる前に脆弱性を特定し、セキュリティリスクを低減
- **開発速度の維持**: 自動化されたセキュリティレビューにより、開発スピードを損なわずにセキュリティを強化
- **コンプライアンス強化**: 内部セキュリティ要件へのコンプライアンスを自動検証

### 技術面

- **AI を活用した脆弱性検出**: 高度な AI 技術により、より正確に脆弱性を検出
- **自動修正**: セキュリティ問題の修正を自動化し、開発者の負担を軽減
- **ワークフロー統合**: GitHub の既存ワークフローにシームレスに統合

## デメリット・制約事項

### 制限事項

- US East (N. Virginia) リージョンでのみ利用可能
- GitHub Enterprise Cloud のみサポート (GitHub Enterprise Server は未対応)
- プライベートリポジトリのみが対象

### 考慮すべき点

- AWS Security Agent GitHub app に必要な権限を付与する必要がある
- 自動修正の提案を適切にレビューする必要がある
- 組織のセキュリティポリシーと整合性を確認する必要がある

## ユースケース

### ユースケース1: プルリクエストの自動セキュリティレビュー

**シナリオ**: 開発者が新しい機能を実装し、プルリクエストを作成

**実装例**:
- AWS Security Agent が自動的にプルリクエストをレビュー
- SQL インジェクション、XSS、認証の脆弱性などを検出
- プルリクエストにコメントを追加し、脆弱性を報告

**効果**: コードがマージされる前にセキュリティ問題を特定し、本番環境への脆弱性の混入を防止

### ユースケース2: ペネトレーションテストの自動化

**シナリオ**: 定期的なペネトレーションテストの実施

**実装例**:
- AWS Security Agent が GitHub Enterprise リポジトリを分析
- 潜在的な攻撃ベクトルとセキュリティ脆弱性を特定
- ペネトレーションテストレポートを生成

**効果**: 手動でのペネトレーションテストの負担を軽減し、継続的なセキュリティテストを実現

### ユースケース3: 自動セキュリティ修正

**シナリオ**: ペネトレーションテストでセキュリティ問題が発見された

**実装例**:
- AWS Security Agent が推奨される修正内容を自動生成
- 修正を含むプルリクエストを自動作成
- 開発者が修正内容をレビューしてマージ

**効果**: セキュリティ問題の修正を加速し、セキュリティインシデントのリスクを低減

## 料金

AWS Security Agent の料金は、使用量に基づいて課金されます。詳細な料金情報は [AWS Security Agent pricing page](https://aws.amazon.com/security-agent/pricing/) を参照してください (利用可能な場合)。

## 利用可能リージョン

AWS Security Agent は、US East (N. Virginia) リージョンで利用可能です。

## 関連サービス・機能

- **Amazon CodeGuru Security**: コードレビューとセキュリティ推奨事項を提供
- **AWS CodePipeline**: CI/CD パイプラインでのセキュリティテスト統合
- **Amazon Inspector**: アプリケーションのセキュリティ脆弱性を自動評価

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/aws-security-agent-ghe-support/)
- [AWS Security Agent console](https://console.aws.amazon.com/security-agent)
- [AWS Security Agent product page](https://aws.amazon.com/security-agent)

## まとめ

AWS Security Agent の GitHub Enterprise Cloud サポートにより、エンタープライズレベルの GitHub 環境でプライベートリポジトリのセキュリティを AI を活用して強化できるようになりました。自動コードレビュー、ペネトレーションテスト統合、自動コード修正により、開発速度を維持しながらセキュリティリスクを低減できます。GitHub Enterprise Cloud を使用している組織は、AWS Security Agent を統合してセキュリティワークフローを自動化し、より安全なアプリケーション開発を実現してください。
