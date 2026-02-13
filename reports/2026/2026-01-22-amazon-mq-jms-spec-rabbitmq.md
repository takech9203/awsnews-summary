# Amazon MQ for RabbitMQ - Java Messaging Service (JMS) 仕様のサポート

**リリース日**: 2026 年 1 月 22 日
**サービス**: Amazon MQ
**機能**: RabbitMQ 4 ブローカーでの JMS サポート

## 概要

Amazon MQ は、RabbitMQ 4 ブローカーで Java Messaging Service (JMS) 仕様をサポート開始しました。RabbitMQ JMS Topic Exchange プラグインと JMS クライアントを通じて、RabbitMQ 4 ブローカーが JMS アプリケーションに接続できるようになります。JMS Topic Exchange プラグインはすべての RabbitMQ 4 ブローカーでデフォルトで有効化されており、JMS 1.1、JMS 2.0、JMS 3.1 アプリケーションを RabbitMQ 上で実行できます。

さらに、RabbitMQ JMS クライアントを使用して、JMS メッセージを AMQP エクスチェンジに送信したり、AMQP キューからメッセージを消費したりすることで、JMS ワークロードと AMQP ワークロードを相互運用または移行できます。これにより、既存の JMS アプリケーションを RabbitMQ に移行する際の柔軟性が大幅に向上します。

**アップデート前の課題**

- Amazon MQ for RabbitMQ では JMS 仕様をサポートしていなかったため、JMS アプリケーションを直接実行できなかった
- JMS ベースのアプリケーションを RabbitMQ に移行するには、アプリケーションコードを AMQP に書き換える必要があった
- 既存の JMS 資産を活用しながら RabbitMQ のメリットを享受することが困難だった

**アップデート後の改善**

- RabbitMQ 4 ブローカーで JMS 1.1、2.0、3.1 アプリケーションを変更なしで実行可能になった
- JMS と AMQP の相互運用により、段階的な移行戦略を実装できるようになった
- JMS Topic Exchange プラグインがデフォルトで有効化されているため、追加設定なしで JMS を利用可能

## サービスアップデートの詳細

### 主要機能

1. **JMS 仕様のフルサポート**
   - JMS 1.1、JMS 2.0、JMS 3.1 の各バージョンをサポート
   - サーバーセッション、XA トランザクション、JMS キューの宛先セレクター、JMS NoLocal サブスクリプション属性を除くすべての JMS API をサポート
   - JMS 2.0 および 3.1 で新たに追加された API もサポート (JMSProducer.setDeliveryDelay を除く)

2. **RabbitMQ JMS Topic Exchange プラグイン**
   - すべての RabbitMQ 4 ブローカーでデフォルトで有効化
   - RabbitMQ JMS クライアントを使用して JMS ワークロードを実行
   - プラグインは JMS クライアントが使用されたときのみアクティベート

3. **JMS と AMQP の相互運用**
   - JMS メッセージを AMQP エクスチェンジに送信可能
   - AMQP キューからメッセージを消費可能
   - JMS ワークロードから AMQP ワークロードへの段階的移行をサポート

4. **認証と認可**
   - RabbitMQ ブローカーへの接続に使用する認証情報は、AMQP Java クライアントと同じ
   - OAuth 2.0、LDAP、HTTP、SSL 証明書認証などの認証メカニズムをサポート

## 技術仕様

### サポートされる JMS バージョン

| JMS バージョン | サポート状況 | 備考 |
|----------------|--------------|------|
| JMS 1.1 | ✓ | フルサポート |
| JMS 2.0 | ✓ | JMSProducer.setDeliveryDelay を除く |
| JMS 3.1 | ✓ | JMSProducer.setDeliveryDelay を除く |

### サポートされない JMS 機能

- サーバーセッション (Server Sessions)
- XA トランザクション
- JMS キューの宛先セレクター (JMS Queue destination selector)
- JMS NoLocal サブスクリプション属性
- JMSProducer.setDeliveryDelay (JMS 2.0/3.1)

### 必要な構成

| 項目 | 詳細 |
|------|------|
| RabbitMQ バージョン | RabbitMQ 4.2 以降 |
| インスタンスタイプ | M7g (Graviton3 ベース) |
| プラグイン | JMS Topic Exchange プラグイン (デフォルトで有効) |
| クライアント | RabbitMQ JMS クライアント |

