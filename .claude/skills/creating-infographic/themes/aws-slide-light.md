# AWS スライドライトテーマ（複数ページ PDF 対応）

AWS 公式プレゼンテーションスタイル (re:Invent、AWS Summit、ウェビナー等) に準拠したライトテーマのスライド形式。PDF 印刷時に複数ページとして出力可能。

## スライドサイズ

印刷時のサイズは以下を想定しています。

- **幅**: 25.4 cm (960px)
- **高さ**: 14.29 cm (540px)
- **アスペクト比**: 16:9
- **解像度**: 96 DPI 相当

CSS 変数で定義:
```css
--slide-width: 960px;
--slide-height: 540px;
```

## エージェント向け重要指示

このテーマでスライドを生成する際は、以下の点を厳守してください。

### 必須ルール

1. **フォントサイズは大きめに**: スライドは遠くから見ることを想定。本文は最低 20px、タイトルは 32px 以上
2. **1 スライド 1 メッセージ**: 情報を詰め込みすぎない。箇条書きは 3〜4 項目まで
3. **余白を十分に確保**: padding は 40px 以上、要素間の gap は 16px 以上
4. **オレンジのアクセント**: 左側に 6px のオレンジバー、重要な要素にオレンジを使用
5. **左揃え基本**: タイトルスライドを含むすべてのスライドで左揃え
6. **絵文字を活用**: タイトル、カード見出し、リストアイコンに積極的に使用。ただし文章内の過剰な使用は避ける

### ⚠️ フッター著作権表示 (最重要・必須)

**すべてのスライドに以下の著作権表示を必ず含めること。これは絶対に省略してはならない。**

```html
<div class="slide-footer">
  <span>© 2026, Amazon Web Services, Inc. or its affiliates. All rights reserved.</span>
  <span class="slide-number">ページ番号</span>
</div>
```

**フッターのルール**:

1. **著作権表示は固定文言**: `© [year], Amazon Web Services, Inc. or its affiliates. All rights reserved.` を必ず使用
2. **年は生成時の年**: 2026 年なら `© 2026, Amazon Web Services, Inc. ...`
3. **すべてのスライドに必須**: タイトルスライド、セクションスライド、コンテンツスライド、出典スライドすべてに含める
4. **省略禁止**: 「AWS Frontier Agents」のような短縮形は使用しない
5. **セクションスライドの色調整**: 背景が暗い場合はフッターの色を `rgba(255,255,255,0.6)` に変更

**❌ 間違った例**:
```html
<div class="slide-footer">
  <span>AWS Frontier Agents</span>  <!-- NG: 著作権表示がない -->
  <span class="slide-number">4</span>
</div>
```

**✅ 正しい例**:
```html
<div class="slide-footer">
  <span>© 2026, Amazon Web Services, Inc. or its affiliates. All rights reserved.</span>
  <span class="slide-number">4</span>
</div>
```

### はみ出し防止ルール (重要)

スライドサイズは 960px × 540px と限られています。コンテンツがスライド外にはみ出さないよう、以下を厳守してください。

1. **スライド内に収める**: すべての要素 (アジェンダ、フロー図、カード、リスト、テーブル) がスライド内に完全に収まるようにする
2. **フッター領域を確保**: コンテンツ領域の下部に 60px 以上の余白を残す
3. **はみ出す場合は調整**: コンテンツが多い場合は以下のいずれかで対応
   - フォントサイズを小さくする
   - 余白・gap を詰める
   - 複数スライドに分割する
   - テキストを短く要約する
4. **フロー図の折り返し**: 横幅に収まらない場合は 2 行に分けて表示
5. **カード内は簡潔に**: 長いテキストは避け、キーワードや短いフレーズで表現

### ビジュアル表現の積極活用

テキストだけのスライドは避け、以下のビジュアル要素を積極的に使用してください。

