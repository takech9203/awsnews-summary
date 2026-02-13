# AWS ライトテーマ（プレゼンテーション風）

AWS 公式プレゼンテーションスタイルに準拠したライトテーマ。白背景を基調とし、AWS Orange をアクセントカラーとして使用。ドキュメント、ホワイトペーパー、印刷物に最適な 1 カラム構成 (最大幅 1000px) です。

## カラーパレット

AWS 公式ブランドカラーに基づいたライトテーマのカラーパレットです。

```xml
<palette name="aws-light">
  <!-- Primary Colors -->
  <color name='White' rgb='FFFFFF' r='255' g='255' b='255' description='メイン背景色' />
  <color name='AWS Orange' rgb='FF9900' r='255' g='153' b='0' description='主要アクセント、CTA、重要な強調' />
  <color name='Smile Orange' rgb='FFAD33' r='255' g='173' b='51' description='セカンダリアクセント、ホバー状態' />
  
  <!-- Text Colors -->
  <color name='Squid Ink' rgb='232F3E' r='35' g='47' b='62' description='メインテキスト、H1' />
  <color name='Dark Gray' rgb='545B64' r='84' g='91' b='100' description='本文テキスト、説明文' />
  <color name='Medium Gray' rgb='687078' r='104' g='112' b='120' description='サブテキスト' />
  
  <!-- Supporting Colors -->
  <color name='Light Gray' rgb='F2F3F3' r='242' g='243' b='243' description='カード背景、セクション区切り' />
  <color name='Border Gray' rgb='E9EBED' r='233' g='235' b='237' description='ボーダー、区切り線' />
  <color name='Teal' rgb='007EB9' r='0' g='126' b='185' description='リンク、インタラクティブ要素' />
  <color name='Purple' rgb='8C4FFF' r='140' g='79' b='255' description='AI/ML 関連の強調' />
  <color name='Green' rgb='1D8102' r='29' g='129' b='2' description='成功状態、ポジティブな指標' />
  <color name='Red' rgb='D13212' r='209' g='50' b='18' description='警告、エラー状態' />
  
  <!-- Accent Backgrounds -->
  <color name='Orange Tint' rgb='FFF4E6' r='255' g='244' b='230' description='オレンジ系の薄い背景' />
  <color name='Teal Tint' rgb='E6F4F9' r='230' g='244' b='249' description='ティール系の薄い背景' />
  <color name='Purple Tint' rgb='F3EDFF' r='243' g='237' b='255' description='パープル系の薄い背景' />
</palette>
```

## デザインガイドライン

### 1. カラースキーム

AWS 公式カラーパレットのライトバリエーションを使用します。

