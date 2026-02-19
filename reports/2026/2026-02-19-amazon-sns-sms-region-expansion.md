# Amazon SNS - SMS 送信の追加リージョンサポート

**リリース日**: 2026 年 2 月 19 日
**サービス**: Amazon Simple Notification Service (Amazon SNS)
**機能**: SMS 送信の追加リージョン対応

📊 [このアップデートのインフォグラフィックを見る](https://takech9203.github.io/aws-news-summary/20260219-amazon-sns-sms-region-expansion.html)

## 概要

Amazon SNS を Asia Pacific (New Zealand) および Asia Pacific (Taipei) リージョンで使用しているお客様が、200 を超える国と地域のサブスクライバに SMS メッセージを送信できるようになった。

この起動により、これらのリージョンで SNS を使用しているお客様は、AWS End User Messaging を介して SMS メッセージを送信できる。Amazon SNS は現在 32 の AWS リージョンで SMS 送信をサポートしている。

**アップデート前の課題**

- Asia Pacific (New Zealand) および Asia Pacific (Taipei) リージョンでは SNS からの SMS 送信が利用できなかった
- これらのリージョンのお客様は SMS 送信のために他のリージョンを使用する必要があった

**アップデート後の改善**

- 2 つの新しいリージョンから 200 を超える国と地域への SMS 送信が可能になった
- SNS の SMS 送信対応リージョンが 32 に拡大した

## サービスアップデートの詳細

### 主要機能

1. **追加リージョン対応**
   - Asia Pacific (New Zealand) リージョンでの SMS 送信サポート
   - Asia Pacific (Taipei) リージョンでの SMS 送信サポート
   - 200 を超える国と地域への送信が可能

2. **AWS End User Messaging 統合**
   - SMS メッセージは AWS End User Messaging を介して送信
   - 既存の SNS pub/sub メッセージング機能と統合

## 技術仕様

### リージョン対応状況

| 項目 | 詳細 |
|------|------|
| 新規追加リージョン | Asia Pacific (New Zealand)、Asia Pacific (Taipei) |
| 合計対応リージョン数 | 32 リージョン |
| 送信先 | 200 を超える国と地域 |
| SMS 送信方法 | AWS End User Messaging 経由 |

## 設定方法

### 前提条件

1. 対応リージョンで Amazon SNS が有効であること
2. SMS 送信に必要な IAM 権限が設定されていること

### 手順

#### ステップ 1: SNS トピックまたは電話番号サブスクリプションの設定

対応リージョンで SNS トピックを作成し、SMS 送信先の電話番号をサブスクライバとして追加する。

## メリット

### ビジネス面

- **データ主権の確保**: New Zealand と Taipei のお客様がローカルリージョンから SMS を送信できる
- **レイテンシの改善**: 最寄りリージョンからの送信によるレスポンス時間の短縮

### 技術面

- **リージョン選択の柔軟性**: 32 リージョンから最適な送信元を選択可能
- **既存アーキテクチャとの統合**: SNS の pub/sub モデルを活用した SMS 配信

## デメリット・制約事項

### 制限事項

- 送信先の国や地域によって送信料金が異なる
- 一部の国では SMS 送信に事前登録が必要な場合がある

### 考慮すべき点

- SMS 送信にはお客様が対象国の規制に準拠する必要がある

## 料金

SMS 送信料金はリージョンと送信先の国により異なる。詳細は Amazon SNS の料金ページを参照。

## 利用可能リージョン

Amazon SNS の SMS 送信は 32 の AWS リージョンで利用可能。今回追加されたのは Asia Pacific (New Zealand) と Asia Pacific (Taipei)。

## 関連サービス・機能

- **Amazon SNS**: フルマネージドの pub/sub メッセージングサービス
- **AWS End User Messaging**: SMS メッセージの配信基盤
- **Amazon SQS**: SNS と連携するメッセージキューイングサービス

## 参考リンク

- 📊 [インフォグラフィック](https://takech9203.github.io/aws-news-summary/20260219-amazon-sns-sms-region-expansion.html)
- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-sns-sms-region-expansion)
- [SMS 送信ドキュメント](https://docs.aws.amazon.com/sns/latest/dg/sns-mobile-phone-number-as-subscriber.html)
- [対応国・地域一覧](https://docs.aws.amazon.com/sms-voice/latest/userguide/phone-numbers-sms-by-country.html)

## まとめ

Amazon SNS の SMS 送信が Asia Pacific (New Zealand) と Asia Pacific (Taipei) に拡大し、合計 32 リージョンで利用可能になった。これらのリージョンで運用しているお客様は、ローカルリージョンから直接 SMS を送信できるようになる。
