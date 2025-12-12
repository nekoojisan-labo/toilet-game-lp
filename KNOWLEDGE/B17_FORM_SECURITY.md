# B17: フォームセキュリティ

## 概要
お問い合わせフォームの多層防御実装ガイド。

---

## 1. 多層防御の構成

```
┌─────────────────────────────────────────┐
│ Layer 1: Honeypot（ボット対策）         │
├─────────────────────────────────────────┤
│ Layer 2: クライアント側バリデーション    │
├─────────────────────────────────────────┤
│ Layer 3: reCAPTCHA v3（オプション）     │
├─────────────────────────────────────────┤
│ Layer 4: サーバー側バリデーション        │
└─────────────────────────────────────────┘
```

---

## 2. Honeypot実装

### 2.1 基本実装

```html
<form id="contactForm" action="#" method="POST" novalidate>
  <!-- Honeypot（ボットトラップ） -->
  <div style="position: absolute; left: -9999px; top: -9999px;" aria-hidden="true">
    <label for="website">ウェブサイト</label>
    <input type="text" name="website" id="website" tabindex="-1" autocomplete="off">
  </div>
  
  <!-- 実際のフォームフィールド -->
  <div class="form-group">
    <label for="name">お名前</label>
    <input type="text" name="name" id="name" required>
  </div>
  ...
</form>
```

### 2.2 JavaScript検証

```javascript
form.addEventListener('submit', function(e) {
  // Honeypotチェック
  const honeypot = form.querySelector('input[name="website"]');
  if (honeypot && honeypot.value !== '') {
    e.preventDefault();
    console.log('Bot detected via honeypot');
    return false;
  }
  // 続行...
});
```

---

## 3. クライアント側バリデーション

### 3.1 HTML5属性活用

```html
<input 
  type="text" 
  name="name" 
  required 
  maxlength="100"
  pattern="^[^<>]*$"
  autocomplete="name"
>

<input 
  type="email" 
  name="email" 
  required 
  maxlength="254"
  autocomplete="email"
>

<textarea 
  name="message" 
  required 
  maxlength="5000"
></textarea>
```

### 3.2 JavaScript検証

```javascript
class FormValidator {
  constructor(form) {
    this.form = form;
    this.errors = [];
  }
  
  // XSS対策: HTMLエスケープ
  escapeHTML(str) {
    const div = document.createElement('div');
    div.textContent = str;
    return div.innerHTML;
  }
  
  // 危険文字検出
  containsDangerous(value) {
    return /<script|javascript:|on\w+\s*=/i.test(value);
  }
  
  // メール形式検証
  isValidEmail(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  }
  
  // フィールド検証
  validateField(field) {
    const value = field.value.trim();
    let isValid = true;
    let message = '';
    
    // 必須チェック
    if (field.required && !value) {
      isValid = false;
      message = 'この項目は必須です';
    }
    
    // 最大長チェック
    else if (field.maxLength > 0 && value.length > field.maxLength) {
      isValid = false;
      message = `${field.maxLength}文字以内で入力してください`;
    }
    
    // メール形式チェック
    else if (field.type === 'email' && value && !this.isValidEmail(value)) {
      isValid = false;
      message = '正しいメールアドレスを入力してください';
    }
    
    // 危険文字チェック
    else if (this.containsDangerous(value)) {
      isValid = false;
      message = '不正な文字が含まれています';
    }
    
    // エラー表示
    this.showError(field, isValid, message);
    return isValid;
  }
  
  // エラー表示
  showError(field, isValid, message) {
    const errorEl = field.parentElement.querySelector('.error-message');
    if (errorEl) {
      errorEl.textContent = isValid ? '' : message;
    }
    field.classList.toggle('error', !isValid);
    field.setAttribute('aria-invalid', !isValid);
  }
  
  // フォーム全体検証
  validate() {
    let isFormValid = true;
    this.form.querySelectorAll('input, textarea, select').forEach(field => {
      if (!this.validateField(field)) {
        isFormValid = false;
      }
    });
    return isFormValid;
  }
}

// 使用例
const form = document.getElementById('contactForm');
const validator = new FormValidator(form);

form.addEventListener('submit', function(e) {
  e.preventDefault();
  
  if (validator.validate()) {
    // 送信処理
  }
});
```

---

## 4. reCAPTCHA v3実装（オプション）

### 4.1 HTML

```html
<head>
  <script src="https://www.google.com/recaptcha/api.js?render=YOUR_SITE_KEY"></script>
</head>

<form id="contactForm">
  <input type="hidden" name="recaptcha_token" id="recaptchaToken">
  ...
</form>
```

### 4.2 JavaScript

```javascript
form.addEventListener('submit', async function(e) {
  e.preventDefault();
  
  try {
    // reCAPTCHA トークン取得
    const token = await grecaptcha.execute('YOUR_SITE_KEY', {action: 'submit'});
    document.getElementById('recaptchaToken').value = token;
    
    // サーバーに送信（トークン検証はサーバー側で）
    // ...
  } catch (error) {
    console.error('reCAPTCHA error:', error);
  }
});
```

### 4.3 CSP設定（reCAPTCHA用）

```html
<meta http-equiv="Content-Security-Policy" content="
  script-src 'self' https://www.google.com https://www.gstatic.com;
  frame-src https://www.google.com;
">
```

---

