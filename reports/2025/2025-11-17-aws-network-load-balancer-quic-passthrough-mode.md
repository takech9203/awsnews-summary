# AWS Network Load Balancer - QUIC プロトコルサポート

**リリース日**: 2025 年 11 月 17 日  
**サービス**: Elastic Load Balancing  
**機能**: Network Load Balancer の QUIC プロトコルパススルーモード


## 概要

AWS Network Load Balancer (NLB) は、QUIC プロトコルのパススルーモードをサポートしました。この機能により、QUIC Connection ID を使用したセッションスティッキネスで超低レイテンシーのトラフィック転送が可能になります。

モバイルファーストアプリケーションでは、ハンドシェイクの最小化と接続の回復力により、アプリケーションレイテンシーを 25-30% 削減できます。

**アップデート前の課題**

- 以前は NLB で QUIC プロトコルをネイティブにサポートしておらず、QUIC トラフィックの効率的な処理が困難だった
- 以前はモバイルアプリケーションでネットワーク切り替え時に接続が切断され、ユーザー体験が低下していた
- 以前は TCP ベースのプロトコルでハンドシェイクのオーバーヘッドによるレイテンシーが発生していた

**アップデート後の改善**

- 今回のアップデートにより、QUIC プロトコルのパススルーモードがサポートされ、超低レイテンシーのトラフィック転送が可能になった
- 今回のアップデートにより、QUIC Connection ID ベースのセッションスティッキネスでネットワーク切り替え時も接続を維持できるようになった
- 今回のアップデートにより、モバイルファーストアプリケーションでアプリケーションレイテンシーを 25-30% 削減できるようになった


## サービスアップデートの詳細

### 主要機能

1. **QUIC パススルーモード**
   - QUIC トラフィックをターゲットに直接転送
   - TLS 証明書の顧客管理を維持
   - エンドツーエンド暗号化のサポート

2. **セッションスティッキネス**
   - QUIC Connection ID ベースのスティッキネス
   - 接続の継続性を確保
   - モバイルネットワーク切り替え時の回復力

3. **レイテンシー削減**
   - 25-30% のレイテンシー削減
   - ハンドシェイクの最小化
   - 0-RTT 接続再開のサポート


## 技術仕様

### QUIC プロトコルの特徴

| 項目 | 詳細 |
|------|------|
| プロトコル | UDP ベース |
| 暗号化 | TLS 1.3 統合 |
| 多重化 | ストリーム多重化 |
| 接続移行 | Connection ID ベース |

### NLB QUIC サポートの仕様

| 項目 | 詳細 |
|------|------|
| モード | パススルー |
| ポート | UDP 443 (カスタマイズ可能) |
| スティッキネス | Connection ID ベース |
| TLS 終端 | ターゲット側 |


## 設定方法

### 前提条件

1. NLB が作成済み
2. QUIC 対応のターゲットアプリケーション
3. 適切なセキュリティグループ設定

### 手順

#### ステップ 1: UDP リスナーの作成

```bash
aws elbv2 create-listener \
  --load-balancer-arn arn:aws:elasticloadbalancing:us-east-1:123456789012:loadbalancer/net/my-nlb/xxxxx \
  --protocol UDP \
  --port 443 \
  --default-actions Type=forward,TargetGroupArn=arn:aws:elasticloadbalancing:us-east-1:123456789012:targetgroup/my-targets/xxxxx
```

#### ステップ 2: ターゲットグループの設定

```bash
aws elbv2 modify-target-group-attributes \
  --target-group-arn arn:aws:elasticloadbalancing:us-east-1:123456789012:targetgroup/my-targets/xxxxx \
  --attributes Key=stickiness.enabled,Value=true Key=stickiness.type,Value=source_ip
```

このコマンドは、ターゲットグループのスティッキネス設定を有効化します。QUIC トラフィックでは、Connection ID ベースのスティッキネスにより、同じクライアントからのリクエストが同じターゲットに転送されます。


## メリット

### ビジネス面

- **ユーザー体験向上**: 低レイテンシーによる快適な操作
- **モバイル対応**: ネットワーク切り替え時の接続維持
- **競争優位性**: 最新プロトコルの採用

### 技術面

- **低レイテンシー**: 25-30% のレイテンシー削減
- **接続回復力**: Connection ID による接続移行
- **セキュリティ**: エンドツーエンド暗号化の維持


## デメリット・制約事項

### 制限事項

- ターゲットアプリケーションが QUIC をサポートしている必要がある
- 一部の古いクライアントは QUIC をサポートしていない

### 考慮すべき点

- QUIC と HTTP/2 のフォールバック戦略
- ファイアウォールでの UDP 443 の許可


## ユースケース

### ユースケース 1: モバイルアプリケーション

**シナリオ**: モバイルユーザー向けの低レイテンシー API を提供したい

**効果**: QUIC により、ネットワーク切り替え時も接続を維持

### ユースケース 2: リアルタイム通信

**シナリオ**: ビデオ通話やゲームなどのリアルタイムアプリケーション

**効果**: 低レイテンシーと接続回復力で品質を向上

### ユースケース 3: グローバルアプリケーション

**シナリオ**: 世界中のユーザーに高速なサービスを提供

**効果**: QUIC の 0-RTT 接続再開で初期接続を高速化


## 料金

NLB の標準料金が適用されます。QUIC サポートに追加料金はありません。


## 利用可能リージョン

NLB が利用可能なすべてのリージョンで利用可能です。


## 関連サービス・機能

- **Elastic Load Balancing**: ロードバランシングサービス
- **Amazon CloudFront**: CDN サービス (QUIC サポート)
- **AWS Global Accelerator**: グローバルネットワーク最適化


## 参考リンク

- [公式発表](https://aws.amazon.com/about-aws/whats-new/2025/11/aws-network-load-balancer-quic-passthrough-mode/)
- [NLB QUIC ブログ](https://aws.amazon.com/blogs/networking-and-content-delivery/introducing-quic-protocol-support-for-network-load-balancer-accelerating-mobile-first-applications/)


## まとめ

AWS Network Load Balancer の QUIC プロトコルサポートにより、モバイルファーストアプリケーションのパフォーマンスが大幅に向上します。25-30% のレイテンシー削減と接続回復力により、優れたユーザー体験を提供できます。
