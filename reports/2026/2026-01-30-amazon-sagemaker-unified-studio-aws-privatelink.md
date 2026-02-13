# Amazon SageMaker Unified Studio - AWS PrivateLink サポート

**リリース日**: 2026 年 1 月 30 日
**サービス**: Amazon SageMaker Unified Studio
**機能**: AWS PrivateLink サポート

## 概要

Amazon SageMaker Unified Studio が AWS PrivateLink をサポートするようになり、顧客の VPC と SageMaker Unified Studio 間の接続をパブリックインターネット経由でデータを送信しない方法で確立できるようになりました。この機能により、ネットワーク管理者は AWS ネットワーク内に含まれたまま VPC サービスエンドポイントをオンボーディングでき、IAM ポリシーにより顧客データが AWS ネットワーク内に留まるように強制できます。

セキュリティが重要な環境や規制要件がある組織にとって、これはデータの流出リスクを大幅に低減します。HTTPS/TLS 2 標準データ転送プロトコルを超えて必要とする顧客は、データ転送が AWS ネットワーク内に留まるように VPC を設定できます。

**アップデート前の課題**

- データが公開インターネット経由で送信される可能性があり、セキュリティ要件を満たさない組織がいた
- 厳格なネットワーク要件や規制要件を持つ顧客は SageMaker Unified Studio の採用に制限があった
- VPC から SageMaker Unified Studio への接続時にネットワークレベルでのセキュリティコントロールが限定的だった

**アップデート後の改善**

- AWS PrivateLink を介して VPC から SageMaker への接続が可能に、すべてのデータが AWS ネットワーク内に留まる
- IAM ポリシーでネットワーク分離を強制できるようになり、セキュリティ体制が大幅に向上
- ネットワーク管理者が VPC サービスエンドポイントを設定管理でき、より細かいアクセス制御が実現可能

## サービスアップデートの詳細

### 主要機能

1. **AWS PrivateLink による接続**
   - VPC から SageMaker Unified Studio への非公開接続
   - すべてのデータが AWS ネットワーク内に留まる
   - HTTPS/TLS 2 以上のセキュアプロトコル利用

2. **IAM ポリシーによる強制**
   - SageMaker IAM ポリシーでデータが AWS ネットワーク内に留まるよう強制
   - 顧客データの流出防止を技術的に保証

3. **VPC サービスエンドポイント**
   - ネットワーク管理者が VPC 内にエンドポイントをオンボーディング可能
   - 既存の VPC アーキテクチャと統合可能

## 技術仕様

### 対応リージョン

| リージョン | 利用可能 |
|-----------|---------|
| Asia Pacific (Tokyo) | ✓ |
| Europe (Ireland) | ✓ |
| US East (N. Virginia) | ✓ |
| US East (Ohio) | ✓ |
| US West (Oregon) | ✓ |
| Europe (Frankfurt) | ✓ |
| South America (São Paulo) | ✓ |
| Asia Pacific (Seoul) | ✓ |
| Europe (London) | ✓ |
| Asia Pacific (Singapore) | ✓ |
| Asia Pacific (Sydney) | ✓ |
| Canada (Central) | ✓ |
| Asia Pacific (Mumbai) | ✓ |
| Europe (Paris) | ✓ |
| Europe (Stockholm) | ✓ |

## メリット

### セキュリティ面

- **データ流出防止**: すべてのデータが AWS ネットワーク内に留まり、パブリックインターネット経由の流出がない
- **コンプライアンス強化**: 厳格なネットワークセキュリティ要件を満たしやすくなる
- **アクセス制御**: VPC レベルでのネットワークセキュリティグループと組み合わせた多層防御が可能

### 運用面

- **ネットワーク管理**: 既存の VPC アーキテクチャに統合でき、運用負担が少ない
- **コスト効率**: AWS PrivateLink の利用でデータ転送コストの最適化が可能

## 制限事項

- AWS PrivateLink の料金が適用される (VPC エンドポイント時間単位と転送量に基づく)
- セットアップに ネットワーク管理者の関与が必要

## ユースケース

### ユースケース1: 規制対応環境での ML 開発

**シナリオ**: 金融機関や医療機関で機械学習モデルを開発する場合、データが AWS ネットワーク内に留まる必要がある。

**効果**: 規制要件を満たしながら SageMaker Unified Studio の全機能を利用可能

### ユースケース2: エンタープライズセキュリティ要件の厳しい組織

**シナリオ**: 大企業のセキュリティポリシーがパブリックインターネット経由のデータ転送を禁止している。

**効果**: VPC 内での完全な閉鎖ネットワークで ML 開発が実現

## 利用可能リージョン

Amazon SageMaker Unified Studio がサポートされているすべてのリージョン (15 リージョン以上)

## 関連サービス・機能

- **AWS PrivateLink**: VPC と AWS サービス間の非公開接続
- **Amazon VPC**: ネットワーク分離の基盤
- **IAM ポリシー**: アクセス制御とセキュリティ強制

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/amazon-sagemaker-unified-studio-aws-privatelink/)
- [ネットワーク分離ドキュメント](https://docs.aws.amazon.com/sagemaker-unified-studio/latest/adminguide/network-isolation.html)
- [AWS PrivateLink ドキュメント](https://docs.aws.amazon.com/vpc/latest/privatelink/what-is-privatelink.html)
- [Amazon SageMaker](https://aws.amazon.com/sagemaker/)

## まとめ

AWS PrivateLink のサポートにより、SageMaker Unified Studio は規制環境や高いセキュリティ要件を持つ企業に適した選択肢となりました。データが確実に AWS ネットワーク内に留まるため、セキュリティとコンプライアンスの懸念を軽減しながら、エンタープライズレベルの ML 開発環境を構築できます。