1. **フロー図・プロセス図**: 手順やワークフローは必ず `.flow-diagram` で視覚化
2. **カードレイアウト**: 複数の概念や機能は `.card` + グリッドで並列表示
3. **アイコン・絵文字**: 各項目の先頭にアイコンを配置して視認性向上
4. **比較表**: 2 つ以上の項目を比較する場合は `<table>` を使用
5. **ハイライト**: 重要なキーワードは `.highlight` で強調
6. **オレンジアンダーライン**: テキストに太いオレンジの下線を引く `.underline-orange` で強調
7. **ノートボックス**: 補足情報は `.note-box`、警告は `.warning-box` で囲む
8. **2/3 カラムレイアウト**: 情報を横に並べて一覧性を高める

### ビジュアル表現の選択ガイド

| コンテンツタイプ | 推奨ビジュアル |
|-----------------|---------------|
| 手順・プロセス | フロー図 (`.flow-diagram`) |
| 機能一覧・特徴 | カードグリッド (`.three-column` + `.card`) |
| 比較・対照 | テーブル (`<table>`) または 2 カラム |
| 数値・統計 | 大きな数字 + 説明テキスト |
| 関係性・構造 | フロー図またはネスト構造 |
| 重要ポイント | ハイライト + アイコン付きリスト |
| キーワード強調 | オレンジアンダーライン (`.underline-orange`) |
| 補足・注意 | ノートボックス (`.note-box`, `.warning-box`) |

### 禁止事項

- 小さすぎるフォント (本文 20px 未満、カード内 18px 未満)
- 1 スライドに 5 項目以上の箇条書き
- 複雑なネスト構造 (2 階層まで)
- 派手なグラデーションやアニメーション
- 情報の詰め込みすぎ
- テキストのみのスライド (必ずビジュアル要素を含める)
- スライド外へのはみ出し (フロー図、カード、リストすべて)

## カラーパレット

AWS 公式ブランドカラーに基づいたライトテーマのカラーパレットです。

```xml
<palette name="aws-slide-light">
  <!-- Primary Colors -->
  <color name='White' rgb='FFFFFF' r='255' g='255' b='255' description='メイン背景色' />
  <color name='AWS Orange' rgb='FF9900' r='255' g='153' b='0' description='主要アクセント、CTA、重要な強調' />
  <color name='Smile Orange' rgb='FFAD33' r='255' g='173' b='51' description='セカンダリアクセント、ホバー状態' />
  
  <!-- Text Colors -->
  <color name='Squid Ink' rgb='232F3E' r='35' g='47' b='62' description='メインテキスト、タイトル' />
  <color name='Dark Gray' rgb='545B64' r='84' g='91' b='100' description='本文テキスト' />
  <color name='Medium Gray' rgb='687078' r='104' g='112' b='120' description='サブテキスト、フッター' />
  
  <!-- Supporting Colors -->
  <color name='Light Gray' rgb='F2F3F3' r='242' g='243' b='243' description='カード背景、セクション区切り' />
  <color name='Border Gray' rgb='E9EBED' r='233' g='235' b='237' description='ボーダー、区切り線' />
  <color name='Teal' rgb='007EB9' r='0' g='126' b='185' description='リンク、インタラクティブ要素' />
  <color name='Purple' rgb='8C4FFF' r='140' g='79' b='255' description='AI/ML 関連の強調' />
  <color name='Green' rgb='1D8102' r='29' g='129' b='2' description='成功状態、ポジティブな指標' />
  <color name='Red' rgb='D13212' r='209' g='50' b='18' description='警告、エラー状態' />
  
  <!-- Tint Colors (背景用の薄い色) -->
  <color name='Orange Tint' rgb='FFF4E6' description='オレンジ系ハイライト背景' />
  <color name='Teal Tint' rgb='E6F4F9' description='ティール系ノート背景' />
  <color name='Purple Tint' rgb='F3EDFF' description='AI/ML 関連背景' />
</palette>
```

## AWS 公式スライドデザインの特徴

