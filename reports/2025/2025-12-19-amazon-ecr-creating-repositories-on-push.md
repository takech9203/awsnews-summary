# Amazon ECR - プッシュ時のリポジトリ自動作成

**リリース日**: 2025 年 12 月 19 日
**サービス**: Amazon Elastic Container Registry (Amazon ECR)
**機能**: プッシュ時のリポジトリ自動作成 (Create on Push)

## 概要

Amazon ECR でイメージプッシュ時のリポジトリ自動作成機能がサポートされました。この機能により、コンテナイメージをプッシュする際にリポジトリが存在しない場合、ECR が自動的にリポジトリを作成します。事前にリポジトリを作成する必要がなくなり、コンテナワークフローが大幅に簡素化されます。

**アップデート前の課題**

- コンテナイメージをプッシュする前に、リポジトリを事前に作成する必要があった
- CI/CD パイプラインでリポジトリ作成ステップを追加する必要があった
- 新しいマイクロサービスを追加する際の手順が煩雑だった

**アップデート後の改善**

- イメージプッシュ時にリポジトリが自動作成される
- リポジトリ作成テンプレートに従った設定が自動適用される
- CI/CD パイプラインの簡素化

## サービスアップデートの詳細

### 主要機能

1. **自動リポジトリ作成**
   - イメージプッシュ時にリポジトリが存在しない場合、自動的に作成
   - 事前のリポジトリ作成が不要に

2. **リポジトリ作成テンプレート**
   - テンプレートで暗号化、タグイミュータビリティ、ライフサイクルポリシーなどを定義
   - 自動作成されるリポジトリに一貫した設定を適用

3. **既存ワークフローとの互換性**
   - 既存のプッシュコマンドをそのまま使用可能
   - 追加の設定変更なしで利用開始可能

## 技術仕様

### API 変更履歴

| 日付 | サービス | 変更内容 |
|------|----------|----------|
| 2025/12/18 | Amazon ECR | 4 updated api methods |
| 2025/11/21 | Amazon ECR | 4 new api methods |

### リポジトリ作成テンプレートの設定例

```json
{
    "prefix": "my-app/",
    "description": "Auto-created repository",
    "encryptionConfiguration": {
        "encryptionType": "KMS",
        "kmsKey": "arn:aws:kms:ap-northeast-1:123456789012:key/xxx"
    },
    "imageTagMutability": "IMMUTABLE",
    "repositoryPolicy": "...",
    "lifecyclePolicy": "..."
}
```

## 設定方法

### 前提条件

1. Amazon ECR へのアクセス権限
2. リポジトリ作成テンプレートの設定（オプション）

### 手順

#### ステップ 1: リポジトリ作成テンプレートの設定

```bash
aws ecr put-repository-creation-template \
    --prefix "my-app/" \
    --encryption-configuration encryptionType=KMS \
    --image-tag-mutability IMMUTABLE
```

#### ステップ 2: イメージのプッシュ

```bash
# リポジトリが存在しなくても自動作成される
docker push 123456789012.dkr.ecr.ap-northeast-1.amazonaws.com/my-app/new-service:latest
```

## メリット

### ビジネス面

- **開発速度の向上**: リポジトリ作成の手間を削減
- **運用の簡素化**: CI/CD パイプラインのステップを削減
- **一貫性の確保**: テンプレートによる標準化された設定

### 技術面

- **ワークフローの簡素化**: プッシュ前のリポジトリ作成が不要
- **自動化の強化**: 新しいサービスの追加が容易に
- **ガバナンスの維持**: テンプレートによるポリシー適用

## デメリット・制約事項

### 制限事項

- リポジトリ作成テンプレートの設定が必要（デフォルト設定を使用する場合は不要）
- 既存のリポジトリには影響しない

### 考慮すべき点

- 意図しないリポジトリが作成される可能性があるため、プレフィックスの設計が重要
- IAM ポリシーでリポジトリ作成権限を適切に管理

## ユースケース

### ユースケース 1: マイクロサービスの迅速なデプロイ

**シナリオ**: 新しいマイクロサービスを追加する際、リポジトリを事前に作成せずにデプロイ

**効果**: 新サービスの追加時間を短縮

### ユースケース 2: CI/CD パイプラインの簡素化

**シナリオ**: ビルドパイプラインでリポジトリ作成ステップを削除

**効果**: パイプラインの複雑さを軽減し、メンテナンス性を向上

## 料金

この機能は追加料金なしで利用可能です。Amazon ECR の標準料金のみが適用されます。

## 利用可能リージョン

すべての AWS 商用リージョンおよび AWS GovCloud (US) リージョンで利用可能です。

## 関連サービス・機能

- **Amazon ECS**: コンテナオーケストレーション
- **Amazon EKS**: Kubernetes マネージドサービス
- **AWS CodePipeline**: CI/CD パイプライン

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/12/amazon-ecr-creating-repositories-on-push/)
- [リポジトリ作成テンプレート ドキュメント](https://docs.aws.amazon.com/AmazonECR/latest/userguide/repository-creation-templates.html)
- [Amazon ECR ユーザーガイド](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)

## まとめ

Amazon ECR のプッシュ時リポジトリ自動作成機能により、コンテナワークフローが大幅に簡素化されます。リポジトリ作成テンプレートと組み合わせることで、一貫した設定を維持しながら開発速度を向上させることができます。
