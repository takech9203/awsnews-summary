# AWS Storage Gateway - Nutanix AHV ハイパーバイザーサポート

**リリース日**: 2025 年 12 月 22 日
**サービス**: AWS Storage Gateway
**機能**: Nutanix AHV ハイパーバイザーサポート

## 概要

AWS Storage Gateway が Nutanix AHV ハイパーバイザーをデプロイオプションとしてサポートしました。S3 File Gateway、Tape Gateway、Volume Gateway を Nutanix AHV ベースのオンプレミスインフラストラクチャにデプロイできるようになります。

Nutanix AHV (Acropolis Hypervisor) は、Nutanix ハイパーコンバージドインフラストラクチャ (HCI) ソリューションに統合された KVM ベースの仮想化プラットフォームです。

**アップデート前の課題**

- Nutanix AHV 環境では Storage Gateway を直接デプロイできなかった
- Nutanix ユーザーは別のハイパーバイザーを用意する必要があった
- ハイブリッドクラウドストレージの選択肢が限られていた

**アップデート後の改善**

- Nutanix AHV 上に Storage Gateway を直接デプロイ可能
- 既存の Nutanix インフラストラクチャを活用してクラウドストレージにアクセス
- VMware ESXi、Microsoft Hyper-V、Linux KVM に加えて新しいデプロイオプション

## サービスアップデートの詳細

### 主要機能

1. **Nutanix AHV サポート**
   - S3 File Gateway のデプロイ
   - Tape Gateway のデプロイ
   - Volume Gateway のデプロイ
   - KVM ベースの仮想化プラットフォームとの互換性

2. **ハイブリッドクラウドストレージ**
   - オンプレミスアプリケーションから事実上無制限のクラウドストレージにアクセス
   - NFS、SMB、iSCSI、iSCSI-VTL インターフェースをサポート
   - AWS へのバックアップとアーカイブ

3. **デプロイオプションの拡張**
   - VMware ESXi
   - Microsoft Hyper-V
   - Linux KVM
   - Nutanix AHV (新規追加)
   - Amazon EC2

## 技術仕様

### サポートされるゲートウェイタイプ

| ゲートウェイタイプ | プロトコル | ユースケース |
|------------------|-----------|-------------|
| S3 File Gateway | NFS, SMB | ファイル共有、クラウドバックアップ |
| Tape Gateway | iSCSI-VTL | テープバックアップの置き換え |
| Volume Gateway | iSCSI | ブロックストレージ、DR |

### デプロイ要件

| 項目 | 要件 |
|------|------|
| ハイパーバイザー | Nutanix AHV |
| ネットワーク | AWS への接続 |
| ストレージ | ローカルキャッシュ用ディスク |

## 設定方法

### 前提条件

1. AWS アカウント
2. Nutanix AHV 環境
3. AWS への ネットワーク接続
4. 適切な IAM 権限

### 手順

#### ステップ 1: Storage Gateway イメージのダウンロード

AWS Management Console から Nutanix AHV 用の Storage Gateway イメージをダウンロードします。

1. Storage Gateway コンソールにアクセス
2. 「ゲートウェイの作成」を選択
3. ホストプラットフォームとして「Nutanix AHV」を選択
4. イメージをダウンロード

#### ステップ 2: Nutanix AHV への仮想マシンデプロイ

```bash
# Nutanix Prism からイメージをアップロード
# 仮想マシンを作成し、ダウンロードしたイメージを使用
```

Nutanix Prism コンソールを使用して、ダウンロードしたイメージから仮想マシンを作成します。

#### ステップ 3: ゲートウェイのアクティベーション

```bash
# ゲートウェイの IP アドレスを使用してアクティベーション
aws storagegateway activate-gateway \
  --activation-key <activation-key> \
  --gateway-name "my-nutanix-gateway" \
  --gateway-timezone "GMT+9:00" \
  --gateway-region ap-northeast-1 \
  --gateway-type FILE_S3
```

