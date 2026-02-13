# AWS ダークテーマ（プレゼンテーション風）

AWS 公式プレゼンテーションスタイルに準拠したダークテーマ。Squid Ink (#232F3E) を背景に使用し、技術資料やクラウドアーキテクチャの説明に最適な 1 カラム構成 (最大幅 1000px) です。

## カラーパレット

AWS 公式ブランドカラーに基づいたダークテーマのカラーパレットです。

```xml
<palette name="aws-dark">
  <!-- Primary Colors -->
  <color name='Squid Ink' rgb='232F3E' r='35' g='47' b='62' description='メイン背景色' />
  <color name='AWS Orange' rgb='FF9900' r='255' g='153' b='0' description='主要アクセント、CTA、重要な強調' />
  <color name='Smile Orange' rgb='FFAD33' r='255' g='173' b='51' description='セカンダリアクセント、ホバー状態' />
  
  <!-- Text Colors -->
  <color name='White' rgb='FFFFFF' r='255' g='255' b='255' description='メインテキスト、H1' />
  <color name='Light Gray' rgb='D5DBDB' r='213' g='219' b='219' description='本文テキスト、説明文' />
  
  <!-- Supporting Colors -->
  <color name='Dark Squid Ink' rgb='1A242F' r='26' g='36' b='47' description='カード背景、セクション区切り' />
  <color name='Teal' rgb='00A1C9' r='0' g='161' b='201' description='リンク、インタラクティブ要素' />
  <color name='Purple' rgb='8C4FFF' r='140' g='79' b='255' description='AI/ML 関連の強調' />
  <color name='Green' rgb='1D8102' r='29' g='129' b='2' description='成功状態、ポジティブな指標' />
  <color name='Red' rgb='D13212' r='209' g='50' b='18' description='警告、エラー状態' />
</palette>
```

## デザインガイドライン

### 1. カラースキーム

AWS 公式カラーパレットを使用します。

- **背景**: Squid Ink (#232F3E) をメイン背景に使用
- **タイトル**: 白文字 (#FFFFFF)、AWS Orange (#FF9900) でサブタイトルを強調
- **アイコン**: オレンジ (#FF9900) のアウトラインスタイル、線幅 2px
- **カード**: Dark Squid Ink (#1A242F) 背景、角丸 8px、微細なボーダー

### 2. ビジュアル表現

AWS プレゼンテーション風の分かりやすいビジュアル要素を積極的に使用します。

#### アイコンと絵文字

- 絵文字やアイコンを効果的に配置 (☁️🔧⚙️🚀📊🔒✅❌⚡💡🎯など)
- 各セクションの見出しにはアイコンを必ず付ける
- 箇条書きの先頭にチェックマーク (✓) や矢印 (→) を使用

#### フロー図・プロセス図

ステップやプロセスは横並びのフロー図で表現します。

```html
<div class="flow-diagram">
  <div class="flow-step">💬 入力</div>
  <span class="flow-arrow">→</span>
  <div class="flow-step">⚙️ 処理</div>
  <span class="flow-arrow">→</span>
  <div class="flow-step">✅ 出力</div>
</div>
```

#### 比較表・機能一覧

- 機能や特徴は表形式またはカード形式で整理
- ✓/✗ マークで対応状況を明示
- 重要な項目はハイライト表示

#### 強調表現

- キーワードの強調 (色付き背景、マーカー効果)
- 重要な数値は大きなフォントで表示
- 引用や注意事項は囲み枠で区別

### 3. タイポグラフィ

テキスト階層を明確にします。

- **タイトル**: 36px, #FFFFFF, font-weight: 700
- **サブタイトル**: 16px, #D5DBDB
- **セクション見出し**: 24px, #FF9900, font-weight: 600
- **本文**: 16px, #D5DBDB, line-height: 1.6
- **フォント**: Noto Sans JP (代替: Hiragino Kaku Gothic ProN, Hiragino Sans, Meiryo, sans-serif)

```html
<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;600;700&display=swap');

body {
  font-family: 'Noto Sans JP', 'Hiragino Kaku Gothic ProN', 'Hiragino Sans', Meiryo, sans-serif;
}
</style>
```

### 4. レイアウト

- **ヘッダー**: 左揃えタイトル＋右揃え日付/出典
- **1 カラム構成**: 中央配置、最大幅 1000px
- **カード型コンポーネント**: Dark Squid Ink 背景、角丸 8px、微細なボーダー
- **セクション間の適切な余白と階層構造**
- **コンテンツの横幅は 100%**

## CSS テンプレート

```css
/* AWS Dark Theme Base Styles */
:root {
  --aws-squid-ink: #232F3E;
  --aws-dark-squid: #1A242F;
  --aws-orange: #FF9900;
  --aws-smile-orange: #FFAD33;
  --aws-white: #FFFFFF;
  --aws-light-gray: #D5DBDB;
  --aws-teal: #00A1C9;
  --aws-purple: #8C4FFF;
  --aws-green: #1D8102;
  --aws-red: #D13212;
}

body {
  background-color: var(--aws-squid-ink);
  font-family: 'Noto Sans JP', 'Hiragino Kaku Gothic ProN', 'Hiragino Sans', Meiryo, sans-serif;
  color: var(--aws-light-gray);
  font-size: 16px;
  line-height: 1.6;
  padding: 20px;
  margin: 0;
}

.container {
  max-width: 1000px;
  margin: 0 auto;
  width: 100%;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 32px;
  flex-wrap: wrap;
}

h1, .title {
  font-size: 36px;
  font-weight: 700;
  color: var(--aws-white);
  margin: 0;
}

h2, .section-title {
  font-size: 24px;
  font-weight: 600;
  color: var(--aws-orange);
  margin-bottom: 16px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.subtitle {
  font-size: 16px;
  color: var(--aws-light-gray);
}

.section {
  background: var(--aws-dark-squid);
  border-radius: 8px;
  padding: 24px;
  margin-bottom: 24px;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.highlight {
  background-color: rgba(255, 153, 0, 0.2);
  padding: 2px 6px;
  border-radius: 4px;
  font-weight: 600;
  color: var(--aws-orange);
}

.card {
  background: var(--aws-dark-squid);
  border-radius: 8px;
  padding: 16px;
  margin: 12px 0;
  border: 1px solid rgba(255, 153, 0, 0.3);
}

.icon-text {
  display: flex;
  align-items: center;
  gap: 8px;
  margin: 8px 0;
  color: var(--aws-light-gray);
}

.arrow {
  color: var(--aws-orange);
  font-size: 20px;
}

.aws-link {
  color: var(--aws-teal);
  text-decoration: none;
}

.aws-link:hover {
  color: var(--aws-smile-orange);
}

/* フロー図スタイル */
.flow-diagram {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  flex-wrap: wrap;
  margin: 24px 0;
}

.flow-step {
  background: var(--aws-dark-squid);
  border: 2px solid var(--aws-orange);
  border-radius: 8px;
  padding: 12px 20px;
  text-align: center;
  min-width: 100px;
}

.flow-arrow {
  color: var(--aws-orange);
  font-size: 24px;
  font-weight: bold;
}

/* チェックリストスタイル */
.check-item {
  display: flex;
  align-items: flex-start;
  gap: 8px;
  margin: 8px 0;
}

.check-icon {
  color: var(--aws-green);
  font-size: 18px;
}

.cross-icon {
  color: var(--aws-red);
  font-size: 18px;
}

/* コードブロック */
.code-block {
  background: var(--aws-dark-squid);
  border: 1px solid rgba(255, 153, 0, 0.2);
  border-radius: 8px;
  padding: 16px;
  font-family: 'Courier New', monospace;
  font-size: 14px;
  overflow-x: auto;
  color: var(--aws-light-gray);
}

footer {
  margin-top: 24px;
  padding-top: 16px;
  border-top: 1px solid rgba(255, 255, 255, 0.2);
  font-size: 12px;
  color: var(--aws-light-gray);
}
```

## HTML テンプレート例

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AWS アーキテクチャ概要</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;600;700&display=swap');
    
    /* CSS テンプレートをここに挿入 */
  </style>
</head>
<body>
  <div class="container">
    <div class="header">
      <h1>☁️ AWS アーキテクチャ概要</h1>
      <div class="subtitle">2026-01-09</div>
    </div>
    
    <div class="section">
      <h2>🔧 セクション 1</h2>
      <p>本文テキスト。<span class="highlight">重要なポイント</span>を強調します。</p>
      
      <div class="card">
        <div class="icon-text">
          <span class="arrow">→</span>
          <span>カード内のコンテンツ</span>
        </div>
      </div>
    </div>
    
    <div class="section">
      <h2>⚡ フロー図の例</h2>
      <div class="flow-diagram">
        <div class="flow-step">💬 入力</div>
        <span class="flow-arrow">→</span>
        <div class="flow-step">⚙️ 処理</div>
        <span class="flow-arrow">→</span>
        <div class="flow-step">✅ 出力</div>
      </div>
    </div>
    
    <footer>
      <p>📎 出典: <a href="#" target="_blank" class="aws-link">ソース URL</a></p>
    </footer>
  </div>
</body>
</html>
```

## 適用キーワード

以下のキーワードが含まれる場合、このテーマを適用します。

- 「AWS」「クラウド」「アーキテクチャ」「技術資料」
- 「ダーク」「ダークテーマ」「ダークモード」
- 「プレゼン風」「Squid Ink」

## Mermaid 図の設定

ダークテーマの背景でも図の可読性を確保するため、Mermaid 図の背景は必ず白にすること。

### 必須: Mermaid 初期化設定

`theme: 'dark'` は使用禁止。必ず `theme: 'base'` を使用し、白背景を指定すること。

```html
<script src="https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.min.js"></script>
<script>
    mermaid.initialize({
        startOnLoad: true,
        theme: 'base',
        themeVariables: {
            primaryColor: '#FF9900',
            primaryTextColor: '#232F3E',
            primaryBorderColor: '#FF9900',
            lineColor: '#FF9900',
            secondaryColor: '#F5F5F5',
            tertiaryColor: '#FAFAFA',
            background: '#FFFFFF',
            mainBkg: '#FFF3E0',
            nodeBorder: '#FF9900',
            clusterBkg: '#FFF8E1',
            clusterBorder: '#FF9900',
            edgeLabelBackground: '#FFFFFF',
            textColor: '#232F3E'
        }
    });
</script>
```

### 必須: Mermaid コンテナの CSS

Mermaid 図は白背景のコンテナで囲むこと。

```css
.mermaid-container {
    background: #FFFFFF;
    border-radius: 12px;
    padding: 24px;
    margin: 20px 0;
    border: 2px solid rgba(255, 153, 0, 0.3);
    overflow-x: auto;
}
.mermaid {
    display: flex;
    justify-content: center;
}
```

### 必須: HTML 構造

Mermaid 図は必ず `.mermaid-container` で囲むこと。

```html
<div class="mermaid-container">
    <pre class="mermaid">
flowchart TD
    A["コンポーネント A"] --> B["コンポーネント B"]
    </pre>
</div>
```

### 禁止事項

以下の設定は使用しないこと。

- `theme: 'dark'` — 図のテキストが読みにくくなるため禁止
- `background: '#232F3E'` — Mermaid 図の背景を Squid Ink にしない
- `.mermaid-container` なしで `<pre class="mermaid">` を直接配置すること
