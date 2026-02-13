# Amazon OpenSearch UI - CMK サポートとメタデータサイズ拡大

**リリース日**: 2025 年 12 月 29 日
**サービス**: Amazon OpenSearch Service
**機能**: OpenSearch UI の CMK サポートとメタデータサイズ拡大

## 概要

Amazon OpenSearch Service で、OpenSearch UI のメタデータに対する AWS KMS カスタマーマネージドキー (CMK) のサポートと、メタデータサイズの上限拡大が発表されました。

Amazon OpenSearch UI は、OpenSearch ドメインやコレクション、Amazon S3、Amazon CloudWatch、AWS Security Lake など、複数のデータソースにわたる統合ビューを提供するダッシュボードおよび運用分析のマネージドサービスです。今回のアップデートにより、規制やコンプライアンス要件を満たすための暗号化オプションと、より複雑なダッシュボードの作成が可能になりました。

**アップデート前の課題**

- OpenSearch UI のメタデータを独自の暗号化キーで保護できなかった
- 規制やコンプライアンス要件で CMK が必要な組織では導入が困難だった
- 保存オブジェクトのサイズ制限により、複雑なクエリや大規模なダッシュボードの作成が制限されていた

**アップデート後の改善**

- 独自の CMK でメタデータを暗号化した OpenSearch UI アプリケーションを作成可能に
- 規制やコンプライアンス要件を満たしやすくなった
- メタデータサイズの上限拡大により、複雑なクエリ、広範なビジュアライゼーション、大規模なダッシュボードの作成・保存が可能に

## サービスアップデートの詳細

### 主要機能

1. **AWS KMS カスタマーマネージドキー (CMK) サポート**
   - 新規 OpenSearch UI アプリケーションで CMK による暗号化が可能
   - 組織独自の暗号化キーでメタデータを保護
   - 規制・コンプライアンス要件への対応

2. **メタデータサイズ上限の拡大**
   - 保存オブジェクトのサイズ制限を拡大
   - 複雑なクエリの保存が可能に
   - 広範なビジュアライゼーションの作成
   - 大規模なダッシュボードの構築

3. **複数データソースの統合ビュー**
   - OpenSearch ドメインとコレクション
   - Amazon S3
   - Amazon CloudWatch
   - AWS Security Lake

## 技術仕様

### CMK 暗号化の設定

| 項目 | 説明 |
|------|------|
| 暗号化タイプ | AWS KMS カスタマーマネージドキー |
| 対象 | OpenSearch UI アプリケーションのメタデータ |
| 適用タイミング | 新規アプリケーション作成時 |

### サポートされるデータソース

| データソース | 説明 |
|-------------|------|
| OpenSearch ドメイン | OpenSearch Service のドメイン |
| OpenSearch コレクション | OpenSearch Serverless のコレクション |
| Amazon S3 | オブジェクトストレージ |
| Amazon CloudWatch | モニタリングデータ |
| AWS Security Lake | セキュリティデータレイク |

## 設定方法

### 前提条件

1. AWS アカウントと OpenSearch Service へのアクセス権限
2. AWS KMS でカスタマーマネージドキーを作成済み
3. 適切な IAM 権限

### 手順

#### ステップ 1: KMS キーの作成

```bash
# KMS カスタマーマネージドキーの作成
aws kms create-key \
    --description "OpenSearch UI metadata encryption key" \
    --key-usage ENCRYPT_DECRYPT \
    --origin AWS_KMS
```

OpenSearch UI のメタデータ暗号化に使用する KMS キーを作成します。

