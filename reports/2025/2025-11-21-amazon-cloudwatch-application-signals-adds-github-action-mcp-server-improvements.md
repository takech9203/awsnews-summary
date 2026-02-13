# Amazon CloudWatch Application Signals - GitHub Action と MCP サーバー改善

**リリース日**: 2025 年 11 月 21 日
**サービス**: Amazon CloudWatch Application Signals
**機能**: GitHub Action と MCP サーバー改善

## 概要

Amazon CloudWatch Application Signals に新しい GitHub Action と MCP サーバーの改善が追加されました。これにより、アプリケーションオブザーバビリティを開発者ツールに統合し、トラブルシューティングをより迅速かつ便利に行えるようになります。開発者は GitHub を離れることなく、本番環境の問題をトリアージし、トレースデータを確認し、オブザーバビリティのカバレッジを確保できます。

**アップデート前の課題**

- 本番環境の問題をトリアージするために GitHub を離れる必要があった
- コンソール、ダッシュボード、ソースコード間の切り替えが頻繁に発生
- レイテンシやエラーの原因となるコードの特定が困難
- オブザーバビリティの設定に手動作業が必要

**アップデート後の改善**

- GitHub Issues で @awsapm をメンションしてオブザーバビリティベースの回答を取得
- MCP サーバーでレイテンシ、エラー、SLO 違反の原因となるコード行を特定
- Infrastructure-as-Code の自動修正で OTel ベースの APM を設定
- コンテキストスイッチングの削減

## アーキテクチャ図

```mermaid
flowchart TB
    subgraph GitHub["🐙 GitHub"]
        Issues[GitHub Issues]
        Action[GitHub Action]
        Workflow[CI/CD Workflow]
    end
    
    subgraph IDE["💻 AI コーディングエージェント"]
        Kiro[Kiro]
        MCP[MCP サーバー]
    end
    
    subgraph AWS["☁️ AWS"]
        AppSignals[Application Signals]
        CloudWatch[CloudWatch]
        Traces[トレースデータ]
        SLO[SLO モニタリング]
    end
    
    Issues -->|@awsapm| Action
    Action --> AppSignals
    Workflow --> AppSignals
    Kiro --> MCP
    MCP --> AppSignals
    AppSignals --> Traces
    AppSignals --> SLO
    AppSignals --> CloudWatch
```

GitHub Action と MCP サーバーにより、開発ワークフローにオブザーバビリティが統合されます。

## サービスアップデートの詳細

### 主要機能

1. **GitHub Action (Application Observability for AWS)**
   - GitHub Issues で @awsapm をメンションして質問
   - SLO 違反やクリティカルなサービスエラーを GitHub ワークフローでキャッチ
   - コンソール切り替えなしでオブザーバビリティベースの回答を取得
   - 例: 「Why is my checkout service experiencing high latency?」

2. **MCP サーバー改善**
   - Kiro などの AI コーディングエージェントで使用可能
   - レイテンシ、エラー、SLO 違反の原因となるファイル、関数、コード行を特定
   - 例: 「Which line of code caused the latency spike in my service?」

3. **Infrastructure-as-Code 自動修正**
   - CDK、Terraform などの IaC を自動修正
   - ECS、EKS、Lambda、EC2 向けの OTel ベース APM を設定
   - コーディング作業なしでインストルメンテーションを追加

## 技術仕様

### GitHub Action の機能

| 機能 | 説明 |
|------|------|
| @awsapm メンション | GitHub Issues でオブザーバビリティクエリを実行 |
| SLO モニタリング | SLO 違反を GitHub ワークフローで検出 |
| サービスエラー検出 | クリティカルなエラーをアラート |

### MCP サーバーの機能

| 機能 | 説明 |
|------|------|
| コード行特定 | レイテンシ/エラーの原因コードを特定 |
| IaC 修正 | CDK/Terraform の自動修正 |
| インストルメンテーション | OTel ベース APM の自動設定 |

### サポートされるサービス

| サービス | IaC 自動設定 |
|----------|-------------|
| Amazon ECS | ✅ |
| Amazon EKS | ✅ |
| AWS Lambda | ✅ |
| Amazon EC2 | ✅ |

## 設定方法

### 前提条件

1. AWS アカウントと Application Signals の有効化
2. GitHub リポジトリ
3. 適切な IAM 権限

### 手順

#### ステップ 1: GitHub Action の設定

