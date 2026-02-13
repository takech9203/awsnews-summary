# Amazon RDS for Oracle - Standard Edition 2 のベアメタルインスタンスサポート

**リリース日**: 2026年01月20日
**サービス**: Amazon RDS for Oracle
**機能**: Oracle Standard Edition 2 の BYOL ライセンスでベアメタルインスタンスをサポート

## 概要

Amazon RDS for Oracle が、Bring Your Own License (BYOL) ライセンスを使用した Oracle Standard Edition 2 でベアメタルインスタンスをサポートするようになりました。M7i、R7i、X2iedn、X2idn、X2iezn、M6i、M6id、M6in、R6i、R6id、および R6in のベアメタルインスタンスを、同等の仮想化インスタンスと比較して 25% 低い価格で使用できます。

ベアメタルインスタンスでは、基盤となるサーバーの CPU コア数とソケット数を完全に可視化できるため、商用データベースライセンスとサポートコストを削減できる可能性があります。ほとんどのベアメタルインスタンスは 2 ソケットですが、db.m7i.metal-24xl および db.r7i.metal-24xl インスタンスは各 1 ソケットです。

**アップデート前の課題**

- Oracle Standard Edition 2 でベアメタルインスタンスを使用できなかった
- 仮想化インスタンスでは CPU コア数とソケット数の可視性が限定的だった
- ライセンスコストを最適化する機会が限られていた

**アップデート後の改善**

- Oracle Standard Edition 2 でベアメタルインスタンスを使用可能になった
- 基盤となるサーバーの CPU コア数とソケット数を完全に可視化できるようになった
- 仮想化インスタンスと比較して 25% 低い価格で利用可能になった
- ライセンスとサポートコストを削減できる可能性がある

## サービスアップデートの詳細

### 主要機能

1. **ベアメタルインスタンスのサポート**
   - M7i、R7i、X2iedn、X2idn、X2iezn、M6i、M6id、M6in、R6i、R6id、R6in
   - 仮想化オーバーヘッドなしで高パフォーマンスを実現
   - Oracle Standard Edition 2 の BYOL ライセンスで利用可能

2. **CPU コアとソケットの完全な可視性**
   - 基盤となるサーバーの CPU コア数を正確に把握
   - ソケット数を明確に確認可能
   - ライセンス管理を簡素化

3. **コスト削減**
   - 仮想化インスタンスと比較して 25% 低い価格
   - ライセンスコストを削減できる可能性
   - サポートコストも削減できる可能性

4. **ソケット構成**
   - ほとんどのベアメタルインスタンス: 2 ソケット
   - db.m7i.metal-24xl および db.r7i.metal-24xl: 1 ソケット

## 技術仕様

### サポート対象ベアメタルインスタンス

| インスタンスファミリー | インスタンスタイプ | ソケット数 |
|---------------------|------------------|----------|
| M7i | db.m7i.metal-24xl | 1 |
| R7i | db.r7i.metal-24xl | 1 |
| M6i | db.m6i.metal など | 2 |
| M6id | db.m6id.metal など | 2 |
| M6in | db.m6in.metal など | 2 |
| R6i | db.r6i.metal など | 2 |
| R6id | db.r6id.metal など | 2 |
| R6in | db.r6in.metal など | 2 |
| X2iedn | db.x2iedn.metal など | 2 |
| X2idn | db.x2idn.metal など | 2 |
| X2iezn | db.x2iezn.metal など | 2 |

### ライセンス要件

| 項目 | 詳細 |
|------|------|
| ライセンスモデル | Bring Your Own License (BYOL) |
| サポート対象エディション | Oracle Standard Edition 2 |
| その他のサポート対象 | Oracle Enterprise Edition (既存) |

## 設定方法

### 前提条件

1. Oracle Standard Edition 2 の BYOL ライセンス
2. License Portability for BYOL の資格
3. ライセンス管理パートナーまたは法務部門との確認

### 手順

#### ステップ1: ライセンスの確認

法務部門またはライセンス管理パートナーに連絡し、ベアメタルインスタンスで Oracle Standard Edition 2 を使用できるか、ライセンスとサポートコストを削減できるかを確認します。

#### ステップ2: インスタンスタイプの選択

ワークロードに適したベアメタルインスタンスタイプを選択します。

- **M7i/M6i**: 汎用ワークロード
- **R7i/R6i**: メモリ集約型ワークロード
- **X2iedn/X2idn/X2iezn**: 超高メモリワークロード

#### ステップ3: RDS インスタンスの作成

RDS コンソール、AWS CLI、または SDK を使用して、ベアメタルインスタンスで Oracle Standard Edition 2 の BYOL インスタンスを作成します。