#### ステップ 2: キーポリシーの設定

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Allow OpenSearch UI to use the key",
            "Effect": "Allow",
            "Principal": {
                "Service": "opensearch.amazonaws.com"
            },
            "Action": [
                "kms:Encrypt",
                "kms:Decrypt",
                "kms:GenerateDataKey"
            ],
            "Resource": "*"
        }
    ]
}
```

OpenSearch Service が KMS キーを使用できるようにキーポリシーを設定します。

#### ステップ 3: OpenSearch UI アプリケーションの作成

1. Amazon OpenSearch Service コンソールを開く
2. 「OpenSearch UI」セクションに移動
3. 「アプリケーションを作成」をクリック
4. 暗号化設定で「カスタマーマネージドキー」を選択
5. 作成した KMS キーを指定
6. アプリケーションを作成

## メリット

### ビジネス面

- **コンプライアンス対応**: 規制要件で CMK が必要な業界 (金融、医療など) での導入が容易に
- **データガバナンス強化**: 組織独自のキーによるデータ保護
- **監査対応**: KMS のキー使用ログによる監査証跡

### 技術面

- **セキュリティ強化**: 独自の暗号化キーによるメタデータ保護
- **柔軟性向上**: より複雑なダッシュボードやクエリの作成が可能
- **スケーラビリティ**: 大規模な分析環境に対応

## デメリット・制約事項

### 制限事項

- CMK 暗号化は新規アプリケーション作成時のみ設定可能
- 既存のアプリケーションへの CMK 適用は不可
- KMS キーの管理コストが発生

### 考慮すべき点

- KMS キーの削除やアクセス権限の変更により、メタデータにアクセスできなくなる可能性
- キーローテーションポリシーの設定を推奨
- 複数リージョンでの運用時はリージョンごとにキーが必要

## ユースケース

### ユースケース 1: 金融機関のコンプライアンス対応

**シナリオ**: 金融規制で顧客データの暗号化に CMK が必要な環境

**実装例**:
1. 金融規制に準拠した KMS キーを作成
2. OpenSearch UI アプリケーションを CMK で暗号化して作成
3. 顧客データの分析ダッシュボードを構築

**効果**: 規制要件を満たしながら、統合的なデータ分析環境を構築

### ユースケース 2: 大規模セキュリティダッシュボード

**シナリオ**: Security Lake のデータを含む複雑なセキュリティダッシュボードを作成

**実装例**:
1. メタデータサイズ拡大を活用
2. 複数のデータソース (Security Lake、CloudWatch、OpenSearch) を統合
3. 複雑なクエリと広範なビジュアライゼーションを保存

**効果**: 包括的なセキュリティ監視環境を単一のダッシュボードで実現

### ユースケース 3: マルチテナント分析環境

**シナリオ**: 複数の顧客向けに分離された分析環境を提供

**実装例**:
1. 顧客ごとに異なる KMS キーを使用
2. 各顧客用の OpenSearch UI アプリケーションを作成
3. データの分離と暗号化を確保

**効果**: 顧客データの分離とセキュリティを確保しながら、分析サービスを提供

## 料金

CMK の使用には AWS KMS の料金が適用されます。

### 料金例

| 項目 | 料金 |
|------|------|
| KMS キー (月額) | $1.00/キー |
| API リクエスト | $0.03/10,000 リクエスト |
| OpenSearch UI | OpenSearch Service の標準料金 |

## 利用可能リージョン

OpenSearch UI が利用可能なすべてのリージョンで利用可能です。詳細は [OpenSearch UI エンドポイントとクォータ](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/opensearch-ui-endpoints-quotas.html) を参照してください。

## 関連サービス・機能

- **Amazon OpenSearch Service**: 検索・分析サービス
- **AWS KMS**: キー管理サービス
- **AWS Security Lake**: セキュリティデータレイク
- **Amazon CloudWatch**: モニタリングサービス

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/12/amazon-opensearch-ui-cmk-metadata/)
- [Amazon OpenSearch UI Developer Guide](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/application.html)
- [OpenSearch UI エンドポイントとクォータ](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/opensearch-ui-endpoints-quotas.html)

## まとめ

Amazon OpenSearch UI の CMK サポートとメタデータサイズ拡大により、規制・コンプライアンス要件を満たしながら、より複雑で大規模なダッシュボードを構築できるようになりました。金融機関や医療機関など、厳格なデータ保護要件がある組織での OpenSearch UI の導入が容易になります。
