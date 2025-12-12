# B16: Webセキュリティ基礎

## 概要
静的Webサイトに必須のセキュリティ対策。OWASP Top 10 2025準拠。

---

## 1. 必須セキュリティヘッダー

### 1.1 CSP（Content-Security-Policy）

**基本テンプレート（静的サイト）**
```html
<meta http-equiv="Content-Security-Policy" content="
  default-src 'self';
  script-src 'self';
  style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
  img-src 'self' data: https:;
  font-src 'self' https://fonts.gstatic.com;
  frame-ancestors 'self';
  base-uri 'self';
  form-action 'self';
  upgrade-insecure-requests;
">
```

**Google Analytics対応**
```html
<meta http-equiv="Content-Security-Policy" content="
  default-src 'self';
  script-src 'self' https://www.googletagmanager.com https://www.google-analytics.com;
  style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
  img-src 'self' data: https: www.google-analytics.com;
  font-src 'self' https://fonts.gstatic.com;
  connect-src 'self' https://www.google-analytics.com https://analytics.google.com;
  frame-ancestors 'self';
  base-uri 'self';
  form-action 'self';
  upgrade-insecure-requests;
">
```

**YouTube埋め込み対応**
```html
frame-src https://www.youtube-nocookie.com https://www.youtube.com;
img-src 'self' data: https://i.ytimg.com https://img.youtube.com;
```

**Googleマップ埋め込み対応**
```html
frame-src https://www.google.com/maps https://maps.google.com;
script-src 'self' https://maps.googleapis.com;
```

**reCAPTCHA v3対応**
```html
script-src 'self' https://www.google.com https://www.gstatic.com;
frame-src https://www.google.com;
```

### 1.2 その他のセキュリティヘッダー

```html
<!-- リファラー制御 -->
<meta name="referrer" content="strict-origin-when-cross-origin">

<!-- 混合コンテンツブロック -->
<meta http-equiv="Content-Security-Policy" content="block-all-mixed-content;">
```

**サーバー設定（.htaccess）推奨**
```apache
Header set X-Content-Type-Options "nosniff"
Header set X-Frame-Options "SAMEORIGIN"
Header set Permissions-Policy "geolocation=(), microphone=(), camera=()"
Header set Strict-Transport-Security "max-age=31536000; includeSubDomains"
```

---

## 2. XSS対策

### 2.1 HTMLエスケープ関数

```javascript
function escapeHTML(str) {
  if (typeof str !== 'string') return '';
  const div = document.createElement('div');
  div.textContent = str;
  return div.innerHTML;
}

// 使用例
element.textContent = userInput;  // 安全
element.innerHTML = escapeHTML(userInput);  // HTMLとして挿入する場合
```

### 2.2 URLパラメータの安全な取得

```javascript
function getSafeParam(name) {
  const params = new URLSearchParams(window.location.search);
  const value = params.get(name);
  if (value === null) return '';
  return value
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;')
    .replace(/'/g, '&#039;');
}
```

### 2.3 危険パターン検出

```javascript
function containsDangerousContent(str) {
  const patterns = [
    /<script/i,
    /javascript:/i,
    /on\w+\s*=/i,
    /data:/i,
    /vbscript:/i
  ];
  return patterns.some(pattern => pattern.test(str));
}
```

---

## 3. クリックジャッキング対策

```html
<!-- CSPで防御（推奨） -->
<meta http-equiv="Content-Security-Policy" content="frame-ancestors 'self';">

<!-- JavaScript補助 -->
<script>
if (window.top !== window.self) {
  document.body.innerHTML = '<p>このページは直接アクセスしてください。</p>';
}
</script>
```

---

## 4. 外部リソースのセキュリティ

### 4.1 SRI（Subresource Integrity）

```html
<script 
  src="https://cdn.example.com/library.min.js"
  integrity="sha384-xxxxxxxxxxxxxxxxxxxxx"
  crossorigin="anonymous">
</script>
```

### 4.2 CDN使用時の注意

- 可能な限りセルフホストを推奨
- CDN使用時は必ずSRIを設定
- フォールバックを用意

---

## 5. HTTPS関連

### 5.1 混合コンテンツ防止

```html
<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests;">
```

### 5.2 リンクの絶対指定

```html
<!-- 推奨 -->
<a href="https://example.com">リンク</a>

<!-- 非推奨（HTTPになる可能性） -->
<a href="//example.com">リンク</a>
```

---

## 6. セキュリティチェックリスト

### 必須（すべてのサイト）
- [ ] CSPヘッダー設定
- [ ] frame-ancestors 'self'
- [ ] base-uri 'self'
- [ ] referrer-policy設定
- [ ] HTTPS前提の設計

### フォームがある場合
- [ ] form-action 'self'
- [ ] 入力値検証
- [ ] Honeypot実装
- [ ] maxlength設定

### 外部サービス利用時
- [ ] 必要なドメインのみCSPに追加
- [ ] SRI設定（可能な場合）

---

## 7. テストツール

### オンラインツール
- CSP Evaluator: https://csp-evaluator.withgoogle.com/
- Security Headers: https://securityheaders.com/
- Mozilla Observatory: https://observatory.mozilla.org/

### ブラウザDevTools
1. F12でDevTools起動
2. Consoleタブでエラー確認
3. Networkタブでリソース読み込み確認