AWS 公式プレゼンテーション (re:Invent、Summit、ウェビナー等) のデザインパターンを踏襲します。

### レイアウト原則

- **1 スライド 1 メッセージ**: 各スライドは 1 つの主要なポイントに集中
- **余白を活かす**: 情報を詰め込みすぎず、視覚的な余裕を確保
- **左揃え基本**: タイトルと本文は左揃えが基本
- **オレンジのアクセント**: 左側にオレンジのバーを配置
- **シンプルな背景**: 白または薄いグレーの単色背景

### タイポグラフィ (スライド向け大きめサイズ)

| 要素 | フォントサイズ | ウェイト | 色 |
|------|---------------|---------|-----|
| タイトルスライド見出し | 56px | 700 (Bold) | #232F3E |
| タイトルスライドサブタイトル | 28px | 400 | #687078 |
| スライドタイトル | 40px | 600 (SemiBold) | #232F3E |
| 本文・箇条書き | 26px | 400 | #545B64 |
| 強調テキスト | 26px | 600 | #232F3E |
| フッター・ページ番号 | 16px | 400 | #687078 |

### スライド構成

1. **タイトルスライド**: 左揃え、プレゼンタイトル + サブタイトル + 日付/発表者
2. **アジェンダスライド**: タイトル直後、番号付きリストで目次を表示
3. **セクション区切りスライド**: 新しいセクションの開始を示す。番号 (01, 02...) + タイトルを同一行に配置
4. **コンテンツスライド**: 左揃えタイトル + 本文/箇条書き/図表
5. **まとめスライド**: Key Takeaways を箇条書きで整理
6. **出典スライド**: 参照した情報源のリンクを一覧表示 (最後のスライド)

### ビジュアル要素

- **アイコン**: 各スライドタイトルに絵文字を 1 つ配置
- **箇条書き**: チェックマーク (✓) または矢印 (→) を使用
- **フロー図**: 横並びのステップ表示
- **カード**: 複数の項目を並列表示する際に使用
- **ハイライト**: 重要なキーワードにオレンジ系の背景

## CSS テンプレート

