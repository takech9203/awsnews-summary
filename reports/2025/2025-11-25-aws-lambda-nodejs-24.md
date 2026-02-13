# AWS Lambda - Node.js 24 ランタイムサポート

**リリース日**: 2025 年 11 月 25 日
**サービス**: AWS Lambda
**機能**: Node.js 24 ランタイムサポート

## 概要

AWS Lambda が Node.js 24 をマネージドランタイムおよびコンテナベースイメージとしてサポートしました。Node.js 24 は最新の長期サポート (LTS) リリースであり、2028 年 4 月までセキュリティおよびバグ修正のサポートが予定されています。このリリースでは、モダンな async/await プログラミングパターンに焦点を当て、コールバックベースの関数ハンドラーのサポートが廃止されました。

**アップデート前の課題**

- Node.js 22 以前のランタイムを使用していた
- コールバックベースとプロミスベースの両方のハンドラーをサポートする必要があった
- 未解決のプロミスの処理に一貫性がなかった

**アップデート後の改善**

- Node.js 24 LTS の最新機能を利用可能
- async/await パターンに統一されたシンプルな開発体験
- TypeScript で書き直された新しい Runtime Interface Client (RIC)
- Explicit Resource Management などの新しい言語機能

## サービスアップデートの詳細

### 主要機能

1. **モダンな async/await パターン**
   - コールバックベースの関数ハンドラーのサポートを廃止
   - async/await パターンに統一
   - 未解決のプロミスの処理が一貫性を持つように改善

2. **新しい Runtime Interface Client (RIC)**
   - TypeScript で書き直された新しい実装
   - Node.js サポートの簡素化
   - レガシー機能の削除

3. **言語機能の追加**
   - Explicit Resource Management のサポート
   - ES モジュールのインライン関数での使用が可能
   - 標準ライブラリの更新

4. **Lambda@Edge サポート**
   - サポートされているリージョンで Lambda@Edge で使用可能
   - Amazon CloudFront を通じた低レイテンシコンテンツ配信

## 技術仕様

### ランタイム仕様

| 項目 | 詳細 |
|------|------|
| Node.js バージョン | 24.x (LTS) |
| サポート期限 | 2028 年 4 月 |
| ベース OS | Amazon Linux 2023 |
| パッケージマネージャー | microdnf |

### 変更点

| 項目 | Node.js 22 | Node.js 24 |
|------|-----------|-----------|
| コールバックハンドラー | サポート | 非サポート |
| RIC 実装 | JavaScript | TypeScript |
| ES モジュール | 制限あり | フルサポート |

## 設定方法

### 前提条件

1. AWS アカウント
2. Lambda 関数の作成権限
3. Node.js 24 対応のコード

### 手順

#### ステップ 1: コンソールでの設定

AWS Management Console で Lambda 関数を作成または更新する際に、ランタイムとして「Node.js 24.x」を選択します。

#### ステップ 2: AWS CLI での設定

```bash
aws lambda create-function \
    --function-name my-function \
    --runtime nodejs24.x \
    --handler index.handler \
    --role arn:aws:iam::123456789012:role/lambda-role \
    --zip-file fileb://function.zip
```

新しい Lambda 関数を Node.js 24 ランタイムで作成します。

#### ステップ 3: AWS SAM での設定

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31

Resources:
  MyFunction:
    Type: AWS::Serverless::Function
    Properties:
      Handler: index.handler
      Runtime: nodejs24.x
      CodeUri: ./src
```

SAM テンプレートで Node.js 24 ランタイムを指定します。

#### ステップ 4: AWS CDK での設定

```typescript
import * as lambda from 'aws-cdk-lib/aws-lambda';

new lambda.Function(this, 'MyFunction', {
  runtime: lambda.Runtime.NODEJS_24_X,
  handler: 'index.handler',
  code: lambda.Code.fromAsset('lambda'),
});
```

CDK で Node.js 24 ランタイムを指定します。

## メリット

### ビジネス面

- **長期サポート**: 2028 年 4 月までのセキュリティサポート
- **開発効率向上**: モダンな JavaScript 機能で生産性向上
- **エコシステム対応**: Powertools for AWS Lambda (TypeScript) サポート

### 技術面

- **パフォーマンス向上**: 新しい RIC 実装による改善
- **シンプルな開発体験**: async/await パターンへの統一
- **新機能**: Explicit Resource Management などの言語機能

## デメリット・制約事項

### 制限事項

- コールバックベースの関数ハンドラーは非サポート
- 既存のコールバックベースのコードは移行が必要
- 一部の実験的機能はデフォルトで無効

### 考慮すべき点

- 既存関数のコード移行が必要な場合がある
- サードパーティライブラリの互換性確認
- パフォーマンステストの実施を推奨

## ユースケース

### ユースケース 1: 新規サーバーレスアプリケーション

**シナリオ**: 新しいサーバーレスアプリケーションを Node.js で開発

**実装例**:
```javascript
export const handler = async (event) => {
    // async/await パターンを使用
    const result = await processEvent(event);
    return {
        statusCode: 200,
        body: JSON.stringify(result)
    };
};
```

**効果**: 最新の Node.js 機能を活用した効率的な開発

### ユースケース 2: Lambda@Edge でのコンテンツカスタマイズ

**シナリオ**: CloudFront でのリクエスト/レスポンスのカスタマイズ

**効果**: 低レイテンシでのコンテンツ配信カスタマイズ

### ユースケース 3: 既存関数のアップグレード

**シナリオ**: Node.js 22 から Node.js 24 への移行

**効果**: 長期サポートと最新機能の恩恵を受ける

## 料金

Node.js 24 ランタイムの追加料金はありません。標準の Lambda 料金が適用されます。

| 項目 | 料金 |
|------|------|
| リクエスト | 100 万リクエストあたり $0.20 |
| 実行時間 | GB-秒あたり $0.0000166667 |

## 利用可能リージョン

すべての AWS リージョン（AWS GovCloud (US) および中国リージョンを含む）で利用可能です。

## 関連サービス・機能

- **Powertools for AWS Lambda (TypeScript)**: サーバーレスベストプラクティスの実装
- **Lambda@Edge**: CloudFront でのエッジコンピューティング
- **AWS SAM**: サーバーレスアプリケーションのデプロイ

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/11/aws-lambda-nodejs-24/)
- [AWS Blog](https://aws.amazon.com/blogs/compute/node-js-24-runtime-now-available-in-aws-lambda/)
- [Powertools for AWS Lambda (TypeScript)](https://docs.powertools.aws.dev/lambda/typescript/latest/)
- [AWS Lambda 製品ページ](https://aws.amazon.com/lambda/)

## まとめ

AWS Lambda が Node.js 24 LTS をサポートしました。async/await パターンへの統一、TypeScript で書き直された新しい RIC、Explicit Resource Management などの新機能により、モダンなサーバーレス開発が可能になります。2028 年 4 月までの長期サポートで、安定した運用が期待できます。
