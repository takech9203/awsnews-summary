# AWS Secrets Manager - シークレットソート機能の強化

**リリース日**: 2025 年 12 月 23 日
**サービス**: AWS Secrets Manager
**機能**: シークレットソート機能の強化

## 概要

AWS Secrets Manager のコンソールおよび ListSecrets API で、シークレットのソート機能が強化されました。従来は作成日のみでソートが可能でしたが、今回のアップデートにより、名前、最終変更日、最終アクセス日、作成日の 4 つの項目でソートできるようになりました。

この機能強化により、大量のシークレットを管理する環境において、目的のシークレットをより効率的に発見・管理できるようになります。

**アップデート前の課題**

- シークレットのソートは作成日のみに限定されていた
- 最近変更されたシークレットや最近アクセスされたシークレットを特定するのが困難だった
- 名前順でのソートができず、アルファベット順での一覧表示ができなかった

**アップデート後の改善**

- 名前、最終変更日、最終アクセス日、作成日の 4 つの項目でソート可能に
- コンソールと API の両方で柔軟なソートオプションを提供
- シークレットの発見と管理効率が大幅に向上

## サービスアップデートの詳細

### 主要機能

1. **複数のソート項目のサポート**
   - `name`: シークレット名でのアルファベット順ソート
   - `created-date`: 作成日でのソート（従来機能）
   - `last-changed-date`: 最終変更日でのソート
   - `last-accessed-date`: 最終アクセス日でのソート

2. **昇順・降順の指定**
   - `SortOrder` パラメータで `asc`（昇順）または `desc`（降順）を指定可能
   - デフォルトは降順

3. **コンソールと API の両方で利用可能**
   - AWS マネジメントコンソールの Secrets Manager 画面でソート可能
   - ListSecrets API で `SortBy` パラメータを使用

## 技術仕様

### API パラメータ

| パラメータ | 値 | 説明 |
|-----------|-----|------|
| SortBy | `name` | シークレット名でソート |
| SortBy | `created-date` | 作成日でソート |
| SortBy | `last-changed-date` | 最終変更日でソート |
| SortBy | `last-accessed-date` | 最終アクセス日でソート |
| SortOrder | `asc` / `desc` | 昇順 / 降順 |

### API 変更履歴

| 日付 | サービス | 変更内容 |
|------|----------|----------|
| 2025/12/11 | AWS Secrets Manager | 1 updated api methods - ListSecrets に SortBy パラメータを追加 |

### ListSecrets API の使用例

```python
import boto3

client = boto3.client('secretsmanager')

# 最終変更日でソート（降順）
response = client.list_secrets(
    SortBy='last-changed-date',
    SortOrder='desc',
    MaxResults=10
)

for secret in response['SecretList']:
    print(f"Name: {secret['Name']}")
    print(f"Last Changed: {secret.get('LastChangedDate', 'N/A')}")
    print(f"Last Accessed: {secret.get('LastAccessedDate', 'N/A')}")
    print("---")
```

ListSecrets API に `SortBy` パラメータを追加することで、指定した項目でソートされた結果を取得できます。

### AWS CLI での使用例

```bash
# 名前でソート（昇順）
aws secretsmanager list-secrets \
    --sort-by name \
    --sort-order asc

# 最終アクセス日でソート（降順）
aws secretsmanager list-secrets \
    --sort-by last-accessed-date \
    --sort-order desc
```

AWS CLI でも `--sort-by` と `--sort-order` オプションを使用してソートを指定できます。

## 設定方法

### 前提条件

1. AWS アカウントと Secrets Manager へのアクセス権限
2. `secretsmanager:ListSecrets` 権限を持つ IAM ロールまたはユーザー

### 手順

#### ステップ 1: コンソールでのソート

1. AWS マネジメントコンソールで Secrets Manager を開く
2. シークレット一覧画面で、列ヘッダーをクリックしてソート
3. 名前、作成日、最終変更日、最終アクセス日でソート可能

#### ステップ 2: API でのソート

```python
import boto3

client = boto3.client('secretsmanager')

# フィルタリングとソートの組み合わせ
response = client.list_secrets(
    Filters=[
        {
            'Key': 'tag-key',
            'Values': ['Environment']
        }
    ],
    SortBy='last-changed-date',
    SortOrder='desc'
)
```