```css
/* AWS Slide Light Theme - PDF Print Ready */
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;600;700&display=swap');

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
  
  --slide-width: 960px;
  --slide-height: 540px;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Noto Sans JP', 'Hiragino Kaku Gothic ProN', 'Hiragino Sans', Meiryo, sans-serif;
  background: #E0E0E0;
  color: var(--aws-dark-gray);
  font-size: 26px;
  line-height: 1.5;
}

/* スライドコンテナ */
.slide-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 40px;
  padding: 40px;
}

/* 個別スライド */
.slide {
  width: var(--slide-width);
  height: var(--slide-height);
  background: var(--aws-white);
  border-radius: 4px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
  padding: 40px 56px 56px;
  display: flex;
  flex-direction: column;
  position: relative;
  overflow: hidden;
}

/* オレンジの左サイドバー */
.slide::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  width: 6px;
  background: var(--aws-orange);
}

/* タイトルスライド */
.slide.title-slide {
  justify-content: center;
  align-items: flex-start;
  text-align: left;
  padding-top: 56px;
}

.slide.title-slide h1 {
  font-size: 56px;
  font-weight: 700;
  color: var(--aws-squid-ink);
  margin-bottom: 20px;
  line-height: 1.2;
}

.slide.title-slide .subtitle {
  font-size: 28px;
  font-weight: 400;
  color: var(--aws-medium-gray);
  margin-bottom: 48px;
  max-width: 900px;
}

.slide.title-slide .meta {
  font-size: 20px;
  color: var(--aws-medium-gray);
}

/* アジェンダスライド */
.slide.agenda-slide h2 {
  font-size: 32px;
  font-weight: 600;
  color: var(--aws-squid-ink);
  margin-bottom: 24px;
  display: flex;
  align-items: center;
  gap: 12px;
}

.agenda-list {
  list-style: none;
  padding: 0;
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  gap: 12px;
  max-height: 380px;
  overflow: hidden;
}

.agenda-list li {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 14px 20px;
  background: var(--aws-light-gray);
  border-radius: 8px;
  border-left: 4px solid var(--aws-orange);
  font-size: 22px;
  color: var(--aws-squid-ink);
}

.agenda-list .number {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 36px;
  height: 36px;
  background: var(--aws-orange);
  color: var(--aws-white);
  border-radius: 50%;
  font-weight: 700;
  font-size: 18px;
  flex-shrink: 0;
}

/* セクション区切りスライド */
.slide.section-slide {
  justify-content: center;
  align-items: flex-start;
  background: var(--aws-squid-ink);
}

.slide.section-slide h2 {
  font-size: 48px;
  font-weight: 700;
  color: var(--aws-white);
  display: flex;
  align-items: baseline;
  gap: 16px;
}

.slide.section-slide .section-number {
  font-size: 32px;
  color: var(--aws-orange);
  font-weight: 600;
}

/* コンテンツスライド */
.slide.content-slide h2 {
  font-size: 32px;
  font-weight: 600;
  color: var(--aws-squid-ink);
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  gap: 12px;
}

.slide-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;  /* 空白防止: コンテンツを上下に分散 */
  gap: 16px;
  max-height: 380px;
  overflow: hidden;
}

/* 本文 */
.slide-content p {
  font-size: 20px;
  line-height: 1.5;
  color: var(--aws-dark-gray);
}

/* 箇条書きリスト */
.content-list {
  list-style: none;
  padding: 0;
}

.content-list li {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  margin-bottom: 12px;
  font-size: 20px;
  line-height: 1.4;
}

.content-list .icon {
  color: var(--aws-orange);
  font-size: 20px;
  flex-shrink: 0;
  margin-top: 2px;
}

/* カード */
.card {
  background: var(--aws-light-gray);
  border-radius: 8px;
  padding: 20px 24px;
  border-left: 4px solid var(--aws-orange);
  overflow: hidden;
}

.card-title {
  font-size: 24px;
  font-weight: 600;
  color: var(--aws-squid-ink);
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  gap: 10px;
}

.card-content {
  font-size: 20px;
  color: var(--aws-dark-gray);
  line-height: 1.5;
}

/* 2 カラムレイアウト */
.two-column {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 24px;
  flex: 1;
  align-items: stretch;  /* 空白防止: カードの高さを揃える */
}

/* 3 カラムレイアウト */
.three-column {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 20px;
  flex: 1;
  align-items: stretch;  /* 空白防止: カードの高さを揃える */
}

/* フロー図 */
.flow-diagram {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  flex-wrap: wrap;
  margin: 16px 0;
  max-width: 100%;
}

.flow-step {
  background: var(--aws-white);
  border: 2px solid var(--aws-orange);
  border-radius: 8px;
  padding: 14px 20px;
  text-align: center;
  min-width: 120px;
  font-size: 18px;
  font-weight: 500;
  color: var(--aws-squid-ink);
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.08);
}

.flow-arrow {
  color: var(--aws-orange);
  font-size: 28px;
  font-weight: bold;
  flex-shrink: 0;
}

/* ハイライト */
.highlight {
  background-color: var(--aws-orange-tint);
  padding: 4px 10px;
  border-radius: 4px;
  font-weight: 600;
  color: var(--aws-squid-ink);
}

/* オレンジアンダーライン装飾 */
.underline-orange {
  display: inline;
  background: linear-gradient(transparent 60%, var(--aws-orange) 60%);
  padding: 0 4px;
}

.underline-orange-thick {
  display: inline;
  background: linear-gradient(transparent 50%, var(--aws-orange) 50%);
  padding: 0 4px;
}

.underline-orange-thin {
  display: inline;
  border-bottom: 3px solid var(--aws-orange);
  padding-bottom: 2px;
}

/* 注意ボックス */
.note-box {
  background: var(--aws-teal-tint);
  border-left: 4px solid var(--aws-teal);
  padding: 16px 20px;
  border-radius: 0 8px 8px 0;
  font-size: 20px;
}

.warning-box {
  background: var(--aws-orange-tint);
  border-left: 4px solid var(--aws-orange);
  padding: 16px 20px;
  border-radius: 0 8px 8px 0;
  font-size: 20px;
}

/* テーブル */
table {
  width: 100%;
  border-collapse: collapse;
  font-size: 20px;
}

th {
  background: var(--aws-squid-ink);
  color: var(--aws-white);
  padding: 14px 18px;
  text-align: left;
  font-weight: 600;
}

td {
  padding: 14px 18px;
  border-bottom: 1px solid var(--aws-border-gray);
}

tr:nth-child(even) {
  background: var(--aws-light-gray);
}

/* スライドフッター */
.slide-footer {
  position: absolute;
  bottom: 20px;
  left: 56px;
  right: 56px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 7px;
  color: var(--aws-medium-gray);
}

.slide-number {
  font-size: 9px;
  font-weight: 400;
  color: var(--aws-squid-ink);
}

/* オレンジアンダーライン装飾 */
.underline-orange {
  display: inline;
  background: linear-gradient(transparent 60%, var(--aws-orange) 60%);
  padding: 0 4px;
}

.underline-orange-thick {
  display: inline;
  background: linear-gradient(transparent 50%, var(--aws-orange) 50%);
  padding: 0 4px;
}

.underline-orange-thin {
  display: inline;
  border-bottom: 3px solid var(--aws-orange);
  padding-bottom: 2px;
}

/* 出典スライド */
.slide.sources-slide h2 {
  font-size: 32px;
  font-weight: 600;
  color: var(--aws-squid-ink);
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  gap: 12px;
}

.sources-list {
  list-style: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 8px;
  max-height: 380px;
  overflow: hidden;
}

.sources-list li {
  font-size: 14px;
  line-height: 1.4;
}

.sources-list a {
  color: var(--aws-teal);
  text-decoration: none;
}

.sources-list a:hover {
  text-decoration: underline;
}

/* 印刷用スタイル */
@media print {
  body {
    background: white;
  }
  
  .slide-container {
    padding: 0;
    gap: 0;
  }
  
  .slide {
    width: 100%;
    height: 100vh;
    border-radius: 0;
    box-shadow: none;
    page-break-after: always;
    page-break-inside: avoid;
  }
  
  .slide:last-child {
    page-break-after: auto;
  }
}

@page {
  size: 25.4cm 14.29cm;
  margin: 0;
}
```