```bash
aws rds create-db-instance \
    --db-instance-identifier my-oracle-se2-bare-metal \
    --db-instance-class db.m7i.metal-24xl \
    --engine oracle-se2-byol \
    --license-model bring-your-own-license \
    --master-username admin \
    --master-user-password MyPassword123 \
    --allocated-storage 100
```

#### ステップ4: CPU コアとソケット数の確認

RDS コンソールまたは AWS CLI を使用して、インスタンスの CPU コア数とソケット数を確認します。

#### ステップ5: ライセンスコストの最適化

ベアメタルインスタンスの CPU コア数とソケット数に基づいて、Oracle ライセンスを最適化します。

## メリット

### ビジネス面

- **コスト削減**: 仮想化インスタンスと比較して 25% 低い価格
- **ライセンス最適化**: CPU コアとソケット数の可視性によりライセンスコストを削減できる可能性
- **予測可能なコスト**: 正確なリソース情報に基づいたライセンス管理

### 技術面

- **高パフォーマンス**: 仮想化オーバーヘッドなしで最大パフォーマンスを実現
- **完全な可視性**: CPU コアとソケット数を正確に把握
- **柔軟性**: 複数のベアメタルインスタンスタイプから選択可能

## デメリット・制約事項

### 制限事項

- BYOL ライセンスが必要
- License Portability for BYOL の資格が必要
- すべてのリージョンで利用可能とは限らない

### 考慮すべき点

- ライセンス管理パートナーまたは法務部門との確認が必要
- Oracle のライセンスポリシーは変更される可能性がある
- ベアメタルインスタンスは仮想化インスタンスよりもサイズが大きい

## ユースケース

### ユースケース1: ライセンスコストの最適化

**シナリオ**: Oracle Standard Edition 2 のライセンスコストを削減したい

**実装例**:
- db.m7i.metal-24xl インスタンス (1 ソケット) を使用
- CPU コア数とソケット数を正確に把握し、ライセンスを最適化
- 仮想化インスタンスと比較して 25% 低い価格で利用

**効果**: ライセンスコストとインスタンスコストの両方を削減

### ユースケース2: 高パフォーマンスデータベース

**シナリオ**: 仮想化オーバーヘッドなしで最大パフォーマンスを実現したい

**実装例**:
- db.r7i.metal-24xl インスタンスでメモリ集約型ワークロードを実行
- ベアメタルインスタンスにより仮想化オーバーヘッドを排除
- 高パフォーマンスなデータベースアプリケーションを実現

**効果**: 最大パフォーマンスと低レイテンシーを実現

### ユースケース3: コンプライアンス要件への対応

**シナリオ**: CPU コアとソケット数を正確に報告する必要がある

**実装例**:
- ベアメタルインスタンスで基盤となるサーバーの情報を完全に可視化
- 監査やコンプライアンス報告で正確なリソース情報を提供
- ライセンス監査に対応

**効果**: コンプライアンス要件を満たし、ライセンス監査に対応

## 料金

ベアメタルインスタンスの料金は、仮想化インスタンスと比較して 25% 低く設定されています。詳細な料金情報は [Amazon RDS for Oracle Pricing](https://aws.amazon.com/rds/oracle/pricing/) を参照してください。

## 利用可能リージョン

ベアメタルインスタンスは、複数の AWS リージョンで利用可能です。最新のリージョン情報は [RDS for Oracle Pricing](https://aws.amazon.com/rds/oracle/pricing/) を参照してください。

## 関連サービス・機能

- **Amazon RDS for Oracle Enterprise Edition**: Oracle Enterprise Edition のマネージドデータベースサービス
- **AWS License Manager**: ライセンスの追跡と管理
- **Amazon CloudWatch**: RDS インスタンスのモニタリング

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2026/01/amazon-rds-oracle-support-bare-metal-instances-standard-edition-2/)
- [Amazon RDS for Oracle Pricing](https://aws.amazon.com/rds/oracle/pricing/)
- [RDS for Oracle User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Oracle.html)

## まとめ

Amazon RDS for Oracle の Oracle Standard Edition 2 ベアメタルインスタンスサポートにより、ライセンスコストを最適化し、高パフォーマンスなデータベースを実現できるようになりました。基盤となるサーバーの CPU コア数とソケット数を完全に可視化できることで、ライセンス管理を簡素化し、コスト削減の機会を最大化できます。Oracle Standard Edition 2 を使用している組織は、ライセンス管理パートナーと相談の上、ベアメタルインスタンスへの移行を検討してください。