- **背景**: 白 (#FFFFFF) をメイン背景に使用
- **タイトル**: Squid Ink (#232F3E)、AWS Orange (#FF9900) でアクセント
- **アイコン**: オレンジ (#FF9900) または Squid Ink (#232F3E)
- **カード**: Light Gray (#F2F3F3) 背景、角丸 8px、微細なボーダー

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

- キーワードの強調 (オレンジ系の薄い背景、マーカー効果)
- 重要な数値は大きなフォントで表示
- 引用や注意事項は囲み枠で区別

### 3. タイポグラフィ

テキスト階層を明確にします。

- **タイトル**: 36px, #232F3E, font-weight: 700
- **サブタイトル**: 16px, #687078
- **セクション見出し**: 24px, #232F3E, font-weight: 600、オレンジのアンダーライン
- **本文**: 16px, #545B64, line-height: 1.6
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
- **カード型コンポーネント**: Light Gray 背景、角丸 8px、微細なボーダー
- **セクション間の適切な余白と階層構造**
- **コンテンツの横幅は 100%**

## CSS テンプレート

```css
/* AWS Light Theme Base Styles */
:root {
  --aws-white: #FFFFFF;
  --aws-squid-ink: #232F3E;
  --aws-orange: #FF9900;
  --aws-smile-orange: #FFAD33;
  --aws-dark-gray: #545B64;
  --aws-medium-gray: #687078;
  --aws-light-gray: #F2F3F3;
  --aws-border-gray: #E9EBED;
  --aws-teal: #007EB9;
  --aws-purple: #8C4FFF;
  --aws-green: #1D8102;
  --aws-red: #D13212;
  --aws-orange-tint: #FFF4E6;
  --aws-teal-tint: #E6F4F9;
  --aws-purple-tint: #F3EDFF;
}

body {
  background-color: var(--aws-white);
  font-family: 'Noto Sans JP', 'Hiragino Kaku Gothic ProN', 'Hiragino Sans', Meiryo, sans-serif;
  color: var(--aws-dark-gray);
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
  padding-bottom: 16px;
  border-bottom: 3px solid var(--aws-orange);
}

h1, .title {
  font-size: 36px;
  font-weight: 700;
  color: var(--aws-squid-ink);
  margin: 0;
}

h2, .section-title {
  font-size: 24px;
  font-weight: 600;
  color: var(--aws-squid-ink);
  margin-bottom: 16px;
  display: flex;
  align-items: center;
  gap: 8px;
  padding-bottom: 8px;
  border-bottom: 2px solid var(--aws-orange);
}

.subtitle {
  font-size: 16px;
  color: var(--aws-medium-gray);
}

.section {
  background: var(--aws-light-gray);
  border-radius: 8px;
  padding: 24px;
  margin-bottom: 24px;
  border: 1px solid var(--aws-border-gray);
}

.highlight {
  background-color: var(--aws-orange-tint);
  padding: 2px 6px;
  border-radius: 4px;
  font-weight: 600;
  color: var(--aws-squid-ink);
  border-bottom: 2px solid var(--aws-orange);
}

.card {
  background: var(--aws-white);
  border-radius: 8px;
  padding: 16px;
  margin: 12px 0;
  border-left: 4px solid var(--aws-orange);
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
}

.icon-text {
  display: flex;
  align-items: center;
  gap: 8px;
  margin: 8px 0;
  color: var(--aws-dark-gray);
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
  color: var(--aws-orange);
  text-decoration: underline;
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
  background: var(--aws-white);
  border: 2px solid var(--aws-orange);
  border-radius: 8px;
  padding: 12px 20px;
  text-align: center;
  min-width: 100px;
  color: var(--aws-squid-ink);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.08);
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
  background: var(--aws-squid-ink);
  border-radius: 8px;
  padding: 16px;
  font-family: 'Courier New', monospace;
  font-size: 14px;
  overflow-x: auto;
  color: #D5DBDB;
}

/* テーブルスタイル */
table {
  width: 100%;
  border-collapse: collapse;
  margin: 16px 0;
}

th {
  background: var(--aws-squid-ink);
  color: var(--aws-white);
  padding: 12px;
  text-align: left;
  font-weight: 600;
}

td {
  padding: 12px;
  border-bottom: 1px solid var(--aws-border-gray);
}

tr:nth-child(even) {
  background: var(--aws-light-gray);
}

/* 注意・警告ボックス */
.note-box {
  background: var(--aws-teal-tint);
  border-left: 4px solid var(--aws-teal);
  padding: 16px;
  border-radius: 0 8px 8px 0;
  margin: 16px 0;
}

.warning-box {
  background: var(--aws-orange-tint);
  border-left: 4px solid var(--aws-orange);
  padding: 16px;
  border-radius: 0 8px 8px 0;
  margin: 16px 0;
}

.ai-box {
  background: var(--aws-purple-tint);
  border-left: 4px solid var(--aws-purple);
  padding: 16px;
  border-radius: 0 8px 8px 0;
  margin: 16px 0;
}

footer {
  margin-top: 24px;
  padding-top: 16px;
  border-top: 1px solid var(--aws-border-gray);
  font-size: 12px;
  color: var(--aws-medium-gray);
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
    
    <div class="note-box">
      <strong>💡 ヒント:</strong> これは情報ボックスの例です。
    </div>
    
    <div class="warning-box">
      <strong>⚠️ 注意:</strong> これは警告ボックスの例です。
    </div>
    
    <div class="ai-box">
      <strong>🤖 AI/ML:</strong> これは AI/ML 関連の情報ボックスの例です。
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
- 「ライト」「ライトテーマ」「白背景」「明るい」
- 「ドキュメント」「ホワイトペーパー」「印刷」
