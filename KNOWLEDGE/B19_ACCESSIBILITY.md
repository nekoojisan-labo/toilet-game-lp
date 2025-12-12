# B19: アクセシビリティ基礎

## 概要
支援技術（スクリーンリーダー等）がコンテンツを正確に解釈できるようにするための実装ガイド。
WCAG 2.1準拠を目標とする。

---

## 1. セマンティックHTMLの徹底

### 1.1 ランドマーク要素の適切な配置

```html
<!DOCTYPE html>
<html lang="ja">
<head>...</head>
<body>
  <!-- ヘッダー：サイト全体のヘッダー -->
  <header class="site-header">
    <nav aria-label="メインナビゲーション">
      <!-- ナビゲーション -->
    </nav>
  </header>
  
  <!-- メインコンテンツ：ページ固有の主要コンテンツ -->
  <main id="main-content">
    <article>
      <h1>ページタイトル</h1>
      <section aria-labelledby="section1-heading">
        <h2 id="section1-heading">セクション見出し</h2>
        <!-- セクション内容 -->
      </section>
    </article>
    
    <!-- サイドバー（補足情報） -->
    <aside aria-label="関連情報">
      <!-- 補足コンテンツ -->
    </aside>
  </main>
  
  <!-- フッター：サイト全体のフッター -->
  <footer class="site-footer">
    <!-- フッター内容 -->
  </footer>
</body>
</html>
```

### 1.2 見出し構造の遵守

```html
<!-- ✅ 正しい見出し構造 -->
<h1>サイトタイトル</h1>
  <h2>セクション1</h2>
    <h3>サブセクション1-1</h3>
    <h3>サブセクション1-2</h3>
  <h2>セクション2</h2>
    <h3>サブセクション2-1</h3>

<!-- ❌ 避けるべき実装 -->
<!-- 見出しレベルの飛ばし -->
<h1>タイトル</h1>
<h3>サブセクション</h3>  <!-- h2を飛ばしている -->

<!-- 見た目調整のための見出し使用 -->
<h4>小さく見せたいだけのテキスト</h4>
```

### 1.3 適切なタグ選択

| 用途 | ✅ 正しい | ❌ 避ける |
|------|----------|----------|
| ボタン | `<button>` | `<div onclick>` |
| リンク | `<a href>` | `<span onclick>` |
| リスト | `<ul><li>` | `<div>・項目</div>` |
| 表 | `<table>` | `<div>`でグリッド |
| 強調 | `<strong>`, `<em>` | `<span style="font-weight:bold">` |
| 段落 | `<p>` | `<div>` |

---

## 2. WAI-ARIAの厳格な適用

### 2.1 ルール1（最重要）

> **HTML標準要素で機能が実現できる場合はARIAを使用しない**

```html
<!-- ✅ 正しい：標準要素を使用 -->
<button type="button">クリック</button>
<a href="/page">リンク</a>
<input type="checkbox">

<!-- ❌ 避ける：不要なARIA -->
<div role="button" tabindex="0">クリック</div>
<span role="link" tabindex="0">リンク</span>
<div role="checkbox" aria-checked="false">
```

### 2.2 ARIAが必要なケース

```html
<!-- タブパネル（HTML標準にない） -->
<div role="tablist" aria-label="コンテンツタブ">
  <button role="tab" aria-selected="true" aria-controls="panel1" id="tab1">
    タブ1
  </button>
  <button role="tab" aria-selected="false" aria-controls="panel2" id="tab2">
    タブ2
  </button>
</div>
<div role="tabpanel" id="panel1" aria-labelledby="tab1">
  パネル1の内容
</div>
<div role="tabpanel" id="panel2" aria-labelledby="tab2" hidden>
  パネル2の内容
</div>

<!-- トグルボタン（状態管理） -->
<button aria-pressed="false" onclick="toggleMenu()">
  メニュー
</button>

<!-- ライブリージョン（動的更新の通知） -->
<div aria-live="polite" aria-atomic="true">
  <!-- 動的に更新される内容 -->
</div>
```

### 2.3 主要なARIA属性

| 属性 | 用途 | 例 |
|------|------|-----|
| `aria-label` | 要素にラベルを付与 | `<nav aria-label="メイン">` |
| `aria-labelledby` | 他要素をラベルとして参照 | `<section aria-labelledby="heading1">` |
| `aria-describedby` | 説明テキストを参照 | `<input aria-describedby="hint1">` |
| `aria-hidden` | 支援技術から隠す | `<span aria-hidden="true">装飾</span>` |
| `aria-expanded` | 展開状態 | `<button aria-expanded="false">` |
| `aria-current` | 現在の項目 | `<a aria-current="page">現在ページ</a>` |

---

## 3. キーボードナビゲーション

### 3.1 基本要件

```html
<!-- すべてのインタラクティブ要素はキーボードでアクセス可能に -->

<!-- フォーカス可能な要素 -->
<a href="#">リンク</a>
<button>ボタン</button>
<input type="text">
<select>...</select>
<textarea>...</textarea>

<!-- カスタム要素をフォーカス可能にする場合 -->
<div role="button" tabindex="0" onkeydown="handleKeydown(event)">
  カスタムボタン
</div>
```