## HTML テンプレート例

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AWS プレゼンテーション</title>
  <style>
    /* CSS テンプレートをここに挿入 */
  </style>
</head>
<body>
  <div class="slide-container">
    
    <!-- タイトルスライド -->
    <div class="slide title-slide">
      <h1>☁️ AWS アーキテクチャ概要</h1>
      <p class="subtitle">クラウドネイティブアプリケーションの設計パターン</p>
      <p class="meta">2026-01-10 | 発表者名</p>
    </div>
    
    <!-- アジェンダスライド -->
    <div class="slide agenda-slide">
      <h2>📋 アジェンダ</h2>
      <ol class="agenda-list">
        <li><span class="number">1</span>はじめに</li>
        <li><span class="number">2</span>アーキテクチャ概要</li>
        <li><span class="number">3</span>主要コンポーネント</li>
        <li><span class="number">4</span>ベストプラクティス</li>
        <li><span class="number">5</span>まとめ</li>
      </ol>
      <div class="slide-footer">
        <span>© 2026, Amazon Web Services, Inc. or its affiliates. All rights reserved.</span>
        <span class="slide-number">2</span>
      </div>
    </div>
    
    <!-- セクションスライド -->
    <div class="slide section-slide">
      <h2><span class="section-number">01</span> はじめに</h2>
      <div class="slide-footer" style="color: rgba(255,255,255,0.6);">
        <span>© 2026, Amazon Web Services, Inc. or its affiliates. All rights reserved.</span>
        <span class="slide-number" style="color: rgba(255,255,255,0.8);">3</span>
      </div>
    </div>
    
    <!-- コンテンツスライド 1 -->
    <div class="slide content-slide">
      <h2>🚀 はじめに</h2>
      <div class="slide-content">
        <p>本プレゼンテーションでは、AWS を活用したクラウドネイティブアプリケーションの設計パターンについて解説します。</p>
        
        <div class="two-column">
          <div class="card">
            <div class="card-title">🎯 目的</div>
            <div class="card-content">スケーラブルで信頼性の高いアーキテクチャの構築</div>
          </div>
          <div class="card">
            <div class="card-title">👥 対象者</div>
            <div class="card-content">クラウドアーキテクト、開発者、インフラエンジニア</div>
          </div>
        </div>
      </div>
      <div class="slide-footer">
        <span>© 2026, Amazon Web Services, Inc. or its affiliates. All rights reserved.</span>
        <span class="slide-number">3</span>
      </div>
    </div>
    
    <!-- コンテンツスライド 2 -->
    <div class="slide content-slide">
      <h2>⚙️ アーキテクチャ概要</h2>
      <div class="slide-content">
        <div class="flow-diagram">
          <div class="flow-step">👤 ユーザー</div>
          <span class="flow-arrow">→</span>
          <div class="flow-step">🌐 CloudFront</div>
          <span class="flow-arrow">→</span>
          <div class="flow-step">⚡ Lambda</div>
          <span class="flow-arrow">→</span>
          <div class="flow-step">🗄️ DynamoDB</div>
        </div>
        
        <div class="note-box">
          <strong>💡 ポイント:</strong> サーバーレスアーキテクチャにより、運用負荷を最小化しながらスケーラビリティを確保します。
        </div>
      </div>
      <div class="slide-footer">
        <span>© 2026, Amazon Web Services, Inc. or its affiliates. All rights reserved.</span>
        <span class="slide-number">4</span>
      </div>
    </div>
    
    <!-- コンテンツスライド 3 -->
    <div class="slide content-slide">
      <h2>🔧 主要コンポーネント</h2>
      <div class="slide-content">
        <div class="three-column">
          <div class="card">
            <div class="card-title">⚡ Lambda</div>
            <div class="card-content">サーバーレスコンピューティング</div>
          </div>
          <div class="card">
            <div class="card-title">🗄️ DynamoDB</div>
            <div class="card-content">NoSQL データベース</div>
          </div>
          <div class="card">
            <div class="card-title">📦 S3</div>
            <div class="card-content">オブジェクトストレージ</div>
          </div>
        </div>
        
        <ul class="content-list">
          <li><span class="icon">✓</span>自動スケーリング対応</li>
          <li><span class="icon">✓</span>高可用性設計</li>
          <li><span class="icon">✓</span>従量課金モデル</li>
        </ul>
      </div>
      <div class="slide-footer">
        <span>© 2026, Amazon Web Services, Inc. or its affiliates. All rights reserved.</span>
        <span class="slide-number">5</span>
      </div>
    </div>
    
    <!-- まとめスライド -->
    <div class="slide content-slide">
      <h2>📝 まとめ</h2>
      <div class="slide-content">
        <ul class="content-list">
          <li><span class="icon">🎯</span><span class="underline-orange">サーバーレスアーキテクチャ</span>で運用負荷を削減</li>
          <li><span class="icon">🎯</span><span class="underline-orange">マネージドサービス</span>を活用して開発に集中</li>
          <li><span class="icon">🎯</span><span class="underline-orange">Well-Architected Framework</span>に準拠した設計</li>
        </ul>
        
        <div class="warning-box">
          <strong>📎 参考資料:</strong> AWS Well-Architected Framework ドキュメント
        </div>
      </div>
      <div class="slide-footer">
        <span>© 2026, Amazon Web Services, Inc. or its affiliates. All rights reserved.</span>
        <span class="slide-number">6</span>
      </div>
    </div>
    
    <!-- 出典スライド -->
    <div class="slide sources-slide">
      <h2>📎 出典</h2>
      <ul class="sources-list">
        <li><a href="https://aws.amazon.com/architecture/well-architected/" target="_blank">AWS Well-Architected Framework</a></li>
        <li><a href="https://docs.aws.amazon.com/lambda/latest/dg/welcome.html" target="_blank">AWS Lambda Developer Guide</a></li>
        <li><a href="https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/" target="_blank">Amazon DynamoDB Developer Guide</a></li>
      </ul>
      <div class="slide-footer">
        <span>© 2026, Amazon Web Services, Inc. or its affiliates. All rights reserved.</span>
        <span class="slide-number">7</span>
      </div>
    </div>
    
  </div>
