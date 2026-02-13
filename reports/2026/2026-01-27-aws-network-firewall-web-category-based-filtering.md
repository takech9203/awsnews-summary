# AWS Network Firewall - Web カテゴリベースフィルタリング

**リリース日**: 2026 年 1 月 27 日
**サービス**: AWS Network Firewall
**機能**: Web カテゴリベースフィルタリングと GenAI トラフィック可視化

## 概要

AWS Network Firewall は、生成 AI (GenAI) アプリケーショントラフィックの可視化と、Web カテゴリに基づくトラフィックフィルタリングをサポートするようになりました。この新機能により、事前定義された URL カテゴリを使用してファイアウォールルール内で直接 GenAI サービス、ソーシャルメディアプラットフォーム、ストリーミングサイト、その他の Web カテゴリへのアクセスを識別および制御できるようになり、ガバナンスが簡素化されます。

URL カテゴリに基づいてトラフィックを検査するこのアプローチにより、セキュリティおよびコンプライアンスチームは AWS 環境全体で一貫したポリシーを適用しながら、GenAI などの新興技術の使用状況を可視化できます。不適切または高リスクなドメインへのアクセスをブロックし、GenAI ツールの使用を承認されたサービスに制限し、規制要件を満たすことができます。AWS Network Firewall の TLS インスペクション機能と組み合わせることで、カテゴリベースのルールを使用して完全な URL パスを検査し、さらに詳細な制御が可能になります。

**アップデート前の課題**

- GenAI サービスへのアクセスを個別の URL やドメインで管理する必要があり、運用オーバーヘッドが大きかった
- 新しい GenAI サービスが登場するたびにファイアウォールルールを更新する必要があった
- Web カテゴリごとに一貫したポリシーを適用することが困難だった

**アップデート後の改善**

- 事前定義された URL カテゴリを使用して GenAI トラフィックを簡単に識別・制御可能になった
- Web カテゴリ (ソーシャルメディア、ストリーミングなど) ごとに一貫したポリシーを適用できるようになった
- 運用オーバーヘッドが削減され、規制要件への対応が容易になった

## サービスアップデートの詳細

### 主要機能

1. **Web カテゴリベースフィルタリング**
   - 事前定義された URL カテゴリを使用してトラフィックをフィルタリング
   - GenAI、ソーシャルメディア、ストリーミングなどのカテゴリをサポート
   - ファイアウォールルール内で直接カテゴリを指定

2. **GenAI トラフィックの可視化**
   - GenAI アプリケーショントラフィックを識別して可視化
   - 承認された GenAI サービスのみへのアクセスを制限
   - 新興技術の使用状況を監視

3. **TLS インスペクションとの統合**
   - TLS インスペクション機能と組み合わせて完全な URL パスを検査
   - カテゴリベースのルールでより詳細な制御
   - 暗号化されたトラフィックも検査可能

## 技術仕様

### サポートされる URL カテゴリ

| カテゴリ | 説明 |
|---------|------|
| GenAI | 生成 AI サービス (ChatGPT、Claude など) |
| Social Media | ソーシャルメディアプラットフォーム |
| Streaming | ストリーミングサイト |
| その他 | 事前定義されたその他のカテゴリ |

### ルール設定例

```json
{
  "RuleGroup": {
    "RulesSource": {
      "StatefulRules": [
        {
          "Action": "DROP",
          "Header": {
            "Protocol": "HTTP",
            "Direction": "FORWARD"
          },
          "RuleOptions": [
            {
              "Keyword": "url-category",
              "Settings": ["GenAI", "SocialMedia"]
            }
          ]
        }
      ]
    }
  }
}
```

## 設定方法

### 前提条件

1. AWS Network Firewall の設定
2. VPC とサブネットの設定
3. 適切な IAM 権限

### 手順

#### ステップ 1: ステートフルルールグループの更新

AWS Management Console、AWS CLI、または AWS SDK を使用してステートフルルールグループを更新します。

```bash
aws network-firewall update-rule-group \
  --rule-group-name my-rule-group \
  --type STATEFUL \
  --rules file://rules.json
```

#### ステップ 2: URL カテゴリフィルタリングの設定

ルールに URL カテゴリを指定します。

```json
{
  "RuleOptions": [
    {
      "Keyword": "url-category",
      "Settings": ["GenAI"]
    }
  ]
}
```

#### ステップ 3: TLS インスペクションの有効化 (オプション)

より詳細な制御のため、TLS インスペクションを有効化します。

```bash
aws network-firewall update-firewall-policy \
  --firewall-policy-name my-policy \
  --tls-inspection-configuration-arn arn:aws:network-firewall:region:account:tls-inspection-configuration/name
```

## メリット

### ビジネス面

- **ガバナンスの簡素化**: 事前定義されたカテゴリで一貫したポリシーを適用
- **規制要件への対応**: GenAI ツールの使用を承認されたサービスに制限
- **運用オーバーヘッドの削減**: 個別の URL 管理が不要

### 技術面

- **可視化の向上**: GenAI トラフィックを識別して監視
- **詳細な制御**: TLS インスペクションと組み合わせて完全な URL パスを検査
- **柔軟性**: カテゴリベースのルールで新しいサービスに自動対応

## ユースケース

### ユースケース 1: GenAI サービスの制限

**シナリオ**: 企業ポリシーにより、承認された GenAI サービスのみへのアクセスを許可する。

**実装例**:
```json
{
  "Action": "DROP",
  "RuleOptions": [
    {
      "Keyword": "url-category",
      "Settings": ["GenAI"]
    }
  ]
}
```

**効果**: 未承認の GenAI サービスへのアクセスをブロックし、データ漏洩を防止します。

### ユースケース 2: ソーシャルメディアのブロック

**シナリオ**: 業務時間中にソーシャルメディアへのアクセスをブロックする。

**実装例**:
```json
{
  "Action": "DROP",
  "RuleOptions": [
    {
      "Keyword": "url-category",
      "Settings": ["SocialMedia"]
    }
  ]
}
```

**効果**: 生産性を向上させ、セキュリティリスクを削減します。

## 料金

AWS Network Firewall の料金は変更されません。

- ファイアウォールエンドポイント: $0.395 / 時間
- トラフィック処理: $0.065 / GB

## 利用可能リージョン

この機能は、AWS Network Firewall がサポートされているすべての AWS 商用リージョンで利用可能です。

## 関連サービス・機能

- **AWS Network Firewall TLS Inspection**: 暗号化されたトラフィックの検査
- **AWS WAF**: Web アプリケーションファイアウォール
- **Amazon VPC**: Virtual Private Cloud

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/aws-network-firewall-web-category-based-filtering/)
- [AWS Network Firewall 製品ページ](https://aws.amazon.com/network-firewall/)
- [URL カテゴリフィルタリングドキュメント](https://docs.aws.amazon.com/network-firewall/latest/developerguide/rule-groups-url-filtering.html)

## まとめ

AWS Network Firewall の Web カテゴリベースフィルタリングにより、GenAI トラフィックの可視化と制御が簡素化されました。事前定義された URL カテゴリを使用して、GenAI、ソーシャルメディア、ストリーミングなどのトラフィックを簡単に識別・制御でき、運用オーバーヘッドが削減されます。TLS インスペクションと組み合わせることで、より詳細な制御が可能になります。GenAI サービスの使用を管理する必要がある場合は、この新機能を活用してガバナンスを強化することをお勧めします。
