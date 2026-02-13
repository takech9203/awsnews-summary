# Amazon Lightsail - Node.js、LAMP、Ruby on Rails ブループリント更新

**リリース日**: 2026 年 1 月 26 日
**サービス**: Amazon Lightsail
**機能**: IMDSv2 強制および IPv6 対応の新しいブループリント

## 概要

Amazon Lightsail が Node.js、LAMP (Linux、Apache、MySQL、PHP)、Ruby on Rails の新しいブループリントを提供開始しました。これらの新ブループリントは、Instance Metadata Service Version 2 (IMDSv2) がデフォルトで強制され、IPv6 専用インスタンスをサポートしています。

このアップデートにより、セキュリティのベストプラクティスである IMDSv2 が自動的に適用され、モダンなネットワーキング要件である IPv6 にも対応した環境で、Web アプリケーションを迅速に構築できるようになりました。わずか数クリックで、Node.js、LAMP、または Ruby on Rails がプリインストールされた Lightsail 仮想プライベートサーバー (VPS) を作成できます。

Lightsail では、ブループリント (事前構成されたアプリケーションスタック) とインスタンスバンドル (コンピューティング、ストレージ、月間データ転送量) を選択するだけで、クラウド上でのアプリケーション構築を簡単に開始できます。これらの新ブループリントは、Lightsail が利用可能なすべての AWS リージョンで提供されています。

**アップデート前の課題**

- 既存のブループリントでは IMDSv1 が有効で、SSRF 攻撃などのセキュリティリスクが存在していた
- IPv6 専用インスタンスのサポートが限定的で、モダンなネットワーク要件に対応できなかった
- 開発者が手動で IMDSv2 を有効化し、IPv6 を設定する必要があり、初期セットアップに時間がかかった

**アップデート後の改善**

- IMDSv2 がデフォルトで強制され、SSRF 攻撃などのセキュリティリスクが軽減された
- IPv6 専用インスタンスをサポートし、IPv4 アドレス枯渇問題に対応できるようになった
- セキュリティとモダンネットワーキングのベストプラクティスが自動的に適用され、開発者の負担が軽減された

## サービスアップデートの詳細

### 主要機能

1. **IMDSv2 のデフォルト強制**
   - Instance Metadata Service Version 2 がデフォルトで有効
   - セッションベースの認証により、SSRF (Server-Side Request Forgery) 攻撃を防止
   - メタデータへのアクセスに 2 段階のトークン取得プロセスを要求
   - AWS のセキュリティベストプラクティスに自動的に準拠

2. **IPv6 専用インスタンスのサポート**
   - IPv6 専用ネットワーキングをサポート
   - IPv4 アドレス不要で VPS を起動可能
   - デュアルスタック (IPv4 + IPv6) 構成も引き続きサポート
   - モダンなクラウドネイティブアプリケーションの要件に対応

3. **更新されたブループリント**
   - **Node.js**: 最新の Node.js ランタイムがプリインストール、モダン JavaScript/TypeScript アプリケーション開発に最適
   - **LAMP**: Linux、Apache、MySQL、PHP スタック、従来型 Web アプリケーションに最適
   - **Ruby on Rails**: Ruby on Rails フレームワーク、Ruby ベースの Web アプリケーション開発に最適

## 技術仕様

### サポートされるブループリント

| ブループリント | 説明 | 主な用途 |
|--------------|------|---------|
| Node.js | 最新の Node.js ランタイム環境 | RESTful API、リアルタイムアプリ、マイクロサービス |
| LAMP | Linux + Apache + MySQL + PHP | WordPress、Drupal、従来型 Web アプリ |
| Ruby on Rails | Ruby on Rails フレームワーク | MVC アプリ、SaaS プラットフォーム、スタートアップ MVP |

### IMDSv2 の特徴

| 項目 | IMDSv1 | IMDSv2 (新ブループリント) |
|------|--------|------------------------|
| 認証方式 | なし (直接アクセス) | セッションベース (トークン必須) |
| SSRF 攻撃対策 | 脆弱 | 保護あり |
| リクエスト方式 | GET リクエストのみ | PUT でトークン取得 → GET でメタデータ取得 |
| TTL | なし | トークンに TTL あり (最大 6 時間) |

### IPv6 サポート