## 設定方法

### 前提条件

1. AWS アカウントを持っていること
2. Amazon MQ の利用権限があること
3. RabbitMQ JMS クライアントライブラリを含む Java 開発環境

### 手順

#### ステップ 1: RabbitMQ 4.2 ブローカーを作成

AWS マネジメントコンソール、AWS CLI、または AWS SDK を使用して、RabbitMQ 4.2 ブローカーを作成します。

```bash
aws mq create-broker \
  --broker-name my-rabbitmq-broker \
  --engine-type RABBITMQ \
  --engine-version 4.2 \
  --host-instance-type mq.m7g.large \
  --deployment-mode SINGLE_INSTANCE \
  --users Username=admin,Password=MyPassword123! \
  --publicly-accessible
```

このコマンドは、M7g インスタンスタイプで RabbitMQ 4.2 ブローカーを作成します。

#### ステップ 2: RabbitMQ JMS クライアントを設定

Java プロジェクトに RabbitMQ JMS クライアントライブラリを追加します。

**Maven の場合**:
```xml
<dependency>
    <groupId>com.rabbitmq.jms</groupId>
    <artifactId>rabbitmq-jms</artifactId>
    <version>3.4.0</version>
</dependency>
```

**Gradle の場合**:
```gradle
implementation 'com.rabbitmq.jms:rabbitmq-jms:3.4.0'
```

#### ステップ 3: JMS アプリケーションを接続

RabbitMQ JMS クライアントを使用して、JMS アプリケーションをブローカーに接続します。

```java
import com.rabbitmq.jms.admin.RMQConnectionFactory;
import javax.jms.*;

public class JMSExample {
    public static void main(String[] args) throws Exception {
        // RabbitMQ JMS 接続ファクトリを作成
        RMQConnectionFactory connectionFactory = new RMQConnectionFactory();
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("MyPassword123!");
        connectionFactory.setVirtualHost("/");
        connectionFactory.setHost("b-xxxx-xxxx-xxxx.mq.us-east-1.amazonaws.com");
        connectionFactory.setPort(5671);
        connectionFactory.setUseSslProtocol(true);

        // 接続とセッションを作成
        Connection connection = connectionFactory.createConnection();
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);

        // 送信先を作成
        Destination queue = session.createQueue("myQueue");

        // メッセージプロデューサーを作成
        MessageProducer producer = session.createProducer(queue);
        TextMessage message = session.createTextMessage("Hello from JMS!");
        producer.send(message);

        System.out.println("Message sent: " + message.getText());

        // リソースをクローズ
        producer.close();
        session.close();
        connection.close();
    }
}
```

このコードは、JMS API を使用して RabbitMQ ブローカーにメッセージを送信します。

## メリット

### ビジネス面

- **既存資産の活用**: 既存の JMS アプリケーションをコード変更なしで RabbitMQ に移行可能
- **段階的移行**: JMS と AMQP の相互運用により、リスクを抑えた段階的な移行戦略を実装可能
- **開発コストの削減**: JMS 標準 API を使用できるため、開発者の学習コストを削減

### 技術面

- **標準準拠**: Java EE および Jakarta EE の JMS 仕様に準拠
- **柔軟なメッセージング**: JMS と AMQP の相互運用により、柔軟なメッセージング戦略を実装
- **プラグインのデフォルト有効化**: 追加設定なしで JMS 機能を利用可能
- **高いパフォーマンス**: Graviton3 ベースの M7g インスタンスで高いパフォーマンスを実現

## デメリット・制約事項

### 制限事項

- RabbitMQ 4.2 以降が必要
- M7g インスタンスタイプが必要
- サーバーセッション、XA トランザクション、一部の JMS 機能はサポートされない
- JMS トピックは RabbitMQ JMS クライアントでサポートされない (AMQP との相互運用時)

### 考慮すべき点

- 既存の JMS アプリケーションが非サポート機能を使用していないか確認すること
- JMS と AMQP の相互運用時のメッセージフォーマットの違いに注意すること
- パフォーマンステストを実施し、ワークロードに適したインスタンスタイプを選択すること

