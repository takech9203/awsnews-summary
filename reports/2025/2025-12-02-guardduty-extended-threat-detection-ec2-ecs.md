# Amazon GuardDuty - Extended Threat Detection for EC2 and ECS

**リリース日**: 2025 年 12 月 2 日  
**サービス**: Amazon GuardDuty  
**機能**: Extended Threat Detection for Amazon EC2 and Amazon ECS


## 概要

AWS は Amazon GuardDuty の Extended Threat Detection を Amazon EC2 と Amazon ECS に拡張しました。この機能により、仮想マシンとコンテナ環境全体で統一された可視性が提供され、同じアプリケーションをサポートする多様な AWS ワークロードにまたがる複雑なマルチステージ攻撃を特定できるようになります。

Extended Threat Detection は、複数のセキュリティシグナルを相関させ、攻撃チェーン全体を可視化することで、セキュリティチームがより迅速かつ効果的に脅威に対応できるよう支援します。

**アップデート前の課題**

- EC2 と ECS の脅威検出が個別に行われ、攻撃の全体像を把握することが困難だった
- マルチステージ攻撃の検出には、複数のアラートを手動で相関させる必要があった
- コンテナと仮想マシンにまたがる攻撃パターンの特定が困難だった

**アップデート後の改善**

- EC2 と ECS 全体で統一された脅威検出と可視性を提供
- マルチステージ攻撃を自動的に検出し、攻撃チェーンを可視化
- 複数のセキュリティシグナルを相関させ、より正確な脅威検出を実現


## サービスアップデートの詳細

### 主要機能

1. **統一された脅威検出**
   - EC2 インスタンスと ECS タスク/サービス全体での脅威検出
   - 同じアプリケーションをサポートするワークロード間の相関分析
   - ハイブリッド環境での一貫したセキュリティ監視

2. **マルチステージ攻撃の検出**
   - 攻撃チェーン全体の自動検出
   - 初期アクセスから横展開、データ流出までの追跡
   - 攻撃の進行状況をタイムラインで可視化

3. **高度な相関分析**
   - 複数のセキュリティシグナルを自動的に相関
   - 誤検知の削減と検出精度の向上
   - コンテキストに基づいた優先度付け

4. **統合されたアラート**
   - 関連するイベントを単一のアラートに統合
   - 調査に必要な情報を一元的に提供
   - Security Hub との統合による一元管理


## 技術仕様

### 検出対象の脅威タイプ

| 脅威タイプ | EC2 | ECS | 説明 |
|-----------|-----|-----|------|
| 初期アクセス | ✅ | ✅ | 不正なアクセス試行の検出 |
| 権限昇格 | ✅ | ✅ | 権限の不正な昇格の検出 |
| 横展開 | ✅ | ✅ | ネットワーク内での横方向の移動 |
| データ流出 | ✅ | ✅ | 機密データの不正な転送 |
| 暗号通貨マイニング | ✅ | ✅ | 不正なマイニング活動 |
| コマンド&コントロール | ✅ | ✅ | C2 サーバーとの通信 |

### 攻撃チェーンの可視化

| 段階 | 検出内容 |
|------|---------|
| 偵察 | ポートスキャン、脆弱性スキャン |
| 初期アクセス | 不正ログイン、エクスプロイト |
| 永続化 | バックドア、スケジュールタスク |
| 権限昇格 | 特権アカウントへのアクセス |
| 横展開 | 他のリソースへの移動 |
| データ流出 | 機密データの転送 |


## 設定方法

### 前提条件

1. AWS アカウント
2. Amazon GuardDuty の有効化
3. EC2 および/または ECS ワークロード

### 手順

#### ステップ 1: GuardDuty の有効化

```bash
# AWS CLI で GuardDuty を有効化
aws guardduty create-detector --enable
```

#### ステップ 2: Extended Threat Detection の有効化

```bash
# Extended Threat Detection を有効化
aws guardduty update-detector \
    --detector-id <detector-id> \
    --features '[{"Name":"RUNTIME_MONITORING","Status":"ENABLED","AdditionalConfiguration":[{"Name":"ECS_FARGATE_AGENT_MANAGEMENT","Status":"ENABLED"}]}]'
```