フィルタリングとソートを組み合わせることで、特定の条件に合致するシークレットを効率的に検索できます。

## メリット

### ビジネス面

- **運用効率の向上**: 最近変更されたシークレットを素早く特定し、監査やトラブルシューティングを効率化
- **セキュリティ監査の簡素化**: 最終アクセス日でソートすることで、未使用のシークレットを特定しやすくなる
- **管理コストの削減**: 大量のシークレットを効率的に管理できるようになり、運用負荷を軽減

### 技術面

- **API の柔軟性向上**: プログラムによるシークレット管理がより柔軟に
- **自動化の強化**: ソート機能を活用した自動化スクリプトの作成が容易に
- **既存ワークフローとの互換性**: 既存の API 呼び出しに影響なく、オプションとして利用可能

## デメリット・制約事項

### 制限事項

- ソートは一度に 1 つの項目のみ指定可能（複数項目での複合ソートは不可）
- ページネーション使用時は、各ページ内でのソートとなる

### 考慮すべき点

- 大量のシークレットがある場合、ソート処理に時間がかかる可能性がある
- 最終アクセス日は、シークレット値の取得時に更新される

## ユースケース

### ユースケース 1: セキュリティ監査

**シナリオ**: 長期間アクセスされていないシークレットを特定し、不要なシークレットを削除する

**実装例**:
```python
import boto3
from datetime import datetime, timedelta

client = boto3.client('secretsmanager')

# 最終アクセス日でソート（昇順 = 古い順）
response = client.list_secrets(
    SortBy='last-accessed-date',
    SortOrder='asc'
)

threshold = datetime.now() - timedelta(days=90)

for secret in response['SecretList']:
    last_accessed = secret.get('LastAccessedDate')
    if last_accessed and last_accessed < threshold:
        print(f"未使用シークレット: {secret['Name']}")
```

**効果**: 90 日以上アクセスされていないシークレットを特定し、セキュリティリスクを軽減

### ユースケース 2: 変更追跡

**シナリオ**: 最近変更されたシークレットを確認し、変更履歴を追跡する

**実装例**:
```python
import boto3

client = boto3.client('secretsmanager')

# 最終変更日でソート（降順 = 新しい順）
response = client.list_secrets(
    SortBy='last-changed-date',
    SortOrder='desc',
    MaxResults=10
)

print("最近変更されたシークレット:")
for secret in response['SecretList']:
    print(f"- {secret['Name']}: {secret.get('LastChangedDate', 'N/A')}")
```

**効果**: 最近の変更を素早く把握し、変更管理プロセスを効率化

### ユースケース 3: 名前による検索

**シナリオ**: 特定のプレフィックスを持つシークレットをアルファベット順で一覧表示

**実装例**:
```python
import boto3

client = boto3.client('secretsmanager')

# 名前でソート（昇順）+ フィルタリング
response = client.list_secrets(
    Filters=[
        {
            'Key': 'name',
            'Values': ['prod/']
        }
    ],
    SortBy='name',
    SortOrder='asc'
)

for secret in response['SecretList']:
    print(secret['Name'])
```

**効果**: 環境やアプリケーション別にシークレットを整理して表示

## 料金

この機能は追加料金なしで利用可能です。Secrets Manager の標準料金のみが適用されます。

## 利用可能リージョン

すべての AWS 商用リージョンおよび AWS GovCloud (US) リージョンで利用可能です。

## 関連サービス・機能

- **AWS CloudTrail**: シークレットへのアクセスログを記録
- **AWS Config**: シークレットの設定変更を追跡
- **AWS IAM**: シークレットへのアクセス制御

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/12/aws-secrets-manager-secret-sorting-capabilities/)
- [AWS Secrets Manager ドキュメント](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)
- [ListSecrets API リファレンス](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_ListSecrets.html)
- [AWS Secrets Manager 料金ページ](https://aws.amazon.com/secrets-manager/pricing/)

## まとめ

AWS Secrets Manager のソート機能強化により、大量のシークレットを管理する環境での運用効率が大幅に向上します。特にセキュリティ監査や変更追跡において、最終アクセス日や最終変更日でのソートが有効です。既存の API との互換性を維持しながら、オプションとして利用できるため、既存のワークフローに影響なく導入できます。