</body>
</html>
```


## スライド作成ガイドライン

### セパレータースライドのフッター色

セパレータースライドの背景色に応じて、フッターとページ番号の色を調整すること。背景とのコントラストを確保し、視認性を保つ。

### リスト項目の構造化パターン

タイトルと説明を持つリスト項目は、縦方向レイアウトで表現します。

```html
<ul class="content-list">
  <li style="flex-direction: column; align-items: flex-start; gap: 4px;">
    <div style="display: flex; align-items: center; gap: 8px;">
      <span class="icon">✓</span>
      <strong>タイトル</strong>
    </div>
    <div style="margin-left: 28px; font-size: 16px; color: var(--aws-dark-gray);">
      説明テキスト
    </div>
  </li>
</ul>
```

インデント幅 (`margin-left`) はアイコン幅 + gap と揃えます。アイコンサイズが 20px、gap が 8px の場合は 28px が目安ですが、フォントサイズやアイコンサイズに応じて調整してください。

### フロー図のテキスト折り返し

フロー図のボックス内テキストが不自然な位置で折り返されないよう、以下のルールに従ってください。

- `.flow-step` に `max-width` は設定しない
- 長いテキストは `<br>` で明示的に改行位置を指定する
- 各行が意味のある単位で収まるようにする

```html
<!-- 良い例: 明示的な改行 -->
<div class="flow-step">要件定義<br>フェーズ</div>

