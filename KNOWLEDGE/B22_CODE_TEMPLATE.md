# B22: コードテンプレート

## 概要
GPT-Cが使用するHTML/CSS/JSの基本テンプレート。

---

## 1. HTML基本構造

```html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{meta.title}</title>
    
    <!-- ═══════════════════════════════════════════════════════ -->
    <!-- セキュリティヘッダー（必須） -->
    <!-- ═══════════════════════════════════════════════════════ -->
    <meta http-equiv="Content-Security-Policy" content="
        default-src 'self';
        script-src 'self' {external_scripts};
        style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
        img-src 'self' data: https:;
        font-src 'self' https://fonts.gstatic.com;
        frame-src {allowed_frames};
        frame-ancestors 'self';
        base-uri 'self';
        form-action 'self';
        upgrade-insecure-requests;
    ">
    <meta name="referrer" content="strict-origin-when-cross-origin">
    
    <!-- SEO・OGP -->
    <meta name="description" content="{meta.description}">
    <meta property="og:title" content="{meta.title}">
    <meta property="og:description" content="{meta.description}">
    <meta property="og:type" content="website">
    
    <!-- フォント -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family={font}&display=swap" rel="stylesheet">
    
    <style>
        /* CSS変数・リセット・コンポーネント */
    </style>
</head>
<body>
    <!-- スキップリンク -->
    <a href="#main-content" class="skip-link">メインコンテンツへスキップ</a>
    
    <header class="header">...</header>
    <main id="main-content">...</main>
    <footer class="footer">...</footer>
    
    <script>
        'use strict';
        /* JavaScript */
    </script>
</body>
</html>
```

---

## 2. CSS変数テンプレート

```css
:root {
    /* カラー（設計書のcolor_systemから） */
    --color-primary: {color_system.primary.hex};
    --color-secondary: {color_system.secondary.hex};
    --color-accent: {color_system.accent.hex};
    --color-background: {color_system.background.main};
    --color-background-alt: {color_system.background.alt};
    --color-text: {color_system.text.primary};
    --color-text-secondary: {color_system.text.secondary};
    
    /* タイポグラフィ（設計書のtypographyから） */
    --font-heading: {typography.font_family.heading};
    --font-body: {typography.font_family.body};
    --font-size-base: {typography.scale.base};
    --line-height-body: {typography.line_height.body};
    
    /* スペーシング */
    --spacing-xs: 0.25rem;
    --spacing-sm: 0.5rem;
    --spacing-md: 1rem;
    --spacing-lg: 2rem;
    --spacing-xl: 4rem;
    --spacing-section: {layout.spacing.section};
    
    /* レイアウト */
    --max-width: {layout.max_width};
    --border-radius: 8px;
    --transition: 0.3s ease;
}
```

---

## 3. CSSリセット

```css
*, *::before, *::after {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

html {
    scroll-behavior: smooth;
    font-size: var(--font-size-base);
}

body {
    font-family: var(--font-body);
    color: var(--color-text);
    background-color: var(--color-background);
    line-height: var(--line-height-body);
    -webkit-font-smoothing: antialiased;
}
```

---

## 4. アクセシビリティCSS

```css
/* フォーカス表示 */
:focus-visible {
    outline: 2px solid var(--color-primary);
    outline-offset: 2px;
}

/* スキップリンク */
.skip-link {
    position: absolute;
    top: -40px;
    left: 0;
    background: var(--color-primary);
    color: #fff;
    padding: var(--spacing-sm) var(--spacing-md);
    z-index: 100;
    transition: top var(--transition);
}

.skip-link:focus {
    top: 0;
}
```

---

## 5. BEM命名パターン

```css
/* Header */
.header { }
.header__container { }
.header__logo { }
.header__nav { }
.header__nav-list { }
.header__nav-item { }
.header__nav-link { }
.header--scrolled { }

/* Hero */
.hero { }
.hero__container { }
.hero__content { }
.hero__title { }
.hero__subtitle { }
.hero__cta { }

/* Section共通 */
.section { }
.section__container { }
.section__title { }
.section__content { }

/* Card */
.card { }
.card__image { }
.card__content { }
.card__title { }
.card__text { }

/* Button */
.btn { }
.btn--primary { }
.btn--secondary { }
.btn--large { }

/* Form */
.form { }
.form__group { }
.form__label { }
.form__input { }
.form__input--error { }
.form__textarea { }
.form__error { }
.form__submit { }
```

---

## 6. フォームHTML（セキュリティ対応）

```html
<form id="contactForm" class="form" action="#" method="POST" novalidate>
    <!-- Honeypot（ボット対策） -->
    <div style="position: absolute; left: -9999px;" aria-hidden="true">
        <input type="text" name="website" tabindex="-1" autocomplete="off">
    </div>
    
    <div class="form__group">
        <label for="name" class="form__label">
            お名前 <span class="form__required" aria-hidden="true">*</span>
        </label>
        <input 
            type="text" 
            id="name" 
            name="name" 
            class="form__input"
            required 
            aria-required="true"
            maxlength="100"
            autocomplete="name"
            aria-describedby="name-error"
        >
        <span id="name-error" class="form__error" role="alert"></span>
    </div>
    
    <div class="form__group">
        <label for="email" class="form__label">
            メールアドレス <span class="form__required" aria-hidden="true">*</span>
        </label>
        <input 
            type="email" 
            id="email" 
            name="email" 
            class="form__input"
            required 
            aria-required="true"
            maxlength="254"
            autocomplete="email"
            aria-describedby="email-error"
        >
        <span id="email-error" class="form__error" role="alert"></span>
    </div>
    
    <div class="form__group">
        <label for="message" class="form__label">
            お問い合わせ内容 <span class="form__required" aria-hidden="true">*</span>
        </label>
        <textarea 
            id="message" 
            name="message" 
            class="form__textarea"
            required 
            aria-required="true"
            maxlength="5000"
            rows="5"
            aria-describedby="message-error"
        ></textarea>
        <span id="message-error" class="form__error" role="alert"></span>
    </div>
    
    <button type="submit" class="btn btn--primary form__submit">
        送信する
    </button>
</form>
```