## ユースケース

### ユースケース 1: 既存 JMS アプリケーションの移行

**シナリオ**: オンプレミスの ActiveMQ で動作している JMS アプリケーションを AWS に移行したい

**実装例**:
1. Amazon MQ for RabbitMQ 4.2 ブローカーを作成
2. 既存の JMS アプリケーションの接続設定を RabbitMQ ブローカーのエンドポイントに変更
3. アプリケーションをデプロイして動作確認

**効果**: コード変更なしで JMS アプリケーションを AWS に移行でき、RabbitMQ の高可用性とスケーラビリティを活用

### ユースケース 2: JMS から AMQP への段階的移行

**シナリオ**: JMS ベースのマイクロサービスアーキテクチャを、より軽量な AMQP に段階的に移行したい

**実装例**:
1. 既存の JMS プロデューサーから AMQP エクスチェンジにメッセージを送信
2. 新しいマイクロサービスは AMQP コンシューマーとして実装
3. 段階的に JMS プロデューサーを AMQP プロデューサーに置き換え

**効果**: ダウンタイムなしで JMS から AMQP に移行でき、リスクを最小化

### ユースケース 3: ハイブリッドメッセージングアーキテクチャ

**シナリオ**: レガシーシステムは JMS を使用し、新しいクラウドネイティブアプリケーションは AMQP を使用するハイブリッド環境

**実装例**:
1. RabbitMQ ブローカーを中央メッセージングハブとして配置
2. レガシーシステムは JMS クライアントで接続
3. 新しいアプリケーションは AMQP クライアントで接続
4. 両者間でメッセージを相互運用

**効果**: レガシーシステムと新しいシステムをシームレスに統合し、段階的なモダナイゼーションを実現

## 料金

Amazon MQ for RabbitMQ の料金は、インスタンスタイプ、ストレージ、データ転送に基づきます。JMS サポート自体に追加料金はかかりません。

### 主な料金要素

- **ブローカーインスタンス料金**: インスタンスタイプ (M7g) に基づく時間単位の料金
- **ストレージ料金**: EBS ストレージのサイズに基づく料金
- **データ転送料金**: VPC 外へのデータ転送に対する料金

### 料金例 (US East - N. Virginia リージョン)

| インスタンスタイプ | 月額料金 (概算) |
|-------------------|------------------|
| mq.m7g.large | 約 $200 |
| mq.m7g.xlarge | 約 $400 |
| mq.m7g.2xlarge | 約 $800 |

詳細は [Amazon MQ 料金ページ](https://aws.amazon.com/amazon-mq/pricing/) をご確認ください。

## 利用可能リージョン

この機能は、Amazon MQ for RabbitMQ 4 インスタンスが利用可能なすべての AWS リージョンで利用できます。詳細は [AWS グローバルインフラストラクチャページ](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) をご確認ください。

## 関連サービス・機能

- **Amazon MQ for ActiveMQ**: JMS をネイティブサポートする別の Amazon MQ オプション
- **AWS Lambda**: Amazon MQ からのメッセージをトリガーとするサーバーレス処理
- **Amazon EventBridge**: イベント駆動型アーキテクチャとの統合
- **Amazon CloudWatch**: ブローカーのメトリクスとログの監視
- **AWS Secrets Manager**: ブローカー認証情報の安全な管理

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/amazon-mq-jms-spec-rabbitmq/)
- [Amazon MQ リリースノート](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/amazon-mq-release-notes.html)
- [Amazon MQ 開発者ガイド: RabbitMQ の使用](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/working-with-rabbitmq.html)
- [Amazon MQ for RabbitMQ JMS サポート](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/rabbitmq-jms-support.html)
- [RabbitMQ JMS クライアント](https://www.rabbitmq.com/client-libraries/jms-client)

## まとめ

Amazon MQ for RabbitMQ の JMS サポートにより、既存の JMS アプリケーションをコード変更なしで RabbitMQ に移行できるようになりました。JMS と AMQP の相互運用により、段階的な移行戦略を実装でき、レガシーシステムと新しいクラウドネイティブアプリケーションをシームレスに統合できます。JMS Topic Exchange プラグインがデフォルトで有効化されているため、すぐに利用を開始できます。
