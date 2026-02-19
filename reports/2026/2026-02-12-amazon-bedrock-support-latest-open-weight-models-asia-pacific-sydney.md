# Amazon Bedrock - 最新オープンウェイトモデルが Asia Pacific (Sydney) で利用可能に

**リリース日**: 2026 年 2 月 12 日
**サービス**: Amazon Bedrock
**機能**: 最新オープンウェイトモデルの Asia Pacific (Sydney) リージョン対応

📊 [このアップデートのインフォグラフィックを見る](https://takech9203.github.io/aws-news-summary/20260212-amazon-bedrock-support-latest-open-weight-models-asia-pacific-sydney.html)

## 概要

Amazon Bedrock が Asia Pacific (Sydney) リージョンで最新のオープンウェイトモデルのサポートを開始した。bedrock-runtime エンドポイントと bedrock-mantle エンドポイントの両方を使用して、DeepSeek、Google、MiniMax、Mistral、Moonshot AI、Nvidia、OpenAI などの業界をリードするプロバイダのモデルが利用可能になった。

bedrock-runtime エンドポイントは InvokeModel/Converse/Chat Completions API を使用したリージョン固有の推論リクエスト用エンドポイントである。bedrock-mantle エンドポイントは OpenAI 互換エンドポイントを使用した推論リクエスト用で、Project Mantle ベースの分散推論エンジンによって駆動される。

**アップデート前の課題**

- Asia Pacific (Sydney) で最新のオープンウェイトモデルが利用できなかった
- Oceania 地域のお客様は他のリージョンにリクエストを送信する必要があった

**アップデート後の改善**

- Sydney リージョンで複数プロバイダの最新オープンウェイトモデルが利用可能になった
- bedrock-runtime と bedrock-mantle の両エンドポイントに対応
- データ主権要件を満たしつつ低レイテンシで推論を実行可能

## サービスアップデートの詳細

### 主要機能

1. **対応モデルプロバイダ**
   - DeepSeek
   - Google
   - MiniMax
   - Mistral
   - Moonshot AI
   - Nvidia
   - OpenAI

2. **エンドポイント対応**
   - bedrock-runtime: InvokeModel、Converse、Chat Completions API
   - bedrock-mantle: OpenAI 互換 API (Project Mantle ベース)

3. **Project Mantle**
   - 大規模 ML モデル提供のための分散推論エンジン
   - 高パフォーマンスかつ信頼性の高いサーバーレス推論
   - OpenAI API 仕様との互換性を提供

## 技術仕様

### エンドポイント比較

| エンドポイント | API | 特徴 |
|-------------|-----|------|
| bedrock-runtime | InvokeModel, Converse, Chat Completions | 標準的な Bedrock API |
| bedrock-mantle | OpenAI 互換 | Project Mantle ベース、高パフォーマンス |

## 設定方法

### 前提条件

1. Asia Pacific (Sydney) リージョンで Amazon Bedrock が有効であること
2. 使用するモデルへのアクセスが許可されていること

### 手順

#### ステップ 1: モデルアクセスの有効化

Amazon Bedrock コンソールで Asia Pacific (Sydney) リージョンを選択し、使用するオープンウェイトモデルへのアクセスをリクエストする。

#### ステップ 2: API の使用

bedrock-runtime または bedrock-mantle エンドポイントを使用して推論リクエストを送信する。

## メリット

### ビジネス面

- **データ主権**: Oceania 地域のデータ主権要件を満たしながらモデルを利用可能
- **低レイテンシ**: ローカルリージョンからの推論で応答時間が改善
- **選択肢の拡大**: 複数プロバイダの最新モデルから最適なものを選択可能

### 技術面

- **OpenAI 互換 API**: bedrock-mantle による OpenAI API 互換性
- **Project Mantle**: 分散推論エンジンによる高パフォーマンス

## デメリット・制約事項

### 制限事項

- 一部のモデルはリージョンによって利用可能性が異なる場合がある

### 考慮すべき点

- モデルごとの料金とクォータを確認すること

## 料金

モデルとリージョンに応じた Amazon Bedrock の標準料金が適用される。詳細は Bedrock 料金ページを参照。

## 利用可能リージョン

今回追加: Asia Pacific (Sydney)。

## 関連サービス・機能

- **Amazon Bedrock**: フルマネージドの基盤モデルサービス
- **Project Mantle**: Bedrock の分散推論エンジン
- **Amazon Bedrock PrivateLink**: プライベートアクセス

## 参考リンク

- 📊 [インフォグラフィック](https://takech9203.github.io/aws-news-summary/20260212-amazon-bedrock-support-latest-open-weight-models-asia-pacific-sydney.html)
- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-bedrock-support-latest-open-weight-models-asia-pacific-sydney/)
- [Amazon Bedrock ドキュメント](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html)

## まとめ

Amazon Bedrock が Asia Pacific (Sydney) で最新のオープンウェイトモデルをサポートし、Oceania 地域のお客様がローカルリージョンから複数プロバイダのモデルを利用できるようになった。データ主権要件を満たしつつ低レイテンシで推論を実行したい場合は、Sydney リージョンでの利用を検討することを推奨する。