---

## 7. JavaScript基本構造

```javascript
'use strict';

/* XSSエスケープ関数（必須） */
function escapeHTML(str) {
    if (typeof str !== 'string') return '';
    const div = document.createElement('div');
    div.textContent = str;
    return div.innerHTML;
}

/* バリデーション関数 */
const validators = {
    required: (value) => value.trim() !== '' || 'この項目は必須です',
    email: (value) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value) || '正しいメールアドレスを入力してください',
    maxLength: (max) => (value) => value.length <= max || `${max}文字以内で入力してください`,
    noScript: (value) => !/<script|javascript:|on\w+=/i.test(value) || '不正な文字が含まれています'
};

document.addEventListener('DOMContentLoaded', function() {
    
    /* フォームバリデーション */
    const form = document.getElementById('contactForm');
    if (form) {
        function validateField(field) {
            const value = field.value.trim();
            const errorEl = document.getElementById(field.id + '-error');
            let isValid = true;
            let message = '';
            
            if (field.required && !value) {
                isValid = false;
                message = 'この項目は必須です';
            } else if (field.type === 'email' && value) {
                const result = validators.email(value);
                if (result !== true) {
                    isValid = false;
                    message = result;
                }
            } else if (value) {
                const result = validators.noScript(value);
                if (result !== true) {
                    isValid = false;
                    message = result;
                }
            }
            
            if (errorEl) errorEl.textContent = message;
            field.classList.toggle('form__input--error', !isValid);
            field.setAttribute('aria-invalid', !isValid);
            
            return isValid;
        }
        
        form.querySelectorAll('input, textarea').forEach(field => {
            field.addEventListener('blur', () => validateField(field));
        });
        
        form.addEventListener('submit', function(e) {
            e.preventDefault();
            
            // Honeypotチェック
            const honeypot = form.querySelector('input[name="website"]');
            if (honeypot && honeypot.value) {
                console.log('Bot detected');
                return;
            }
            
            let isValid = true;
            form.querySelectorAll('input[required], textarea[required]').forEach(field => {
                if (!validateField(field)) isValid = false;
            });
            
            if (isValid) {
                alert('送信が完了しました');
                form.reset();
            }
        });
    }
    
    /* ヘッダースクロール */
    const header = document.querySelector('.header');
    if (header) {
        window.addEventListener('scroll', () => {
            header.classList.toggle('header--scrolled', window.scrollY > 100);
        });
    }
    
    /* スクロールアニメーション */
    const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                entry.target.classList.add('animate-in');
                observer.unobserve(entry.target);
            }
        });
    }, { threshold: 0.1 });
    
    document.querySelectorAll('.animate-on-scroll').forEach(el => {
        observer.observe(el);
    });
});
```

---

## 8. セクションHTML例

### Header
```html
<header class="header">
    <div class="header__container">
        <a href="/" class="header__logo">{site_name}</a>
        <nav class="header__nav" aria-label="メインナビゲーション">
            <ul class="header__nav-list">
                <li class="header__nav-item">
                    <a href="#about" class="header__nav-link">私たちについて</a>
                </li>
            </ul>
        </nav>
    </div>
</header>
```

### Hero
```html
<section class="hero" aria-labelledby="hero-heading">
    <div class="hero__container">
        <h1 id="hero-heading" class="hero__title">{headline}</h1>
        <p class="hero__subtitle">{subheadline}</p>
        <a href="#contact" class="hero__cta btn btn--primary">{cta_text}</a>
    </div>
</section>
```

### 汎用セクション
```html
<section id="{id}" class="{id}" aria-labelledby="{id}-heading">
    <div class="{id}__container">
        <h2 id="{id}-heading" class="{id}__title">{heading}</h2>
        <!-- コンテンツ -->
    </div>
</section>
```

### Footer
```html
<footer class="footer">
    <div class="footer__container">
        <p class="footer__copyright">&copy; 2025 {site_name}. All rights reserved.</p>
        <nav class="footer__nav" aria-label="フッターナビゲーション">
            <a href="privacy-policy.html" class="footer__link">プライバシーポリシー</a>
        </nav>
    </div>
</footer>
```

---

## 9. 出力完了メッセージ

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ 実装が完了しました！
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

【生成ファイル】
📄 index.html

【品質保証】
🔒 セキュリティ: CSP, XSS対策, フォーム保護 ✅
♿ アクセシビリティ: WCAG準拠 ✅
🔧 保守性: DRY, BEM, CSS変数 ✅

【確認事項】
・ブラウザで表示確認してください
・フォーム送信先は実際のAPIに変更が必要です
・画像は適切なファイルに差し替えてください
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