### 3.2 フォーカス管理

```javascript
// モーダル開閉時のフォーカス管理
function openModal() {
  const modal = document.getElementById('modal');
  const firstFocusable = modal.querySelector('button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])');
  
  modal.hidden = false;
  modal.setAttribute('aria-hidden', 'false');
  
  // フォーカスをモーダル内に移動
  if (firstFocusable) {
    firstFocusable.focus();
  }
  
  // フォーカストラップ（モーダル外にフォーカスが出ないように）
  modal.addEventListener('keydown', trapFocus);
}

function closeModal() {
  const modal = document.getElementById('modal');
  const trigger = document.getElementById('modalTrigger');
  
  modal.hidden = true;
  modal.setAttribute('aria-hidden', 'true');
  
  // フォーカスをトリガー要素に戻す
  trigger.focus();
}
```

### 3.3 スキップリンク

```html
<!-- ページ上部に配置 -->
<a href="#main-content" class="skip-link">
  メインコンテンツへスキップ
</a>

<style>
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #000;
  color: #fff;
  padding: 8px;
  z-index: 100;
  transition: top 0.3s;
}

.skip-link:focus {
  top: 0;
}
</style>
```

---

## 4. 画像と代替テキスト

### 4.1 alt属性の設計原則

| 画像の種類 | alt属性の内容 | 例 |
|-----------|--------------|-----|
| 情報を伝える画像 | 画像の内容を説明 | `alt="2024年売上グラフ：前年比120%"` |
| 装飾目的の画像 | 空のalt | `alt=""` |
| リンク内の画像 | リンク先を説明 | `alt="ホームへ戻る"` |
| 複雑な図表 | 簡潔な説明 + 詳細テキスト | `alt="組織図" aria-describedby="org-desc"` |

### 4.2 実装例

```html
<!-- 情報を伝える画像 -->
<img src="chart.png" alt="2024年第3四半期の売上推移。7月100万円、8月120万円、9月150万円と右肩上がり">

<!-- 装飾目的の画像 -->
<img src="decoration.png" alt="">

<!-- リンク内の画像 -->
<a href="/">
  <img src="logo.png" alt="サイト名 - ホームへ">
</a>

<!-- テキストと画像の組み合わせ -->
<a href="/products">
  <img src="product.jpg" alt="">  <!-- テキストがあるので空 -->
  <span>商品一覧を見る</span>
</a>

<!-- 複雑な図表 -->
<figure>
  <img src="org-chart.png" alt="組織図" aria-describedby="org-description">
  <figcaption id="org-description">
    代表取締役の下に営業部、開発部、管理部の3部門があり、
    各部門には2〜3名のチームリーダーが配置されています。
  </figcaption>
</figure>
```

---

## 5. フォームのアクセシビリティ

```html
<form>
  <!-- ラベルと入力の関連付け -->
  <div class="form-group">
    <label for="name">お名前 <span aria-hidden="true">*</span></label>
    <input 
      type="text" 
      id="name" 
      name="name" 
      required
      aria-required="true"
      aria-describedby="name-hint"
    >
    <span id="name-hint" class="hint">姓と名の間にスペースを入れてください</span>
  </div>
  
  <!-- エラーメッセージ -->
  <div class="form-group">
    <label for="email">メールアドレス</label>
    <input 
      type="email" 
      id="email" 
      name="email"
      aria-invalid="true"
      aria-describedby="email-error"
    >
    <span id="email-error" class="error" role="alert">
      正しいメールアドレスを入力してください
    </span>
  </div>
  
  <!-- ラジオボタングループ -->
  <fieldset>
    <legend>お問い合わせ種別</legend>
    <label>
      <input type="radio" name="type" value="general"> 一般的なお問い合わせ
    </label>
    <label>
      <input type="radio" name="type" value="support"> サポート
    </label>
  </fieldset>
  
  <button type="submit">送信</button>
</form>
```

---

## 6. チェックリスト

### 構造
- [ ] 適切なランドマーク要素（header, nav, main, footer）を使用
- [ ] 見出しレベルが論理的な階層構造
- [ ] セマンティックなHTMLタグを選択

### ARIA
- [ ] HTML標準要素で実現可能な場合はARIA未使用
- [ ] カスタムコンポーネントには適切なrole属性
- [ ] 状態変化はaria属性で通知

### キーボード
- [ ] すべてのインタラクティブ要素がキーボードでアクセス可能
- [ ] フォーカス順序が論理的
- [ ] モーダル等でフォーカストラップを実装
- [ ] スキップリンクを提供

### 画像・メディア
- [ ] 情報画像に適切なalt属性
- [ ] 装飾画像にalt=""
- [ ] 動画に字幕またはトランスクリプト

### フォーム
- [ ] すべての入力にラベルを関連付け
- [ ] エラーメッセージが支援技術に通知される
- [ ] 必須項目が明示されている

---

## 7. テストツール

- **axe DevTools**: Chrome拡張機能
- **WAVE**: Webアクセシビリティ評価ツール
- **Lighthouse**: Chromeの監査ツール
- **NVDA/VoiceOver**: スクリーンリーダーでの実機テスト