デプロイした仮想マシンのアクティベーションキーを使用して、ゲートウェイを AWS にアクティベートします。

## メリット

### ビジネス面

- **既存投資の活用**: Nutanix HCI インフラストラクチャを活用
- **コスト最適化**: 追加のハイパーバイザーライセンス不要
- **運用統合**: 既存の Nutanix 管理ツールで管理

### 技術面

- **シンプルなデプロイ**: Nutanix Prism から直接デプロイ
- **高可用性**: Nutanix HCI の HA 機能を活用
- **パフォーマンス**: ローカルキャッシュによる低レイテンシアクセス

## デメリット・制約事項

### 制限事項

- Nutanix AHV の特定バージョンが必要
- ローカルキャッシュ用のストレージが必要
- AWS への安定したネットワーク接続が必要

### 考慮すべき点

- ゲートウェイの仮想マシンリソース要件
- ネットワーク帯域幅の計画
- バックアップとリカバリの設計

## ユースケース

### ユースケース 1: Nutanix 環境からのクラウドバックアップ

**シナリオ**: Nutanix HCI 上のファイルサーバーデータを AWS にバックアップ

**実装例**:
```
S3 File Gateway を Nutanix AHV にデプロイ
→ NFS/SMB 共有を作成
→ バックアップソフトウェアから共有にバックアップ
→ データは自動的に S3 に保存
```

**効果**: 既存の Nutanix インフラストラクチャを活用したシームレスなクラウドバックアップ

### ユースケース 2: テープバックアップの置き換え

**シナリオ**: 物理テープライブラリを仮想テープライブラリに置き換え

**実装例**:
```
Tape Gateway を Nutanix AHV にデプロイ
→ 既存のバックアップソフトウェアから iSCSI-VTL として認識
→ 仮想テープは S3 Glacier に保存
```

**効果**: テープ管理の運用負荷を削減しながら長期保存コストを最適化

### ユースケース 3: ハイブリッドストレージアーキテクチャ

**シナリオ**: オンプレミスアプリケーションからクラウドストレージにアクセス

**実装例**:
```
Volume Gateway を Nutanix AHV にデプロイ
→ iSCSI ボリュームをアプリケーションサーバーにマウント
→ データは AWS にレプリケート
```

**効果**: オンプレミスとクラウドのシームレスな統合

## 料金

AWS Storage Gateway の料金は、ゲートウェイタイプと使用量に基づきます。

### 料金例

| 項目 | 料金 |
|------|------|
| S3 File Gateway | 書き込みデータ量に基づく |
| Tape Gateway | 仮想テープストレージに基づく |
| Volume Gateway | ボリュームストレージに基づく |

詳細な料金については、[AWS Storage Gateway 料金ページ](https://aws.amazon.com/storagegateway/pricing/)を参照してください。

## 利用可能リージョン

すべての AWS リージョンで利用可能です。

## 関連サービス・機能

- **AWS Storage Gateway**: ハイブリッドクラウドストレージサービス
- **Amazon S3**: オブジェクトストレージ
- **Amazon S3 Glacier**: 長期アーカイブストレージ
- **Amazon EBS**: ブロックストレージ

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/12/aws-storage-gateway-nutanix-avh-hypervisor/)
- [AWS Storage Gateway ユーザーガイド](https://docs.aws.amazon.com/filegateway/latest/files3/what-is-file-s3.html)
- [Storage Gateway コンソール](https://console.aws.amazon.com/storagegateway/)

## まとめ

AWS Storage Gateway が Nutanix AHV ハイパーバイザーをサポートしました。Nutanix HCI 環境を使用している組織は、既存のインフラストラクチャを活用して AWS クラウドストレージにシームレスにアクセスできるようになります。S3 File Gateway、Tape Gateway、Volume Gateway のすべてのゲートウェイタイプが Nutanix AHV 上にデプロイ可能です。
