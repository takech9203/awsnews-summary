# Amazon WorkSpaces Applications - Ubuntu Pro 24.04 LTS Elastic フリートサポート

**リリース日**: 2025 年 12 月 18 日
**サービス**: Amazon WorkSpaces Applications
**機能**: Ubuntu Pro 24.04 LTS on Elastic fleets

## 概要

Amazon WorkSpaces Applications が、Elastic フリートで Ubuntu Pro 24.04 LTS のサポートを開始しました。これにより、独立系ソフトウェアベンダー (ISV) や中央 IT 組織は、AWS クラウドの柔軟性、スケーラビリティ、コスト効率を活用しながら、Ubuntu デスクトップアプリケーションをユーザーにストリーミングできるようになります。

Elastic フリートは、AWS が管理するストリーミングインスタンスプールからデスクトップアプリケーションをエンドユーザーにストリーミングするサーバーレスフリートタイプです。使用量の予測、スケーリングポリシーの作成・管理、イメージの作成が不要です。

**アップデート前の課題**

- Ubuntu デスクトップアプリケーションのストリーミングには、独自のインフラストラクチャ管理が必要だった
- キャパシティ管理やスケーリングポリシーの設定が複雑だった
- Ubuntu 環境でのアプリケーション配信に専門知識が必要だった

**アップデート後の改善**

- Elastic フリートで Ubuntu Pro 24.04 LTS アプリケーションをサーバーレスでストリーミング可能
- キャパシティ管理やイメージ作成が不要
- AWS が管理するインフラストラクチャで Ubuntu アプリケーションを配信

## サービスアップデートの詳細

### 主要機能

1. **サーバーレス Ubuntu アプリケーションストリーミング**
   - AWS が管理するインスタンスプールからアプリケーションをストリーミング
   - キャパシティの事前予測が不要
   - 自動スケーリング

2. **Ubuntu Pro 24.04 LTS サポート**
   - 最新の Ubuntu LTS バージョン
   - 拡張セキュリティメンテナンス
   - エンタープライズグレードのサポート

3. **Elastic フリートの利点**
   - イメージ作成不要
   - スケーリングポリシー管理不要
   - 従量課金制

### 対象ユーザー

| ユーザータイプ | ユースケース |
|--------------|-------------|
| ISV | Ubuntu ベースのアプリケーションを顧客に配信 |
| 中央 IT 組織 | 社内 Ubuntu アプリケーションの配布 |
| 開発チーム | Linux 開発環境の提供 |
| 教育機関 | Ubuntu ベースの学習環境 |

## 設定方法

### 前提条件

1. AWS アカウント
2. WorkSpaces Applications へのアクセス権限
3. 配信する Ubuntu アプリケーション

### 手順

#### ステップ 1: WorkSpaces Applications コンソールにアクセス

AWS マネジメントコンソールから WorkSpaces Applications を選択し、対象リージョンを選択します。

#### ステップ 2: Elastic フリートの作成

1. 「フリートの作成」を選択
2. フリートタイプとして「Elastic」を選択
3. プラットフォームとして「Ubuntu Pro 24.04 LTS」を選択
4. インスタンスタイプとネットワーク設定を構成

#### ステップ 3: アプリケーションの設定

1. ストリーミングするアプリケーションを指定
2. ユーザーアクセス設定を構成
3. フリートを起動

## メリット

### ビジネス面

- **コスト効率**: 従量課金制で使用した分だけ支払い
- **運用負荷の軽減**: インフラストラクチャ管理が不要
- **迅速な展開**: イメージ作成なしですぐに開始可能

### 技術面

- **スケーラビリティ**: 需要に応じた自動スケーリング
- **セキュリティ**: Ubuntu Pro のセキュリティ機能を活用
- **最新環境**: Ubuntu 24.04 LTS の最新機能を利用可能

## デメリット・制約事項

### 制限事項

- Elastic フリートでは一部のカスタマイズが制限される
- 特定のハードウェア要件があるアプリケーションには不向きな場合がある
- リージョンによって利用可能なインスタンスタイプが異なる

### 考慮すべき点

- アプリケーションの Ubuntu 24.04 LTS との互換性を確認
- ネットワークレイテンシーがユーザーエクスペリエンスに影響する可能性

## ユースケース

### ユースケース 1: ISV によるアプリケーション配信

**シナリオ**: ISV が Ubuntu ベースの専門アプリケーションを顧客に提供したい。

**効果**: インフラストラクチャ管理なしで、グローバルに Ubuntu アプリケーションを配信可能。

### ユースケース 2: 開発環境の提供

**シナリオ**: 開発チームに統一された Ubuntu 開発環境を提供したい。

**効果**: 一貫した開発環境を迅速に展開し、オンボーディング時間を短縮。

## 料金

Amazon WorkSpaces Applications は従量課金制です。詳細は [Amazon WorkSpaces Applications 料金ページ](https://aws.amazon.com/appstream2/pricing/) を参照してください。

## 利用可能リージョン

WorkSpaces Applications が利用可能なすべての AWS リージョンで利用可能です。詳細は [AWS リージョン表](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) を参照してください。

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/12/amazon-workspaces-ubuntu-elasticfleets/)
- [Amazon WorkSpaces Applications](https://aws.amazon.com/workspaces/applications/)
- [料金ページ](https://aws.amazon.com/appstream2/pricing/)

## まとめ

Amazon WorkSpaces Applications の Ubuntu Pro 24.04 LTS Elastic フリートサポートにより、Ubuntu アプリケーションのストリーミングがサーバーレスで実現可能になりました。ISV や IT 組織は、インフラストラクチャ管理の負担なく、Ubuntu アプリケーションをユーザーに配信できます。