## 5. 完全なフォーム実装例

```html
<!-- お問い合わせフォーム -->
<section id="contact" class="contact">
  <div class="container">
    <h2>お問い合わせ</h2>
    
    <form id="contactForm" action="#" method="POST" novalidate>
      <!-- Honeypot -->
      <div style="position: absolute; left: -9999px;" aria-hidden="true">
        <input type="text" name="website" tabindex="-1" autocomplete="off">
      </div>
      
      <!-- 名前 -->
      <div class="form-group">
        <label for="name">お名前 <span class="required">*</span></label>
        <input 
          type="text" 
          id="name" 
          name="name" 
          required 
          maxlength="100"
          autocomplete="name"
          aria-describedby="name-error"
        >
        <span class="error-message" id="name-error" role="alert"></span>
      </div>
      
      <!-- メール -->
      <div class="form-group">
        <label for="email">メールアドレス <span class="required">*</span></label>
        <input 
          type="email" 
          id="email" 
          name="email" 
          required 
          maxlength="254"
          autocomplete="email"
          aria-describedby="email-error"
        >
        <span class="error-message" id="email-error" role="alert"></span>
      </div>
      
      <!-- 電話（任意） -->
      <div class="form-group">
        <label for="tel">電話番号</label>
        <input 
          type="tel" 
          id="tel" 
          name="tel" 
          maxlength="20"
          pattern="[\d\-\+\(\)\s]+"
          autocomplete="tel"
        >
        <span class="error-message" role="alert"></span>
      </div>
      
      <!-- お問い合わせ内容 -->
      <div class="form-group">
        <label for="message">お問い合わせ内容 <span class="required">*</span></label>
        <textarea 
          id="message" 
          name="message" 
          required 
          maxlength="5000"
          rows="5"
          aria-describedby="message-error"
        ></textarea>
        <span class="error-message" id="message-error" role="alert"></span>
      </div>
      
      <!-- 同意チェック -->
      <div class="form-group form-group--checkbox">
        <label>
          <input type="checkbox" name="privacy" required>
          <a href="privacy-policy.html" target="_blank">プライバシーポリシー</a>に同意する
        </label>
        <span class="error-message" role="alert"></span>
      </div>
      
      <!-- 送信ボタン -->
      <button type="submit" class="btn btn-primary">
        <span class="btn-text">送信する</span>
        <span class="btn-loading" hidden>送信中...</span>
      </button>
    </form>
  </div>
</section>

<style>
.form-group {
  margin-bottom: 1.5rem;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 500;
}

.form-group input,
.form-group textarea {
  width: 100%;
  padding: 0.75rem 1rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
  transition: border-color 0.3s, box-shadow 0.3s;
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px rgba(var(--color-primary-rgb), 0.1);
}

.form-group input.error,
.form-group textarea.error {
  border-color: #e53935;
}

.error-message {
  display: block;
  margin-top: 0.25rem;
  color: #e53935;
  font-size: 0.875rem;
  min-height: 1.25rem;
}

.required {
  color: #e53935;
}
</style>

<script>
document.addEventListener('DOMContentLoaded', function() {
  const form = document.getElementById('contactForm');
  if (!form) return;
  
  // バリデーション関数
  function escapeHTML(str) {
    const div = document.createElement('div');
    div.textContent = str;
    return div.innerHTML;
  }
  
  function validateField(field) {
    const value = field.value.trim();
    const errorEl = field.parentElement.querySelector('.error-message');
    let isValid = true;
    let message = '';
    
    if (field.required && !value) {
      isValid = false;
      message = 'この項目は必須です';
    } else if (field.type === 'email' && value) {
      if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value)) {
        isValid = false;
        message = '正しいメールアドレスを入力してください';
      }
    } else if (field.type === 'checkbox' && field.required && !field.checked) {
      isValid = false;
      message = 'チェックしてください';
    } else if (/<script|javascript:|on\w+=/i.test(value)) {
      isValid = false;
      message = '不正な文字が含まれています';
    }
    
    if (errorEl) errorEl.textContent = message;
    field.classList.toggle('error', !isValid);
    
    return isValid;
  }
  
  // リアルタイムバリデーション
  form.querySelectorAll('input, textarea').forEach(field => {
    field.addEventListener('blur', () => validateField(field));
  });
  
  // 送信処理
  form.addEventListener('submit', function(e) {
    e.preventDefault();
    
    // Honeypotチェック
    const honeypot = form.querySelector('input[name="website"]');
    if (honeypot && honeypot.value) return;
    
    // バリデーション
    let isValid = true;
    form.querySelectorAll('input, textarea').forEach(field => {
      if (!validateField(field)) isValid = false;
    });
    
    if (isValid) {
      // 送信処理（実際はfetch APIなどで送信）
      alert('送信が完了しました');
      form.reset();
    }
  });
});
</script>
```

---

## 6. フォームセキュリティチェックリスト

- [ ] Honeypotフィールドが実装されている
- [ ] 全必須項目にrequired属性がある
- [ ] 全入力項目にmaxlength属性がある
- [ ] メールフィールドにtype="email"がある
- [ ] XSSエスケープ関数が実装されている
- [ ] 危険文字パターンの検出がある
- [ ] エラーメッセージが適切に表示される
- [ ] プライバシーポリシー同意チェックがある
- [ ] form-action 'self'がCSPに設定されている
