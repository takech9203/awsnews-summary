# AWS Lambda - Java 25 サポート

**リリース日**: 2025 年 11 月 17 日  
**サービス**: AWS Lambda  
**機能**: Java 25 ランタイムサポート


## 概要

AWS Lambda は、Java 25 ランタイムのサポートを開始しました。Java 25 には、Lambda 関数をより効率的に実行するための変更が含まれており、パフォーマンスの向上とリソース使用の最適化が期待できます。

最新の Java 機能を活用しながら、サーバーレスアプリケーションを構築できるようになりました。

**アップデート前の課題**

- 以前は Lambda で Java 21 までのランタイムしかサポートされておらず、最新の Java 機能を活用できなかった
- 以前は仮想スレッド (Project Loom) などの新しい並行処理機能を Lambda で使用できなかった
- 以前はパターンマッチングやレコードパターンなどの最新言語機能を活用したコードを Lambda で実行できなかった

**アップデート後の改善**

- 今回のアップデートにより、Java 25 ランタイムがサポートされ、最新の Java 機能を Lambda 関数で活用できるようになった
- 今回のアップデートにより、仮想スレッドを使用した効率的な並行処理が Lambda で実現可能になった
- 今回のアップデートにより、パターンマッチングやレコードパターンなどの新機能でより表現力豊かなコードを書けるようになった


## サービスアップデートの詳細

### 主要機能

1. **Java 25 ランタイム**
   - 最新の Java LTS バージョン
   - パフォーマンス最適化
   - 新しい言語機能のサポート

2. **Lambda 最適化**
   - 改善されたコールドスタート時間
   - 効率的なメモリ使用
   - JIT コンパイラの最適化

3. **新しい言語機能**
   - パターンマッチングの強化
   - レコードパターン
   - 仮想スレッド (Project Loom)


## 技術仕様

### ランタイム情報

| 項目 | 詳細 |
|------|------|
| ランタイム識別子 | java25 |
| アーキテクチャ | x86_64, arm64 |
| 最小メモリ | 128 MB |
| 最大メモリ | 10,240 MB |

### Java 25 の主な新機能

| 機能 | 説明 |
|------|------|
| 仮想スレッド | 軽量スレッドによる並行処理の改善 |
| パターンマッチング | switch 式でのパターンマッチング |
| レコードパターン | レコードの分解パターン |
| シーケンスコレクション | 順序付きコレクションの新 API |


## 設定方法

### 前提条件

1. Java 25 JDK がインストールされていること
2. Maven または Gradle が設定されていること
3. AWS CLI が設定されていること

### 手順

#### ステップ 1: pom.xml の設定

```xml
<properties>
    <maven.compiler.source>25</maven.compiler.source>
    <maven.compiler.target>25</maven.compiler.target>
</properties>

<dependencies>
    <dependency>
        <groupId>com.amazonaws</groupId>
        <artifactId>aws-lambda-java-core</artifactId>
        <version>1.2.3</version>
    </dependency>
</dependencies>
```

#### ステップ 2: Lambda 関数の実装

```java
package example;

import com.amazonaws.services.lambda.runtime.Context;
import com.amazonaws.services.lambda.runtime.RequestHandler;

public record Request(String name) {}
public record Response(String message) {}

public class Handler implements RequestHandler<Request, Response> {
    @Override
    public Response handleRequest(Request request, Context context) {
        return new Response("Hello, " + request.name() + "!");
    }
}
```

#### ステップ 3: デプロイ

```bash
mvn package
aws lambda create-function \
  --function-name my-java25-function \
  --runtime java25 \
  --handler example.Handler::handleRequest \
  --zip-file fileb://target/function.jar \
  --role arn:aws:iam::123456789012:role/lambda-role
```

`mvn package` でプロジェクトをビルドし、`aws lambda create-function` で Java 25 ランタイムを使用した Lambda 関数を作成します。`--runtime java25` を指定することで、最新の Java 25 ランタイムが使用されます。


## メリット

### ビジネス面

- **最新技術の活用**: 最新の Java 機能を使用可能
- **パフォーマンス向上**: 改善された実行効率
- **開発者体験**: モダンな言語機能による生産性向上

### 技術面

- **仮想スレッド**: 効率的な並行処理
- **パターンマッチング**: より表現力豊かなコード
- **改善された JIT**: 高速な実行


## デメリット・制約事項

### 制限事項

- 一部のライブラリが Java 25 に対応していない可能性
- 既存のコードベースの移行が必要な場合がある

### 考慮すべき点

- 依存ライブラリの互換性確認
- テスト環境での十分な検証


## ユースケース

### ユースケース 1: 高並行処理

**シナリオ**: 多数の同時リクエストを処理する API を構築

**効果**: 仮想スレッドにより効率的な並行処理を実現

### ユースケース 2: データ処理

**シナリオ**: 複雑なデータ構造を処理する Lambda 関数

**効果**: パターンマッチングとレコードで簡潔なコードを実現


## 料金

AWS Lambda の標準料金が適用されます。


## 利用可能リージョン

AWS Lambda が利用可能なすべてのリージョンで利用可能です。


## 関連サービス・機能

- **AWS Lambda**: サーバーレスコンピューティング
- **AWS SAM**: サーバーレスアプリケーションモデル
- **Amazon Corretto**: AWS の Java ディストリビューション


## 参考リンク

- [公式発表](https://aws.amazon.com/about-aws/whats-new/2025/11/aws-lambda-java-25/)
- [Java 25 on Lambda ブログ](https://aws.amazon.com/blogs/compute/aws-lambda-now-supports-java-25/)


## まとめ

AWS Lambda の Java 25 サポートにより、最新の Java 機能を活用したサーバーレスアプリケーションを構築できるようになりました。仮想スレッドやパターンマッチングなどの新機能により、より効率的で表現力豊かなコードを書くことができます。