```yaml
name: Application Observability
on:
  issues:
    types: [opened, edited]

jobs:
  observability:
    runs-on: ubuntu-latest
    steps:
      - uses: aws-actions/application-observability-for-aws@v1
        with:
          aws-region: ap-northeast-1
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

GitHub Action を設定して、Issues でのオブザーバビリティクエリを有効にします。

#### ステップ 2: GitHub Issues での使用

```markdown
@awsapm Why is my checkout service experiencing high latency?
```

GitHub Issues で @awsapm をメンションして質問します。

#### ステップ 3: MCP サーバーの設定（Kiro）

```json
{
  "mcpServers": {
    "cloudwatch-applicationsignals": {
      "command": "uvx",
      "args": ["awslabs.cloudwatch-applicationsignals-mcp-server@latest"],
      "env": {
        "AWS_REGION": "ap-northeast-1"
      }
    }
  }
}
```

Kiro で MCP サーバーを設定します。

#### ステップ 4: MCP サーバーでの質問

```
Which line of code caused the latency spike in my checkout service?
```

AI コーディングエージェントで質問して、問題のコード行を特定します。

## メリット

### ビジネス面

- **トラブルシューティング時間短縮**: コンテキストスイッチングの削減
- **開発者生産性向上**: GitHub を離れずに問題を解決
- **オンボーディング簡素化**: IaC 自動修正でオブザーバビリティ設定

### 技術面

- **コード行レベルの特定**: 問題の根本原因を正確に特定
- **AI 支援デバッグ**: インテリジェントなトラブルシューティング
- **自動インストルメンテーション**: コーディング不要の APM 設定

## デメリット・制約事項

### 制限事項

- Application Signals が利用可能なリージョンでのみ使用可能
- GitHub Action は GitHub リポジトリでのみ使用可能
- MCP サーバーは対応する AI コーディングエージェントが必要

### 考慮すべき点

- AWS 認証情報の安全な管理
- GitHub Secrets の適切な設定
- MCP サーバーの設定とメンテナンス

## ユースケース

### ユースケース 1: 本番環境のレイテンシ調査

**シナリオ**: チェックアウトサービスで高レイテンシが発生

**実装例**:
```markdown
# GitHub Issue
@awsapm Why is my checkout service experiencing high latency?
```

**効果**: GitHub を離れずに、レイテンシの原因となるサービスとコード行を特定

### ユースケース 2: SLO 違反の自動検出

**シナリオ**: CI/CD パイプラインで SLO 違反を検出

**実装例**:
```yaml
- uses: aws-actions/application-observability-for-aws@v1
  with:
    check-slo-violations: true
    fail-on-violation: true
```

**効果**: デプロイ前に SLO 違反を検出し、問題のあるコードのマージを防止

### ユースケース 3: オブザーバビリティの自動設定

**シナリオ**: 新しい ECS サービスに APM を設定

**実装例**:
```
MCP: Add OpenTelemetry instrumentation to my ECS service
```

**効果**: CDK/Terraform を自動修正して、OTel ベースの APM を設定

## 料金

GitHub Action と MCP サーバーの追加料金はありません。Application Signals の標準料金が適用されます。

| 項目 | 料金 |
|------|------|
| Application Signals | バンドル料金 |
| GitHub Action | 無料 |
| MCP サーバー | 無料（オープンソース） |

## 利用可能リージョン

Application Signals が利用可能なすべてのリージョンで使用できます。

## 関連サービス・機能

- **Amazon CloudWatch**: オブザーバビリティプラットフォーム
- **AWS X-Ray**: 分散トレーシング
- **OpenTelemetry**: オープンソースのオブザーバビリティフレームワーク

## 参考リンク

- [公式発表 (What's New)](https://aws.amazon.com/about-aws/whats-new/2025/11/amazon-cloudwatch-application-signals-adds-github-action-mcp-server-improvements/)
- [GitHub Action ドキュメント](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Service-Application-Observability-for-AWS-GitHub-Action.html)
- [MCP サーバードキュメント](https://awslabs.github.io/mcp/servers/cloudwatch-applicationsignals-mcp-server)
- [GitHub Marketplace](https://github.com/marketplace/actions/application-observability-for-aws)

## まとめ

Amazon CloudWatch Application Signals の GitHub Action と MCP サーバー改善により、開発ワークフローにオブザーバビリティが統合されました。GitHub Issues での @awsapm メンション、AI コーディングエージェントでのコード行特定、IaC 自動修正により、トラブルシューティングが大幅に効率化されます。