#### ステップ 3: 検出結果の確認

```bash
# 検出結果を取得
aws guardduty list-findings \
    --detector-id <detector-id> \
    --finding-criteria '{"Criterion":{"type":{"Eq":["Recon:EC2/PortProbeUnprotectedPort"]}}}'
```


## メリット

### ビジネス面

- **リスク軽減**: マルチステージ攻撃の早期検出により、被害を最小化
- **コンプライアンス**: 統一された脅威検出でコンプライアンス要件に対応
- **運用効率**: 自動化された相関分析により、セキュリティチームの負担を軽減

### 技術面

- **統一された可視性**: EC2 と ECS 全体での一貫した脅威検出
- **高精度な検出**: 相関分析による誤検知の削減
- **迅速な対応**: 攻撃チェーンの可視化による効率的な調査


## デメリット・制約事項

### 制限事項

- Runtime Monitoring の有効化が必要
- 一部の古いインスタンスタイプでは機能が制限される場合がある
- 大規模環境では追加のコストが発生

### 考慮すべき点

- 適切なアラートしきい値の設定が重要
- セキュリティチームのトレーニングが必要
- 他のセキュリティツールとの統合を検討


## ユースケース

### ユースケース 1: ハイブリッドアプリケーションの保護

**シナリオ**: EC2 と ECS の両方で構成されるマイクロサービスアプリケーション

**実装例**:
```
Extended Threat Detection を有効化:
- EC2 上のレガシーコンポーネントを監視
- ECS 上のコンテナ化されたサービスを監視
- 両環境にまたがる攻撃を検出
```

**効果**: アプリケーション全体のセキュリティ態勢を統一的に監視

### ユースケース 2: ランサムウェア攻撃の早期検出

**シナリオ**: ランサムウェア攻撃の初期段階での検出と対応

**実装例**:
```
攻撃チェーンの検出:
1. 初期アクセス: 不正なログイン試行を検出
2. 横展開: 他のインスタンスへの移動を検出
3. データ暗号化: 異常なファイル操作を検出
4. 自動アラート: Security Hub に通知
```

**効果**: ランサムウェアの展開前に攻撃を検出し、被害を防止

### ユースケース 3: コンプライアンス監査

**シナリオ**: PCI DSS や HIPAA などのコンプライアンス要件への対応

**実装例**:
```
統一された監視とレポート:
- EC2 と ECS 全体の脅威検出ログを収集
- Security Hub でコンプライアンスダッシュボードを作成
- 監査レポートを自動生成
```

**効果**: コンプライアンス監査の効率化と証跡の一元管理


## 料金

Extended Threat Detection は、GuardDuty の Runtime Monitoring の一部として提供されます。料金は分析されたイベント数に基づきます。

詳細は [Amazon GuardDuty 料金ページ](https://aws.amazon.com/guardduty/pricing/) を参照してください。


## 利用可能リージョン

Extended Threat Detection は、Amazon GuardDuty が利用可能なすべてのリージョンで利用できます。


## 関連サービス・機能

- **AWS Security Hub**: セキュリティアラートの一元管理
- **Amazon Detective**: セキュリティ調査の支援
- **AWS CloudTrail**: API アクティビティの記録
- **Amazon EventBridge**: 自動対応のトリガー


## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/12/guardduty-extended-threat-detection-ec2-ecs/)
- [AWS Blog](https://aws.amazon.com/blogs/aws/amazon-guardduty-adds-extended-threat-detection-for-amazon-ec2-and-amazon-ecs)
- [Amazon GuardDuty](https://aws.amazon.com/guardduty/)
- [GuardDuty ドキュメント](https://docs.aws.amazon.com/guardduty/)


## まとめ

Amazon GuardDuty の Extended Threat Detection for EC2 and ECS は、仮想マシンとコンテナ環境全体で統一された脅威検出を提供します。マルチステージ攻撃の自動検出、攻撃チェーンの可視化、高度な相関分析により、セキュリティチームはより迅速かつ効果的に脅威に対応できます。re:Invent 2025 で発表されたこの機能は、ハイブリッド環境のセキュリティを大幅に強化します。