<!-- 避けるべき例: 自動折り返しに依存 -->
<div class="flow-step">要件定義フェーズ</div>
```

### ⚠️ 余白の活用と空白防止 (重要・必須)

スライド内に過度な空白が生じないよう、**作成前に**以下を計画すること。

**情報密度の目安**: スライド面積の **70〜85%** をコンテンツで埋める。

#### 作成前の計画ステップ (必須)

スライドを作成する前に、以下の順序で計画を立てること。

1. **コンテンツ量を確認**: 配置する情報の量を把握
2. **レイアウトを選択**: 情報量に応じて 2 カラム or 3 カラムを決定
3. **フォントサイズを固定**: 最小 15px を維持することを前提に設計
4. **項目数を決定**: フォントサイズを維持できる項目数を計算
5. **余白対策を計画**: ノートボックスや説明文の追加を事前に決定

#### フォントサイズの厳守 (最重要)

**カード内のフォントサイズを 18px 未満にすることは絶対に禁止。**

余白を埋めるために項目数を増やす際、フォントサイズを縮小してはならない。

| 要素 | 最小サイズ | 推奨サイズ |
|------|-----------|-----------|
| カードタイトル (.card-title) | 20px | 24px |
| カードコンテンツ (.card-content) | 18px | 20px |
| カード内リスト項目 | 18px | 20px |
| ノートボックス (.note-box) | 18px | 20px |

#### 3 カラムレイアウトでの注意

3 カラムレイアウトでは横幅が狭くなるため、特に注意が必要。

- 各カードの項目数は **2-3 個まで**
- テキストは短く簡潔に
- フォントサイズは **18px 以上**を維持
- 収まらない場合は 2 カラムに変更

#### 余白を埋める際の正しいアプローチ

**❌ 間違ったアプローチ (禁止)**:
```html
<!-- フォントを 12px に縮小して項目を詰め込む -->
<div class="card-content" style="font-size: 12px; line-height: 1.4;">
  <ul style="padding-left: 14px;">
    <li>項目 1</li>
    <li>項目 2</li>
    <li>項目 3</li>
    <li>項目 4</li>
    <li>項目 5</li>
  </ul>
