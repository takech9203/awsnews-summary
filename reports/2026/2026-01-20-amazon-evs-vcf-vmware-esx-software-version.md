# Amazon EVS - VCF と VMware ESX ソフトウェアバージョン選択サポート

**リリース日**: 2026年01月20日
**サービス**: Amazon Elastic VMware Service (Amazon EVS)
**機能**: VCF と ESX ソフトウェアバージョンの選択サポート

## 概要

Amazon Elastic VMware Service (Amazon EVS) が、EVS 環境とホストをセットアップする際に、サポートされる VMware Cloud Foundation (VCF) と ESX ソフトウェアバージョンの組み合わせを指定できる機能を追加しました。

Amazon EVS を使用すると、AWS Nitro EC2 ベアメタルインスタンスを基盤として、Amazon Virtual Private Cloud (VPC) 内で VCF をネイティブに実行できます。Amazon EVS は、AWS コンソールの直感的なステップバイステップ構成ワークフローまたは AWS Command Line Interface (CLI) を使用して、数時間で完全な VCF 環境のデプロイを自動化します。この最新の機能強化は、ワークロードの AWS への移行を高速化し、運用の複雑性とリスクを削減し、Amazon EVS でデータセンター退出期限を満たすための重要なバージョン柔軟性のニーズに対応します。

**アップデート前の課題**

- VCF と ESX のソフトウェアバージョンを選択できず、デフォルトバージョンのみが使用可能だった
- 既存の VCF 環境との互換性を維持しながら移行するのが困難だった
- データセンター退出時に特定のバージョンを使用する必要がある場合に柔軟性が不足していた

**アップデート後の改善**

- 環境作成時に VCF バージョンを指定できるようになった
- 既存環境にホストを追加する際に ESX バージョンを選択できるようになった
- サポートされるバージョンの組み合わせを API で照会可能になった
- VCF 5.2.2 での新規環境デプロイをサポート

## サービスアップデートの詳細

### 主要機能

1. **VCF バージョンの選択**
   - CreateEnvironment API を使用して新規環境作成時に VCF バージョンを指定可能
   - VCF 5.2.2 を含む複数のバージョンをサポート
   - 既存の VCF 環境との互換性を維持しながら移行可能

2. **ESX バージョンの選択**
   - CreateEnvironmentHost API を使用して既存環境にホストを追加する際に ESX バージョンを選択可能
   - 環境内の異なるホストで異なる ESX バージョンを使用可能 (サポートされる組み合わせ内)
   - 段階的なアップグレードパスを実現

3. **バージョン互換性の照会**
   - 新しい GetVersions API でサポートされるバージョンの組み合わせを照会可能
   - VCF と ESX の互換性マトリックスを取得
   - 適切なバージョン選択をサポート

4. **VCF 5.2.2 のサポート**
   - 新規環境デプロイで VCF 5.2.2 をサポート
   - 最新の VCF 機能を活用可能

## 技術仕様

### 新しい API

| API | 機能 |
|-----|------|
| CreateEnvironment | VCF バージョンを指定して新規環境を作成 |
| CreateEnvironmentHost | ESX バージョンを指定して既存環境にホストを追加 |
| GetVersions | サポートされる VCF と ESX のバージョン組み合わせを照会 |

### サポート対象バージョン

- **VCF**: 複数のバージョン (VCF 5.2.2 を含む) をサポート
- **ESX**: 各 VCF バージョンと互換性のある ESX バージョンをサポート

詳細なサポート対象バージョンは GetVersions API で照会できます。

## 設定方法

### 前提条件

1. Amazon EVS へのアクセス権限
2. AWS CLI または SDK の設定
3. 適切な IAM 権限

### 手順

#### ステップ1: サポートされるバージョンの確認

GetVersions API を使用して、サポートされる VCF と ESX のバージョン組み合わせを照会します。

```bash
aws evs get-versions
```

#### ステップ2: VCF バージョンを指定して環境を作成

CreateEnvironment API を使用して、特定の VCF バージョンで新規環境を作成します。

```bash
aws evs create-environment \
    --name my-evs-environment \
    --vcf-version 5.2.2 \
    --vpc-id vpc-xxxxx \
    --subnet-ids subnet-xxxxx,subnet-yyyyy \
    --instance-type i4i.metal
```

#### ステップ3: ESX バージョンを指定してホストを追加

