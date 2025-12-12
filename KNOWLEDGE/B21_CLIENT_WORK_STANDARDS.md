# B21: クライアントワーク品質基準

## 概要
実務（クライアントワーク）で求められるHTML/CSS/JS構築の品質基準。
「自分以外の誰か（ユーザー、検索ロボット、将来の担当者）が触れても問題が起きない品質」を目指す。

---

## 1. 品質基準の3つの柱

```
┌─────────────────────────────────────────────┐
│        クライアントワーク品質基準             │
├─────────────────────────────────────────────┤
│  1. 保守性（Maintainability）               │
│     → 将来の修正・拡張が容易                │
├─────────────────────────────────────────────┤
│  2. セキュリティ（Security）                │
│     → 脆弱性を作り込まない実装              │
├─────────────────────────────────────────────┤
│  3. アクセシビリティ（Accessibility）       │
│     → すべてのユーザーが利用可能            │
└─────────────────────────────────────────────┘
```

---

## 2. HTML構築の品質基準

### 2.1 セマンティックマークアップ

| 項目 | 必須要件 | 根拠 |
|------|---------|------|
| ランドマーク要素 | header, nav, main, footer を適切に配置 | 支援技術のナビゲーション |
| 見出し構造 | h1-h6で論理的な階層構造 | SEO・アクセシビリティ |
| 適切なタグ選択 | div/span の乱用を避ける | セマンティクス |
| リスト構造 | ul/ol/dl を適切に使用 | 構造の明示 |

```html
<!-- ✅ クライアントワーク品質 -->
<article>
  <header>
    <h1>記事タイトル</h1>
    <time datetime="2025-01-15">2025年1月15日</time>
  </header>
  
  <section aria-labelledby="intro-heading">
    <h2 id="intro-heading">はじめに</h2>
    <p>本文テキスト...</p>
  </section>
  
  <section aria-labelledby="detail-heading">
    <h2 id="detail-heading">詳細</h2>
    <ul>
      <li>項目1</li>
      <li>項目2</li>
    </ul>
  </section>
</article>


<!-- ❌ 避けるべき実装 -->
<div class="article">
  <div class="title">記事タイトル</div>
  <div class="date">2025年1月15日</div>
  
  <div class="section">
    <div class="heading">はじめに</div>
    <div class="text">本文テキスト...</div>
  </div>
</div>
```

### 2.2 禁止事項

| 禁止事項 | 理由 | 代替手段 |
|---------|------|---------|
| brタグでの余白調整 | 視覚的調整はCSSの責務 | margin/padding |
| 見出しレベルの飛ばし | アウトライン構造の破壊 | 正しい階層順 |
| divのボタン化 | キーボード操作・支援技術の問題 | button要素 |
| 画像単体のpタグ囲み | 段落の意味がない | figure/div |
| display:noneでの画像切替 | 不要な通信が発生 | picture要素 |

---

## 3. CSS構築の品質基準

### 3.1 必須要件

```css
/* 1. CSS変数による一元管理 */
:root {
  /* カラー */
  --color-primary: #2563eb;
  --color-text: #1f2937;
  --color-background: #ffffff;
  
  /* スペーシング */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 2rem;
  --spacing-xl: 4rem;
  
  /* タイポグラフィ */
  --font-family-base: 'Noto Sans JP', sans-serif;
  --font-size-base: 1rem;
  --line-height-base: 1.8;
  
  /* トランジション */
  --transition-base: 0.3s ease;
  
  /* ブレークポイント（参照用） */
  --breakpoint-sm: 640px;
  --breakpoint-md: 768px;
  --breakpoint-lg: 1024px;
}

/* 2. リセット/ノーマライズ */
*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

/* 3. フォーカス表示の確保 */
:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}

/* 4. レスポンシブ対応 */
.container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--spacing-md);
}

@media (min-width: 768px) {
  .container {
    padding: 0 var(--spacing-lg);
  }
}
```

### 3.2 命名規則（BEM推奨）

```css
/* Block: 独立したコンポーネント */
.card { }
.button { }
.form { }

/* Element: ブロックの構成要素 */
.card__header { }
.card__title { }
.card__body { }
.card__footer { }
.button__icon { }
.form__group { }

/* Modifier: 状態・バリエーション */
.card--featured { }
.card--horizontal { }
.button--primary { }
.button--secondary { }
.button--disabled { }
.form__group--error { }
```

---

## 4. JavaScript構築の品質基準

### 4.1 必須要件