| 機能 | 詳細 |
|------|------|
| IPv6 専用インスタンス | サポート (IPv4 アドレス不要) |
| デュアルスタック | サポート (IPv4 + IPv6 同時利用) |
| IPv6 ファイアウォール | 自動的に利用可能 |
| DNS AAAA レコード | 手動設定が必要 (ドメイン使用時) |

## 設定方法

### 前提条件

1. AWS アカウント
2. Lightsail へのアクセス権限

### 手順

#### ステップ 1: Lightsail コンソールでインスタンスを作成

```bash
# AWS CLI で Lightsail インスタンスを作成 (Node.js ブループリント例)
aws lightsail create-instances \
  --instance-names my-nodejs-app \
  --availability-zone us-east-1a \
  --blueprint-id nodejs \
  --bundle-id nano_3_0 \
  --ip-address-type ipv6
```

このコマンドは、IMDSv2 が強制され、IPv6 対応の Node.js インスタンスを作成します。

#### ステップ 2: インスタンスへの接続

```bash
# Lightsail ブラウザベース SSH 経由で接続 (コンソールから "Connect using SSH" をクリック)
# または SSH キーを使用して接続
ssh -i ~/.ssh/lightsail-key.pem bitnami@[インスタンスのIPv6アドレス]
```

#### ステップ 3: IMDSv2 の動作確認

```bash
# トークンを取得 (IMDSv2)
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" \
  -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")

# メタデータにアクセス
curl -H "X-aws-ec2-metadata-token: $TOKEN" \
  http://169.254.169.254/latest/meta-data/instance-id
```

このコマンドで IMDSv2 が正しく動作していることを確認します。IMDSv1 スタイルのリクエスト (トークンなし) は拒否されます。

#### ステップ 4: IPv6 ファイアウォールの設定

```bash
# Lightsail コンソールまたは CLI でファイアウォールルールを追加
aws lightsail put-instance-public-ports \
  --instance-name my-nodejs-app \
  --port-infos fromPort=443,toPort=443,protocol=tcp,ipv6Cidrs=['::/0']
```

このコマンドは、IPv6 トラフィックに対して HTTPS (ポート 443) を開放します。

## メリット

### ビジネス面

- **セキュリティ強化**: IMDSv2 強制により、SSRF 攻撃のリスクが大幅に軽減され、コンプライアンス要件を満たしやすくなります
- **将来対応**: IPv6 サポートにより、IPv4 アドレス枯渇問題に対応し、将来のネットワーク要件に備えられます
- **迅速な開発**: プリインストールされたスタックにより、開発チームは即座にアプリケーション開発を開始できます

### 技術面

- **自動化されたセキュリティ**: 手動で IMDSv2 を設定する必要がなく、デフォルトでベストプラクティスが適用されます
- **ネットワークの柔軟性**: IPv6 専用、IPv4 専用、デュアルスタックから選択可能で、ユースケースに応じた構成ができます
- **シンプルな管理**: Lightsail の統合管理により、インスタンス、ストレージ、ネットワーキングを一元管理できます

## デメリット・制約事項

### 制限事項

- IMDSv2 を必要としないレガシーアプリケーションでは、コード修正が必要な場合があります
- IPv6 専用インスタンスは、IPv4 のみをサポートする外部サービスと直接通信できません
- 一部の古いライブラリやツールは IMDSv2 に対応していない可能性があります

### 考慮すべき点

- 既存のアプリケーションを新ブループリントに移行する場合、IMDSv2 互換性をテストする必要があります
- IPv6 専用インスタンスを使用する場合、DNS に AAAA レコードを手動で設定する必要があります
- 外部 API が IPv4 のみの場合、NAT64/DNS64 または IPv4 接続が必要になる可能性があります

## ユースケース

### ユースケース 1: セキュアな Node.js API サーバー

**シナリオ**: スタートアップ企業が、セキュリティベストプラクティスに準拠した RESTful API をすばやく構築したい

**実装例**:
```bash
# Node.js ブループリントでインスタンスを作成
aws lightsail create-instances \
  --instance-names secure-api-server \
  --availability-zone us-west-2a \
  --blueprint-id nodejs \
  --bundle-id small_3_0

# SSH でインスタンスに接続し、Express.js アプリをデプロイ
ssh bitnami@[インスタンスIP]
cd /opt/bitnami/projects
npm init -y
npm install express
node index.js
```

