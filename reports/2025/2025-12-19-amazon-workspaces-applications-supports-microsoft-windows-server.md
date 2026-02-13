# Amazon WorkSpaces Applications - Microsoft Windows Server 2025 サポート

**リリース日**: 2025 年 12 月 19 日
**サービス**: Amazon WorkSpaces Applications
**機能**: Microsoft Windows Server 2025 イメージサポート

## 概要

Amazon WorkSpaces Applications が Microsoft Windows Server 2025 を搭載したイメージのサポートを開始しました。これにより、お客様は Microsoft の最新サーバーオペレーティングシステムの機能と強化を活用したストリーミングインスタンスを起動できます。

Windows Server 2025 サポートにより、エンドユーザーに Microsoft Windows 11 デスクトップエクスペリエンスを提供でき、特定のアプリケーションやデスクトップストリーミングのニーズに合わせた柔軟なオペレーティングシステム選択が可能になります。

**アップデート前の課題**

- 最新の Windows Server 機能を活用できなかった
- Windows 11 デスクトップエクスペリエンスの提供が制限されていた
- 最新のセキュリティ機能やパフォーマンス改善を利用できなかった

**アップデート後の改善**

- Windows Server 2025 の最新機能を活用可能
- Windows 11 デスクトップエクスペリエンスをエンドユーザーに提供
- 改善されたセキュリティ、パフォーマンス、モダンな機能を利用可能
- AWS 提供のパブリックイメージまたは Image Builder でカスタムイメージを作成可能

## サービスアップデートの詳細

### 主要機能

1. **Windows Server 2025 イメージ**
   - AWS 提供のパブリックイメージを利用可能
   - Image Builder でカスタムイメージを作成可能
   - 最新のセキュリティ機能を搭載

2. **Windows 11 デスクトップエクスペリエンス**
   - モダンな UI をエンドユーザーに提供
   - 最新の Windows 機能を活用
   - 生産性向上

3. **柔軟なデプロイメントオプション**
   - ビジネスクリティカルなアプリケーションの実行
   - 専門ソフトウェアへのリモートアクセス
   - 組織の標準に合わせたインフラストラクチャ選択

### Windows Server 2025 の主な特徴

| 機能 | 説明 |
|------|------|
| セキュリティ強化 | 最新のセキュリティ機能とパッチ |
| パフォーマンス改善 | 最適化されたリソース管理 |
| モダン機能 | 最新の Windows 機能セット |
| 互換性 | 幅広いアプリケーションサポート |

## 設定方法

### 前提条件

1. AWS アカウント
2. Amazon WorkSpaces Applications へのアクセス
3. 適切な IAM 権限

### 手順

#### ステップ 1: イメージの選択

AWS マネジメントコンソールから WorkSpaces Applications にアクセスし、Windows Server 2025 ベースのイメージを選択します。

- AWS 提供のパブリックイメージを使用
- または Image Builder でカスタムイメージを作成

#### ステップ 2: フリートの作成

1. 「フリートの作成」を選択
2. Windows Server 2025 イメージを選択
3. インスタンスタイプとネットワーク設定を構成
4. フリートを起動

#### ステップ 3: アプリケーションの設定

1. ストリーミングするアプリケーションをインストール
2. ユーザーアクセス設定を構成
3. ストリーミングを開始

## メリット

### ビジネス面

- **最新機能の活用**: Windows Server 2025 の最新機能を利用
- **セキュリティ強化**: 最新のセキュリティパッチと機能
- **ユーザーエクスペリエンス向上**: Windows 11 デスクトップエクスペリエンス

### 技術面

- **パフォーマンス改善**: 最適化されたリソース管理
- **互換性**: 最新アプリケーションとの互換性
- **柔軟性**: カスタムイメージ作成オプション

## デメリット・制約事項

### 制限事項

- 一部のレガシーアプリケーションは互換性確認が必要
- Windows Server 2025 ライセンスコストが発生
- 既存のカスタムイメージは再作成が必要

### 考慮すべき点

- アプリケーションの Windows Server 2025 互換性を事前にテスト
- ユーザートレーニングの必要性を評価

## ユースケース

### ユースケース 1: エンタープライズアプリケーションのストリーミング

**シナリオ**: 最新の Windows 環境でビジネスクリティカルなアプリケーションをリモートユーザーに提供したい。

**効果**: Windows Server 2025 の最新セキュリティ機能を活用しながら、Windows 11 エクスペリエンスでアプリケーションを配信。

### ユースケース 2: 開発環境の提供

**シナリオ**: 開発者に最新の Windows 開発環境を提供したい。

**効果**: Windows Server 2025 ベースの開発環境を迅速にプロビジョニング。

## 料金

Amazon WorkSpaces Applications の標準料金が適用されます。詳細は [Amazon WorkSpaces Applications 料金ページ](https://aws.amazon.com/appstream2/pricing/) を参照してください。

## 利用可能リージョン

Amazon WorkSpaces Applications が提供されているすべての AWS リージョンで利用可能です。

## 関連サービス・機能

- **Amazon WorkSpaces Applications Image Builder**: カスタムイメージの作成
- **AWS Directory Service**: ユーザー認証
- **Amazon S3**: アプリケーション設定の保存

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/12/amazon-workspaces-applications-supports-microsoft-windows-server/)
- [Amazon WorkSpaces Applications ドキュメント](https://docs.aws.amazon.com/appstream2/)
- [料金ページ](https://aws.amazon.com/appstream2/pricing/)

## まとめ

Amazon WorkSpaces Applications の Windows Server 2025 サポートにより、お客様は最新の Windows 環境でアプリケーションをストリーミングできるようになりました。セキュリティ、パフォーマンス、ユーザーエクスペリエンスの向上を求める組織に推奨します。
