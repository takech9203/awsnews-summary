# AWS KMS - EdDSA (Edwards-curve Digital Signature Algorithm) サポート

**リリース日**: 2025 年 11 月 17 日  
**サービス**: AWS Key Management Service (KMS)  
**機能**: EdDSA (Ed25519) サポート


## 概要

AWS KMS は、Edwards-curve Digital Signature Algorithm (EdDSA) をサポートしました。この機能により、NIST P-256 と同等の 128 ビットセキュリティを、より高速な署名パフォーマンスとコンパクトなサイズ (64 バイトの署名、32 バイトの公開鍵) で実現できます。

Ed25519 は、小さな鍵と署名サイズが求められる IoT デバイスやブロックチェーンアプリケーションに最適です。

**アップデート前の課題**

- 以前は KMS で ECDSA (P-256、P-384、P-521) や RSA などの署名アルゴリズムのみがサポートされており、EdDSA を使用できなかった
- 以前は IoT デバイスやブロックチェーンアプリケーションで求められるコンパクトな署名サイズを実現するのが困難だった
- 以前は高速な署名パフォーマンスが必要な場合、KMS 以外のソリューションを検討する必要があった

**アップデート後の改善**

- 今回のアップデートにより、Ed25519 アルゴリズムが KMS でサポートされ、64 バイトの署名と 32 バイトの公開鍵というコンパクトなサイズで署名が可能になった
- 今回のアップデートにより、NIST P-256 と同等の 128 ビットセキュリティをより高速なパフォーマンスで実現できるようになった
- 今回のアップデートにより、IoT デバイスやブロックチェーンアプリケーションに最適な署名アルゴリズムを KMS で利用できるようになった


## サービスアップデートの詳細

### 主要機能

1. **Ed25519 アルゴリズム**
   - 128 ビットセキュリティ (NIST P-256 相当)
   - 高速な署名パフォーマンス
   - コンパクトなサイズ

2. **効率的なサイズ**
   - 署名サイズ: 64 バイト
   - 公開鍵サイズ: 32 バイト
   - 秘密鍵サイズ: 32 バイト

3. **幅広い用途**
   - IoT デバイス
   - ブロックチェーンアプリケーション
   - 証明書署名


## 技術仕様

### Ed25519 の特徴

| 項目 | Ed25519 | ECDSA P-256 |
|------|---------|-------------|
| セキュリティレベル | 128 ビット | 128 ビット |
| 署名サイズ | 64 バイト | 64 バイト |
| 公開鍵サイズ | 32 バイト | 64 バイト |
| 署名速度 | 高速 | 中程度 |
| 検証速度 | 高速 | 中程度 |

### KMS での使用

```bash
# Ed25519 キーの作成
aws kms create-key \
  --key-spec ECC_EDWARDS_25519 \
  --key-usage SIGN_VERIFY

# 署名の作成
aws kms sign \
  --key-id alias/my-ed25519-key \
  --message fileb://message.txt \
  --message-type RAW \
  --signing-algorithm EDDSA_SHA_512
```

これらのコマンドは、KMS で Ed25519 キーを作成し、メッセージに署名する例です。`--key-spec ECC_EDWARDS_25519` で Ed25519 アルゴリズムを指定し、`--signing-algorithm EDDSA_SHA_512` で署名アルゴリズムを指定します。


## 設定方法

### 前提条件

1. AWS アカウント
2. KMS へのアクセス権限
3. 適切な IAM ポリシー

### 手順

#### ステップ 1: Ed25519 キーの作成

```bash
aws kms create-key \
  --key-spec ECC_EDWARDS_25519 \
  --key-usage SIGN_VERIFY \
  --description "Ed25519 signing key"
```

#### ステップ 2: エイリアスの設定

```bash
aws kms create-alias \
  --alias-name alias/my-ed25519-key \
  --target-key-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

#### ステップ 3: 署名の作成と検証

```bash
# 署名
aws kms sign \
  --key-id alias/my-ed25519-key \
  --message "Hello, World!" \
  --message-type RAW \
  --signing-algorithm EDDSA_SHA_512

# 検証
aws kms verify \
  --key-id alias/my-ed25519-key \
  --message "Hello, World!" \
  --message-type RAW \
  --signing-algorithm EDDSA_SHA_512 \
  --signature <base64-signature>
```

`aws kms sign` コマンドでメッセージに署名し、`aws kms verify` コマンドで署名を検証します。Ed25519 アルゴリズムにより、高速かつコンパクトな署名と検証が実行されます。


## メリット

### ビジネス面

- **コスト効率**: 小さなデータサイズによるストレージと帯域幅の削減
- **パフォーマンス**: 高速な署名と検証
- **互換性**: 業界標準のアルゴリズム

### 技術面

- **コンパクト**: 小さな鍵と署名サイズ
- **高速**: 効率的な署名と検証
- **セキュア**: 128 ビットセキュリティ


## デメリット・制約事項

### 制限事項

- 暗号化には使用できない (署名/検証のみ)
- 一部の古いシステムとの互換性

### 考慮すべき点

- アプリケーションの Ed25519 サポート確認
- 既存の署名システムとの移行計画


## ユースケース

### ユースケース 1: IoT デバイス認証

**シナリオ**: リソース制約のある IoT デバイスで署名を使用したい

**効果**: 小さな鍵サイズと高速な署名で効率的な認証を実現

### ユースケース 2: ブロックチェーン

**シナリオ**: ブロックチェーントランザクションに署名したい

**効果**: Ed25519 の広範なブロックチェーンサポートを活用

### ユースケース 3: ソフトウェア署名

**シナリオ**: ソフトウェアパッケージに署名したい

**効果**: コンパクトな署名でパッケージサイズへの影響を最小化


## 料金

KMS の標準料金が適用されます。Ed25519 キーの使用に追加料金はありません。


## 利用可能リージョン

AWS KMS が利用可能なすべてのリージョンで利用可能です。


## 関連サービス・機能

- **AWS KMS**: 鍵管理サービス
- **AWS CloudHSM**: ハードウェアセキュリティモジュール
- **AWS Certificate Manager**: 証明書管理


## 参考リンク

- [公式発表](https://aws.amazon.com/about-aws/whats-new/2025/11/aws-kms-edwards-curve-digital-signature-algorithm/)
- [KMS ドキュメント](https://docs.aws.amazon.com/kms/)


## まとめ

AWS KMS の EdDSA (Ed25519) サポートにより、高速でコンパクトなデジタル署名が可能になりました。IoT デバイスやブロックチェーンアプリケーションなど、効率的な署名が求められるユースケースに最適です。