**効果**: IMDSv2 により、メタデータへの不正アクセスを防ぎ、セキュアな API 環境を即座に構築できます

### ユースケース 2: IPv6 ネイティブな Web アプリケーション

**シナリオ**: 最新のネットワーク標準に準拠し、IPv6 ネイティブな LAMP ベースの Web サイトを構築したい

**実装例**:
```bash
# IPv6 専用 LAMP インスタンスを作成
aws lightsail create-instances \
  --instance-names ipv6-lamp-site \
  --availability-zone eu-west-1a \
  --blueprint-id lamp_8 \
  --bundle-id micro_3_0 \
  --ip-address-type ipv6

# DNS に AAAA レコードを追加
# example.com -> [IPv6アドレス]
```

**効果**: IPv4 アドレスを消費せず、モダンなネットワーク環境でコスト効率よく Web サイトをホストできます

### ユースケース 3: Ruby on Rails スタートアップ MVP

**シナリオ**: スタートアップが MVP (Minimum Viable Product) を迅速に構築し、セキュリティとスケーラビリティを確保したい

**実装例**:
```bash
# Ruby on Rails ブループリントでインスタンスを作成
aws lightsail create-instances \
  --instance-names mvp-rails-app \
  --availability-zone ap-northeast-1a \
  --blueprint-id ruby \
  --bundle-id medium_3_0

# Rails アプリをデプロイ
ssh bitnami@[インスタンスIP]
cd /opt/bitnami/projects
rails new myapp
cd myapp
rails server -b 0.0.0.0
```

**効果**: IMDSv2 によりセキュリティが強化され、Rails アプリを迅速に本番環境にデプロイできます

## 料金

Lightsail の既存の料金体系が適用されます。IMDSv2 および IPv6 サポートに対する追加料金はありません。

### 料金例

| インスタンスバンドル | vCPU | RAM | ストレージ | データ転送 | 月額料金 |
|------------------|------|-----|-----------|----------|---------|
| Nano | 0.5 | 512 MB | 20 GB SSD | 1 TB | $3.50 |
| Micro | 1 | 1 GB | 40 GB SSD | 2 TB | $5 |
| Small | 1 | 2 GB | 60 GB SSD | 3 TB | $10 |
| Medium | 2 | 4 GB | 80 GB SSD | 4 TB | $20 |

*料金は米国東部 (バージニア北部) リージョンの例です。リージョンにより異なる場合があります。

## 利用可能リージョン

これらの新ブループリントは、Lightsail が利用可能なすべての AWS リージョンで提供されています。

## 関連サービス・機能

- **Amazon EC2**: より高度な制御とカスタマイズが必要な場合の選択肢
- **AWS Elastic Beanstalk**: アプリケーションのデプロイとスケーリングを自動化
- **Amazon RDS for Lightsail**: マネージドデータベースサービス
- **Lightsail CDN**: グローバルコンテンツ配信ネットワーク

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/amazon-lightsail-nodejs-lamp-and-ruby-on-rails/)
- [ドキュメント - Lightsail ブループリント](https://lightsail.aws.amazon.com/ls/docs/en_us/articles/compare-options-choose-lightsail-instance-image)
- [ドキュメント - IPv6 互換ブループリント](https://docs.aws.amazon.com/lightsail/latest/userguide/ipv6-only-blueprints.html)
- [ドキュメント - IPv6 有効化](https://docs.aws.amazon.com/lightsail/latest/userguide/enable-ipv6.html)
- [Lightsail 料金ページ](https://aws.amazon.com/lightsail/pricing/)

## まとめ

Amazon Lightsail の Node.js、LAMP、Ruby on Rails 新ブループリントは、IMDSv2 強制と IPv6 サポートにより、セキュリティとモダンネットワーキングのベストプラクティスを自動的に適用します。開発者は、セキュリティ設定に時間を費やすことなく、即座にアプリケーション開発を開始できます。IPv6 対応により、将来のネットワーク要件にも対応し、IPv4 アドレス枯渇問題を回避できます。スタートアップから中小企業まで、迅速で安全な Web アプリケーション開発を実現する理想的なソリューションです。
