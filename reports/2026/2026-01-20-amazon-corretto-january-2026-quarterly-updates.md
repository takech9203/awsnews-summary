# Amazon Corretto - 2026 年 1 月四半期アップデート

**リリース日**: 2026年01月20日
**サービス**: Amazon Corretto
**機能**: 四半期セキュリティおよびクリティカルアップデート

## 概要

2026 年 1 月 20 日、Amazon は Amazon Corretto の Long-Term Supported (LTS) バージョンの OpenJDK に対する四半期セキュリティおよびクリティカルアップデートを発表しました。Corretto 25.0.2、21.0.10、17.0.18、11.0.30、および 8u482 が[ダウンロード](https://aws.amazon.com/corretto/)可能になりました。Amazon Corretto は、無料で、マルチプラットフォームに対応した、本番環境対応の OpenJDK ディストリビューションです。

**アップデート前の課題**

- 以前のバージョンにはセキュリティ脆弱性が存在していた可能性がある
- クリティカルな問題が修正されていなかった
- 最新の Java 機能とパフォーマンス改善が含まれていなかった

**アップデート後の改善**

- セキュリティ脆弱性が修正され、より安全な Java 実行環境を提供
- クリティカルな問題が解決され、安定性が向上
- 最新の Java 機能とパフォーマンス改善が含まれる

## サービスアップデートの詳細

### 主要機能

1. **セキュリティアップデート**
   - 既知のセキュリティ脆弱性を修正
   - セキュリティベストプラクティスに準拠
   - 本番環境での安全な使用をサポート

2. **クリティカルアップデート**
   - 重要なバグ修正を含む
   - 安定性とパフォーマンスの向上
   - 本番環境での信頼性を強化

3. **複数の LTS バージョンをサポート**
   - Corretto 25.0.2 (最新 LTS)
   - Corretto 21.0.10
   - Corretto 17.0.18
   - Corretto 11.0.30
   - Corretto 8u482 (Java 8)

## 技術仕様

### 更新されたバージョン

| バージョン | アップデート | サポート状況 |
|-----------|------------|------------|
| Corretto 25 | 25.0.2 | LTS |
| Corretto 21 | 21.0.10 | LTS |
| Corretto 17 | 17.0.18 | LTS |
| Corretto 11 | 11.0.30 | LTS |
| Corretto 8 | 8u482 | LTS |

### 対応プラットフォーム

- Linux (x86_64、aarch64)
- Windows (x86_64)
- macOS (x86_64、aarch64)
- Docker コンテナイメージ

## 設定方法

### 前提条件

1. Java アプリケーションまたは開発環境
2. 適切なプラットフォーム (Linux、Windows、macOS)
3. 管理者権限 (インストールに必要)

### 手順

#### ステップ1: Corretto のダウンロード

[Corretto home page](https://aws.amazon.com/corretto) から適切なバージョンをダウンロードします。

#### ステップ2: インストール

ダウンロードしたインストーラーを実行し、指示に従ってインストールします。

#### ステップ3: Linux での apt/yum/apk リポジトリの設定 (オプション)

Linux システムでは、Corretto の apt、yum、または apk リポジトリを設定することで、自動アップデートを受け取ることができます。

```bash
# apt の例 (Debian/Ubuntu)
wget -O- https://apt.corretto.aws/corretto.key | sudo apt-key add -
sudo add-apt-repository 'deb https://apt.corretto.aws stable main'
sudo apt-get update
sudo apt-get install -y java-25-amazon-corretto-jdk
```

詳細な手順は [Corretto Linux installation guide](https://docs.aws.amazon.com/corretto/latest/corretto-25-ug/generic-linux-install.html) を参照してください。

#### ステップ4: バージョンの確認

インストール後、Java バージョンを確認します。

```bash
java -version
```

## メリット

### ビジネス面

- **無料**: ライセンス費用なしで商用利用可能
- **本番環境対応**: AWS が本番環境での使用をサポート
- **長期サポート (LTS)**: 複数の LTS バージョンでセキュリティアップデートを継続提供

### 技術面

- **セキュリティ**: 定期的なセキュリティアップデートで脆弱性を修正
- **安定性**: クリティカルなバグ修正により安定性が向上
- **マルチプラットフォーム**: Linux、Windows、macOS をサポート

## デメリット・制約事項

### 制限事項

- 特定の商用 Java ディストリビューションの独自機能は含まれない
- サポートは AWS のサポートチャネルを通じて提供される

### 考慮すべき点

- アプリケーションの互換性を確認する必要がある
- バージョンアップグレードにはテストが推奨される
- 既存の Java 環境からの移行を計画する必要がある

## ユースケース

### ユースケース1: 本番環境での Java アプリケーション実行

**シナリオ**: EC2 インスタンスで Java アプリケーションを実行

**実装例**:
- EC2 インスタンスに Corretto 21 をインストール
- Java アプリケーションを Corretto で実行
- 定期的に四半期アップデートを適用

**効果**: 無料で本番環境対応の Java ランタイムを使用し、セキュリティアップデートを継続的に受け取る

### ユースケース2: コンテナ化された Java アプリケーション

**シナリオ**: Docker コンテナで Java マイクロサービスを実行

**実装例**:
```dockerfile
FROM amazoncorretto:21
COPY target/myapp.jar /app.jar
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

**効果**: 軽量で安全な Corretto ベースイメージを使用して Java アプリケーションをコンテナ化

### ユースケース3: Lambda 関数での Java 実行

**シナリオ**: AWS Lambda で Java 関数を実行

**実装例**:
- Lambda カスタムランタイムで Corretto を使用
- または Corretto ベースの Lambda レイヤーを作成
- 定期的にアップデートされた Corretto バージョンを使用

**効果**: サーバーレス環境で最新のセキュリティアップデートを適用した Java を実行

## 料金

Amazon Corretto は完全に無料で、ライセンス費用は発生しません。

## 利用可能リージョン

Amazon Corretto は、すべての AWS リージョンおよびオンプレミス環境で利用可能です。

## 関連サービス・機能

- **AWS Lambda**: サーバーレス Java 関数の実行
- **Amazon EC2**: Java アプリケーションのホスティング
- **Amazon ECS/EKS**: コンテナ化された Java アプリケーションの実行
- **AWS CodeBuild**: Corretto を使用した Java アプリケーションのビルド

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/amazon-corretto-january-2026-quarterly-updates/)
- [Corretto home page](https://aws.amazon.com/corretto)
- [Corretto downloads](https://aws.amazon.com/corretto/)
- [Corretto Linux installation guide](https://docs.aws.amazon.com/corretto/latest/corretto-25-ug/generic-linux-install.html)
- [GitHub - Corretto](https://github.com/corretto)

## まとめ

Amazon Corretto の 2026 年 1 月四半期アップデートにより、複数の LTS バージョンでセキュリティおよびクリティカルな問題が修正されました。本番環境で Java アプリケーションを実行している組織は、この無料のアップデートを適用してセキュリティと安定性を向上させてください。定期的な四半期アップデートにより、Corretto は本番環境で安全に使用できる Java ディストリビューションとして継続的にサポートされています。