</div>
```

**✅ 正しいアプローチ**:
```html
<!-- フォントサイズを維持し、項目数を 3-4 に制限 -->
<div class="card-content" style="font-size: 15px; line-height: 1.5;">
  概要説明文をここに記載
  <ul style="margin-top: 8px; padding-left: 16px;">
    <li>項目 1</li>
    <li>項目 2</li>
    <li>項目 3</li>
  </ul>
</div>
```

#### 空白が生じた場合の対処法 (優先順位順)

| 優先度 | 状況 | 対応方法 |
|--------|------|----------|
| 1 | カード内が薄い | 概要文 + 箇条書き (3〜4 項目) の構造に拡充 |
| 2 | 下部に空白 | `.note-box` / `.warning-box` でサマリーや補足を追加 |
| 3 | カード 2 枚で余白大 | 3 カラムに変更、または各カードに詳細リストを追加 |
| 4 | リスト項目が少ない | 各項目に説明文を追加 |

**禁止事項**: 余白を埋めるために無関係な情報を追加しない。フォントサイズを縮小しない。

### カード内の情報構造

カードに複数の情報を含める場合は、以下の構造を推奨する。

1. タイトル (アイコン + 名称)
2. 概要文 (1〜2 行の説明)
3. 詳細リスト (箇条書きで 3〜4 項目)

情報量やコンテンツの性質に応じて、概要文のみ、または箇条書きのみでも可。

### 1 スライド 1 メッセージの原則

各スライドは 1 つの主要なポイントに集中させます。

- タイトルでそのスライドの要点を明確に示す
- 本文は 3〜5 項目程度に抑える
- 詳細は口頭で補足し、スライドはシンプルに保つ

### 箇条書きのルール

- 1 行は 40 文字以内を目安
- 各項目は動詞または名詞で始める (並列構造)
- 階層は 2 レベルまでに抑える
- チェックマーク (✓) または矢印 (→) をアイコンとして使用

### フォントサイズの目安

| 要素 | 最小サイズ | 推奨サイズ |
|------|-----------|-----------|
| タイトル | 36px | 40-56px |
| 本文 | 22px | 26px |
| 補足テキスト | 20px | 22px |
| カードタイトル | 20px | 24px |
| カードコンテンツ | 18px | 20px |
| フロー図ステップ | 16px | 18px |
| ノートボックス | 18px | 20px |
| テーブル | 18px | 20px |
| フッター | 7px | 7px |

### 色の使い方

- **オレンジ (#FF9900)**: 強調、アクセント、重要なポイント
- **Squid Ink (#232F3E)**: タイトル、見出し
- **ダークグレー (#545B64)**: 本文テキスト
- **ライトグレー (#F2F3F3)**: カード背景、区切り

## PDF 印刷手順

1. 生成された HTML ファイルをブラウザで開く
2. `Ctrl+P` (Windows) または `Cmd+P` (Mac) で印刷ダイアログを開く
3. 「送信先」で「PDF に保存」を選択
4. 「レイアウト」で「横」を選択
5. 「余白」で「なし」を選択
6. 「背景のグラフィック」にチェックを入れる
7. 「保存」をクリック

## 適用キーワード

以下のキーワードが含まれる場合、このテーマを適用します。

- 「スライド」「プレゼン」「プレゼンテーション」「PowerPoint 風」
- 「PDF」「印刷」「複数ページ」
- 「AWS」「ライト」「白背景」
