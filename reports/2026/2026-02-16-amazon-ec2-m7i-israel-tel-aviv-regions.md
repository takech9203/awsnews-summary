# Amazon EC2 - M7i インスタンスが Israel (Tel Aviv) で利用可能に

**リリース日**: 2026 年 2 月 16 日
**サービス**: Amazon EC2
**機能**: M7i インスタンスの Israel (Tel Aviv) リージョン展開

📊 [このアップデートのインフォグラフィックを見る](https://takech9203.github.io/aws-news-summary/20260216-amazon-ec2-m7i-israel-tel-aviv-regions.html)

## 概要

Amazon EC2 M7i インスタンスが Israel (Tel Aviv) リージョンで利用可能になった。M7i インスタンスはカスタム第 4 世代 Intel Xeon Scalable プロセッサ (Sapphire Rapids) を搭載し、M6i と比較して最大 15% のプライスパフォーマンス向上を提供する。

M7i は 48xlarge までの大型サイズとベアメタル 2 サイズ (metal-24xl、metal-48xl) を提供する。ベアメタルサイズでは Intel Data Streaming Accelerator、In-Memory Analytics Accelerator、QuickAssist Technology などの組み込みアクセラレータが利用可能である。

**アップデート前の課題**

- Israel (Tel Aviv) リージョンでは M7i が利用できなかった
- 高パフォーマンスな汎用インスタンスの選択肢が限られていた

**アップデート後の改善**

- Israel (Tel Aviv) で M7i が利用可能になった
- 48xlarge やベアメタルサイズが利用可能になった
- Intel 組み込みアクセラレータが利用可能になった

## サービスアップデートの詳細

### 主要機能

1. **M7i インスタンス**
   - カスタム第 4 世代 Intel Xeon Scalable プロセッサ
   - M6i 比で最大 15% のプライスパフォーマンス向上
   - 48xlarge を含む大型サイズとベアメタル 2 サイズ

2. **Intel 組み込みアクセラレータ** (ベアメタル)
   - Data Streaming Accelerator: データ操作のオフロード
   - In-Memory Analytics Accelerator: インメモリ分析の高速化
   - QuickAssist Technology: 暗号化・圧縮の加速

## 技術仕様

### M7i インスタンスサイズ

| 項目 | 詳細 |
|------|------|
| プロセッサ | カスタム第 4 世代 Intel Xeon Scalable |
| サイズ範囲 | large ～ 48xlarge |
| ベアメタル | metal-24xl、metal-48xl |
| M6i 比パフォーマンス | 最大 15% 向上 |

## 設定方法

### 前提条件

1. Israel (Tel Aviv) リージョンの AWS アカウントを保有していること

### 手順

#### ステップ 1: インスタンスの起動

AWS マネジメントコンソールで Israel (Tel Aviv) リージョンを選択し、M7i インスタンスタイプを指定して起動する。

## メリット

### ビジネス面

- **Israel リージョンの選択肢拡大**: ローカルリージョンで最新の汎用インスタンスが利用可能
- **コスト効率**: M6i 比で最大 15% のプライスパフォーマンス向上

### 技術面

- **ベアメタルサイズ**: 大規模ワークロードに対応する 48xlarge とベアメタルサイズ
- **Intel アクセラレータ**: データ処理と暗号化の加速

## デメリット・制約事項

### 制限事項

- Intel アクセラレータはベアメタルサイズでのみ利用可能

## 料金

リージョン固有の料金が適用される。詳細は EC2 料金ページを参照。

## 利用可能リージョン

今回追加: Israel (Tel Aviv)。

## 関連サービス・機能

- **Amazon EC2 M8i**: 次世代の汎用インスタンス
- **Amazon EC2 M7i-flex**: M7i の Flex バリアント

## 参考リンク

- 📊 [インフォグラフィック](https://takech9203.github.io/aws-news-summary/20260216-amazon-ec2-m7i-israel-tel-aviv-regions.html)
- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/02/amazon-ec2-m7i-israel-tel-aviv-regions)
- [M7i インスタンスページ](https://aws.amazon.com/ec2/instance-types/m7i/)

## まとめ

M7i インスタンスが Israel (Tel Aviv) リージョンで利用可能になった。M6i からの移行でプライスパフォーマンスが向上し、ベアメタルサイズでは Intel 組み込みアクセラレータも利用できる。Israel リージョンで汎用インスタンスを使用しているお客様は、M7i への移行を検討することを推奨する。
