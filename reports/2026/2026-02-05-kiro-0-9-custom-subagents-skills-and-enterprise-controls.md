# Kiro 0.9 - カスタムサブエージェント・スキル・エンタープライズ制御

**リリース日**: 2026 年 2 月 5 日
**サービス**: Kiro
**機能**: カスタムサブエージェント、Agent Skills、エンタープライズ制御

📊 [このアップデートのインフォグラフィックを見る](https://takech9203.github.io/aws-news-summary/20260205-kiro-0-9-custom-subagents-skills-and-enterprise-controls.html)

## 概要

Kiro 0.9 がリリースされ、開発者とエンタープライズチーム向けの大幅な機能強化が行われました。このリリースでは、カスタムサブエージェント、Agent Skills サポート、スマートリファクタリングツール、およびエンタープライズ向けのガバナンス制御が追加されています。

カスタムサブエージェントにより、開発者は特定のタスクに特化した独自のエージェントを定義できます。Agent Skills は、エージェントに新しい機能を教えるモジュラーな指示パッケージで、必要な時にのみコンテキストに読み込まれます。また、言語サーバーを活用したスマートリファクタリングツールにより、エージェントによるリファクタリング操作がより確実かつ高速になりました。

**アップデート前の課題**

- サブエージェントは汎用的で、特定のユースケースにカスタマイズできなかった
- 専門知識がコンテキストウィンドウを常に占有し、関係ないタスクでも消費されていた
- リファクタリング操作でシンボル参照の更新漏れやターン数の増加が発生していた

**アップデート後の改善**

- カスタムサブエージェントで特定タスクに特化したエージェントを作成可能
- Agent Skills により専門知識が必要な時にのみコンテキストに読み込まれる
- スマートリファクタリングで確実かつ高速なコード変更が可能

## サービスアップデートの詳細

### 主要機能

1. **カスタムサブエージェント**
   - 特定タスクに特化したサブエージェントを定義
   - フロントエンド用、バックエンド用など用途別に分離
   - 各サブエージェントが独自のコンテキストを管理

2. **Agent Skills**
   - agentskills.io 標準に基づくモジュラー指示パッケージ
   - プログレッシブディスクロージャーで必要時のみロード
   - ユーザーレベル (~/.kiro/skills/) またはワークスペースレベルでインストール

3. **スマートリファクタリング**
   - 言語サーバーを活用した IDE リファクタリング機能
   - シンボル名変更やファイル移動の確実な実行
   - ビルド・コンパイル・ランタイムエラーの削減

4. **エンタープライズ制御**
   - 拡張機能レジストリガバナンス: 独自の VS Code 拡張機能レジストリを指定可能
   - Web ツールトグル: Web 検索・フェッチの有効/無効を制御

## 技術仕様

### カスタムサブエージェントの定義

```markdown
<!-- .kiro/agents/frontend-agent.md -->
# Frontend Agent

フロントエンド開発に特化したサブエージェント。

## 使用ツール
- Chrome DevTools
- Component libraries
- CSS frameworks

## 除外ツール
- Database MCP servers
- Backend API docs
```

### Agent Skills の構造

```
~/.kiro/skills/
└── terraform-skill/
    ├── SKILL.md
    └── scripts/
        └── validate.sh
```

### Steering vs Skills vs Powers

| 機能 | 目的 | ロードタイミング |
|------|------|-----------------|
| Steering | 常時適用される指針 | 常に読み込み |
| Skills | 専門知識・能力 | 必要時のみ |
| Powers | MCP サーバー + ルール | 設定時 |

## 設定方法

### 前提条件

1. Kiro IDE または CLI のバージョン 0.9 以上
2. 必要に応じて ~/.kiro/skills/ ディレクトリ

### 手順

#### ステップ 1: カスタムサブエージェントの作成

`.kiro/agents/` ディレクトリにマークダウンファイルを作成します。

```bash
mkdir -p .kiro/agents
cat > .kiro/agents/backend-agent.md << EOF
# Backend Agent

Python バックエンドに特化したサブエージェント。

## コンテキスト
- Database MCP server
- API documentation
- Python type hints

## 除外
- Frontend frameworks
- CSS tools
EOF
```

プロジェクトに合わせたサブエージェントを定義します。

#### ステップ 2: Agent Skills のインストール

```bash
# グローバルスキル
mkdir -p ~/.kiro/skills/security-review
cat > ~/.kiro/skills/security-review/SKILL.md << EOF
# Security Review Skill

コードのセキュリティレビューを実行する専門スキル。

## トリガー
- "security review" と言及された時
- セキュリティ関連のファイル変更時
EOF

# プロジェクトスキル
mkdir -p .kiro/skills/project-conventions
cat > .kiro/skills/project-conventions/SKILL.md << EOF
# Project Conventions Skill

このプロジェクト固有のコーディング規約。
EOF
```

スキルは名前と説明のみが起動時に読み込まれ、関連性が判断された時に詳細がロードされます。

#### ステップ 3: エンタープライズ設定

組織管理者は、設定ファイルでガバナンス制御を行います。

```json
{
  "enterprise": {
    "extensionRegistry": "https://internal-registry.company.com",
    "webTools": {
      "webSearch": false,
      "webFetch": true
    }
  }
}
```

## メリット

### ビジネス面

- **開発効率向上**: 特化したサブエージェントで作業効率が向上
- **ガバナンス強化**: エンタープライズ向けの制御機能
- **コスト最適化**: コンテキストウィンドウの効率的な使用

### 技術面

- **コンテキスト管理**: 必要な情報のみをロードして効率化
- **リファクタリング品質**: 言語サーバー連携で確実な変更
- **拡張性**: モジュラーなスキルシステムで機能拡張

## デメリット・制約事項

### 制限事項

- カスタムサブエージェントの設定には学習コストがある
- スキルの作成にはマークダウン形式の理解が必要
- エンタープライズ機能は組織管理者権限が必要

### 考慮すべき点

- サブエージェント間の責任分担の設計
- スキルの粒度と再利用性のバランス
- 組織全体でのスキル標準化

## ユースケース

### ユースケース 1: フルスタック開発のコンテキスト分離

**シナリオ**: React フロントエンドと Python バックエンドを持つプロジェクト

**実装例**:
```
.kiro/agents/
├── frontend-agent.md  # React, Chrome DevTools
└── backend-agent.md   # Python, Database
```

**効果**: 各サブエージェントが専門分野に集中し、コンテキストオーバーフローを防止

### ユースケース 2: セキュリティレビューの自動化

**シナリオ**: コード変更時に自動でセキュリティレビューを実行

**実装例**:
```markdown
# Security Review Skill

## トリガー条件
- authentication, authorization に関連するファイル変更
- .env, credentials に関連するファイル変更

## レビュー項目
- SQL インジェクション
- XSS 脆弱性
- 認証情報のハードコーディング
```

**効果**: セキュリティ専門知識が必要な時のみスキルがアクティブになり、効率的にレビュー

### ユースケース 3: エンタープライズ拡張機能管理

**シナリオ**: 承認済み拡張機能のみを開発者に提供

**実装例**:
```json
{
  "enterprise": {
    "extensionRegistry": "https://approved-extensions.company.com"
  }
}
```

**効果**: セキュリティチームが事前承認した拡張機能のみが利用可能

## 料金

Kiro の料金については [Kiro 料金ページ](https://kiro.dev/pricing/) を参照してください。

## 利用可能リージョン

グローバル

## 関連サービス・機能

- **Claude Opus 4.6**: Kiro 0.9 と同時にサポート開始
- **ACP (Agent Client Protocol)**: JetBrains IDE や Zed との連携
- **MCP (Model Context Protocol)**: 外部ツールとの統合

## 参考リンク

- 📊 [インフォグラフィック](https://takech9203.github.io/aws-news-summary/20260205-kiro-0-9-custom-subagents-skills-and-enterprise-controls.html)
- [公式ブログ](https://kiro.dev/blog/custom-subagents-skills-and-enterprise-controls/)
- [Changelog](https://kiro.dev/changelog/)
- [ドキュメント](https://kiro.dev/docs/)
- [リファクタリングの詳細](https://kiro.dev/blog/refactoring-made-right/)

## まとめ

Kiro 0.9 は、開発者の生産性とエンタープライズのガバナンス要件の両方を満たす重要なアップデートです。カスタムサブエージェントと Agent Skills により、コンテキスト管理が大幅に改善され、スマートリファクタリングでコード変更の品質が向上します。大規模なコードベースを扱う開発チームや、ガバナンス要件の厳しいエンタープライズ環境では、ぜひこれらの新機能を活用してください。