CreateEnvironmentHost API を使用して、特定の ESX バージョンで既存環境にホストを追加します。

```bash
aws evs create-environment-host \
    --environment-id env-xxxxx \
    --esx-version 8.0.3 \
    --subnet-id subnet-xxxxx \
    --instance-type i4i.metal
```

## メリット

### ビジネス面

- **移行の柔軟性**: 既存の VCF 環境との互換性を維持しながら AWS に移行可能
- **データセンター退出の加速**: 特定のバージョン要件を満たしながら迅速に移行
- **リスクの削減**: バージョン互換性を事前に確認し、移行リスクを低減

### 技術面

- **バージョン管理**: 環境とホストのバージョンを細かく制御可能
- **段階的アップグレード**: 異なるホストで異なる ESX バージョンを使用し、段階的にアップグレード
- **互換性の保証**: GetVersions API でサポートされる組み合わせを事前に確認

## デメリット・制約事項

### 制限事項

- サポートされる VCF と ESX のバージョン組み合わせは限定的
- すべてのバージョンがすべてのリージョンで利用可能とは限らない
- 古いバージョンは将来的にサポートが終了する可能性がある

### 考慮すべき点

- バージョン選択前に GetVersions API でサポート状況を確認する必要がある
- 環境内のすべてのホストで互換性のある ESX バージョンを使用する必要がある
- VMware のサポートポリシーとライフサイクルを考慮する必要がある

## ユースケース

### ユースケース1: 既存 VCF 環境からの移行

**シナリオ**: オンプレミスの VCF 5.1 環境から AWS に移行

**実装例**:
- GetVersions API で VCF 5.1 と互換性のあるバージョンを確認
- CreateEnvironment API で VCF 5.1 を指定して環境を作成
- オンプレミス環境と同じバージョンで移行を開始
- 移行完了後、段階的に新しいバージョンにアップグレード

**効果**: バージョン互換性を維持しながらスムーズに移行し、移行リスクを低減

### ユースケース2: 段階的な ESX バージョンアップグレード

**シナリオ**: 本番環境で ESX を段階的にアップグレード

**実装例**:
- 既存環境で古い ESX バージョンのホストを稼働
- CreateEnvironmentHost API で新しい ESX バージョンのホストを追加
- ワークロードを新しいホストに段階的に移行
- 古いホストを削除

**効果**: ダウンタイムなしで ESX をアップグレードし、本番環境へのリスクを最小化

### ユースケース3: データセンター退出期限への対応

**シナリオ**: データセンター退出期限が迫っており、特定の VCF バージョンで移行する必要がある

**実装例**:
- GetVersions API で要求される VCF バージョンがサポートされているか確認
- CreateEnvironment API で特定の VCF バージョンを指定して迅速に環境を構築
- オンプレミスワークロードを AWS に移行
- データセンター退出期限を満たす

**効果**: 柔軟なバージョン選択により、期限内に移行を完了し、データセンター退出を実現

## 料金

Amazon EVS の料金は、使用する EC2 ベアメタルインスタンスの数と種類に基づいて課金されます。ソフトウェアバージョンの選択による追加料金は発生しません。

詳細な料金情報は [Amazon EVS pricing page](https://aws.amazon.com/evs/pricing/) を参照してください。

## 利用可能リージョン

Amazon EVS が利用可能な全リージョンで、VCF と ESX ソフトウェアバージョン選択機能を使用できます。

## 関連サービス・機能

- **VMware Cloud Foundation (VCF)**: 統合されたソフトウェアスタック
- **VMware ESX**: 仮想化プラットフォーム
- **AWS Nitro System**: 高性能なベアメタルインスタンスを提供

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/amazon-evs-vcf-vmware-esx-software-version/)
- [Amazon EVS product detail page](https://aws.amazon.com/evs/)
- [Amazon EVS user guide](https://docs.aws.amazon.com/evs/latest/userguide/what-is-evs.html)

## まとめ

Amazon EVS の VCF と VMware ESX ソフトウェアバージョン選択サポートにより、既存の VCF 環境との互換性を維持しながら AWS への移行がより柔軟になりました。バージョンを細かく制御できることで、データセンター退出期限を満たし、運用の複雑性とリスクを削減できます。VMware ワークロードを AWS に移行する組織は、この新機能を活用して移行を加速し、段階的なアップグレードパスを実現してください。