```javascript
// 1. 厳格モード
'use strict';

// 2. 定数は大文字スネークケース
const MAX_RETRY_COUNT = 3;
const API_ENDPOINT = '/api/contact';

// 3. DOM操作の安全性確認
const form = document.getElementById('contactForm');
if (!form) return; // 要素がない場合の早期リターン

// 4. イベントリスナーの適切な登録
document.addEventListener('DOMContentLoaded', () => {
  initializeForm();
});

// 5. エラーハンドリング
async function submitForm(data) {
  try {
    const response = await fetch(API_ENDPOINT, {
      method: 'POST',
      body: JSON.stringify(data),
      headers: { 'Content-Type': 'application/json' }
    });
    
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    
    return await response.json();
  } catch (error) {
    console.error('Form submission failed:', error);
    throw error; // 呼び出し元でハンドリング
  }
}

// 6. XSS対策の徹底
function escapeHTML(str) {
  const div = document.createElement('div');
  div.textContent = str;
  return div.innerHTML;
}

// ユーザー入力をDOMに反映する場合
element.textContent = userInput;  // 安全
// または
element.innerHTML = escapeHTML(userInput);  // HTMLとして必要な場合
```

### 4.2 禁止事項

```javascript
// ❌ eval()の使用禁止
eval(userInput);

// ❌ innerHTML への直接代入
element.innerHTML = userInput;

// ❌ document.write()の使用禁止
document.write('<script>...</script>');

// ❌ インラインイベントハンドラ
// <button onclick="handleClick()">  HTMLで記述しない

// ❌ グローバル変数の汚染
window.myData = data;  // グローバルスコープを汚染
```

---

## 5. パフォーマンス要件

### 5.1 画像の最適化

```html
<!-- レスポンシブ画像 -->
<picture>
  <source 
    media="(min-width: 1024px)" 
    srcset="hero-large.webp"
    type="image/webp"
  >
  <source 
    media="(min-width: 768px)" 
    srcset="hero-medium.webp"
    type="image/webp"
  >
  <img 
    src="hero-small.jpg" 
    alt="ヒーロー画像の説明"
    loading="lazy"
    decoding="async"
    width="800"
    height="600"
  >
</picture>

<!-- 遅延読み込み -->
<img 
  src="product.jpg" 
  alt="商品画像"
  loading="lazy"
  width="400"
  height="300"
>
```

### 5.2 リソース読み込みの最適化

```html
<head>
  <!-- クリティカルCSSはインライン -->
  <style>
    /* ファーストビューに必要な最小限のCSS */
  </style>
  
  <!-- 外部CSSは非同期で -->
  <link rel="preload" href="styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
  
  <!-- フォントのプリロード -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  
  <!-- スクリプトは defer で -->
  <script src="main.js" defer></script>
</head>
```

---

## 6. 納品前チェックリスト

### 構造・セマンティクス
- [ ] ランドマーク要素（header, nav, main, footer）を適切に配置
- [ ] 見出しh1-h6が論理的な階層構造
- [ ] divの乱用なし、適切なタグを選択
- [ ] brタグをレイアウト目的で使用していない

### アクセシビリティ
- [ ] すべてのインタラクティブ要素がキーボードでアクセス可能
- [ ] フォーカス表示が視認可能
- [ ] 画像にalt属性あり（装飾画像はalt=""）
- [ ] フォームにラベルが関連付け
- [ ] ARIA使用はルール1を遵守

### セキュリティ
- [ ] CSPヘッダー設定
- [ ] XSSエスケープ処理の実装
- [ ] フォームにHoneypot実装
- [ ] 入力値のバリデーション

### 保守性
- [ ] CSS変数で値を一元管理
- [ ] 一貫した命名規則（BEM等）
- [ ] 「なぜ」を説明するコメント
- [ ] 重複コードなし（DRY原則）

### パフォーマンス
- [ ] 画像にwidth/height属性
- [ ] 遅延読み込み（loading="lazy"）
- [ ] レスポンシブ画像（picture要素）
- [ ] 不要なリソースの読み込みなし

### レスポンシブ
- [ ] モバイル表示の確認
- [ ] タブレット表示の確認
- [ ] デスクトップ表示の確認
- [ ] 横向き表示の確認

### クロスブラウザ
- [ ] Chrome
- [ ] Firefox
- [ ] Safari
- [ ] Edge
- [ ] iOS Safari
- [ ] Android Chrome

---

## 7. AI生成コードのレビューポイント

AI（Claude, GPT等）にコードを生成させた場合、以下を必ず人間が確認：

### セキュリティ
- [ ] eval()やinnerHTMLへの直接代入がないか
- [ ] XSSエスケープが適切か
- [ ] CSPヘッダーが含まれているか

### アクセシビリティ
- [ ] セマンティックHTMLを使用しているか
- [ ] ARIA使用がルール1に違反していないか
- [ ] キーボード操作が考慮されているか

### 保守性
- [ ] 値がハードコードされていないか
- [ ] 重複コードがないか
- [ ] 命名が適切か

### 実務適合性
- [ ] 実際のブラウザで動作確認
- [ ] レスポンシブ動作確認
- [ ] エッジケース（空データ、大量データ等）の確認
