# B20: 保守性・コード品質

## 概要
長期的な運用に耐える堅牢なコードを書くための原則。
「書く時間」より「読まれる時間」の方が長いことを前提とした設計指針。

---

## 1. DRY原則（Don't Repeat Yourself）

### 1.1 基本概念

> **「システム内で、すべての知識は単一、明確、かつ権威ある表現を持つべきである」**

重複があると：
- 仕様変更時にすべての箇所を修正する必要
- 修正漏れがバグの原因に
- コードの肥大化で可読性低下

### 1.2 実装例：CSS変数による一元管理

```css
/* ✅ DRY：変数で一元管理 */
:root {
  --color-primary: #2563eb;
  --color-primary-hover: #1d4ed8;
  --color-text: #1f2937;
  --color-background: #ffffff;
  
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 2rem;
  
  --font-size-base: 1rem;
  --font-size-lg: 1.25rem;
  --font-size-xl: 1.5rem;
  
  --border-radius: 8px;
  --transition-base: 0.3s ease;
}

.button {
  background: var(--color-primary);
  padding: var(--spacing-sm) var(--spacing-md);
  border-radius: var(--border-radius);
  transition: background var(--transition-base);
}

.button:hover {
  background: var(--color-primary-hover);
}

/* ❌ WET（重複）：同じ値が散在 */
.button {
  background: #2563eb;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  transition: background 0.3s ease;
}

.card {
  border-radius: 8px;  /* 同じ値が重複 */
  padding: 1rem;       /* 同じ値が重複 */
}
```

### 1.3 実装例：JavaScript関数の共通化

```javascript
/* ✅ DRY：共通関数に集約 */
// 共通のバリデーション関数
const validators = {
  required: (value) => value.trim() !== '' || 'この項目は必須です',
  email: (value) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value) || '正しいメールアドレスを入力してください',
  maxLength: (max) => (value) => value.length <= max || `${max}文字以内で入力してください`,
  noScript: (value) => !/<script|javascript:|on\w+=/i.test(value) || '不正な文字が含まれています'
};

// 使用例
function validateField(field, rules) {
  for (const rule of rules) {
    const result = rule(field.value);
    if (result !== true) {
      return { valid: false, message: result };
    }
  }
  return { valid: true };
}

// フィールドごとのルール定義
const fieldRules = {
  name: [validators.required, validators.maxLength(100), validators.noScript],
  email: [validators.required, validators.email],
  message: [validators.required, validators.maxLength(5000), validators.noScript]
};

/* ❌ WET：同じロジックが重複 */
function validateName(value) {
  if (value.trim() === '') return 'この項目は必須です';
  if (value.length > 100) return '100文字以内で入力してください';
  if (/<script|javascript:|on\w+=/i.test(value)) return '不正な文字が含まれています';
  return true;
}

function validateEmail(value) {
  if (value.trim() === '') return 'この項目は必須です';  // 重複
  if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value)) return '正しいメールアドレスを入力してください';
  return true;
}
```

### 1.4 設定値の集約

```javascript
/* ✅ DRY：設定ファイルに集約 */
// config.js
const CONFIG = {
  API_ENDPOINT: 'https://api.example.com',
  RECAPTCHA_SITE_KEY: 'your-site-key',
  MAX_FILE_SIZE: 5 * 1024 * 1024, // 5MB
  SUPPORTED_FORMATS: ['image/jpeg', 'image/png', 'image/webp'],
  ANIMATION_DURATION: 300,
  BREAKPOINTS: {
    mobile: 640,
    tablet: 768,
    desktop: 1024
  }
};

export default CONFIG;

// 使用側
import CONFIG from './config.js';
fetch(`${CONFIG.API_ENDPOINT}/contact`, { ... });
```

---

## 2. 単一責任の原則（SRP）

### 2.1 基本概念

> **「一つのクラスや関数は、一つの責任（機能）だけを持つべき」**

変更理由が一つに限定されることで、影響範囲を最小化。

### 2.2 実装例：関数の分離

```javascript
/* ✅ SRP：責任を分離 */

// 1. データ取得の責任
async function fetchFormData() {
  const form = document.getElementById('contactForm');
  return new FormData(form);
}

// 2. バリデーションの責任
function validateFormData(data) {
  const errors = [];
  if (!data.get('name')) errors.push({ field: 'name', message: '名前は必須です' });
  if (!data.get('email')) errors.push({ field: 'email', message: 'メールは必須です' });
  return errors;
}

// 3. API送信の責任
async function submitToAPI(data) {
  const response = await fetch('/api/contact', {
    method: 'POST',
    body: data
  });
  return response.json();
}

// 4. UI更新の責任
function showErrors(errors) {
  errors.forEach(({ field, message }) => {
    const errorEl = document.querySelector(`#${field}-error`);
    if (errorEl) errorEl.textContent = message;
  });
}

function showSuccess() {
  document.getElementById('successMessage').hidden = false;
}

// 統合（オーケストレーション）
async function handleSubmit(event) {
  event.preventDefault();
  
  const data = await fetchFormData();
  const errors = validateFormData(data);
  
  if (errors.length > 0) {
    showErrors(errors);
    return;
  }
  
  const result = await submitToAPI(data);
  showSuccess();
}


/* ❌ 違反：一つの関数に複数の責任 */
async function handleSubmit(event) {
  event.preventDefault();
  const form = document.getElementById('contactForm');  // データ取得
  const data = new FormData(form);
  
  // バリデーション
  if (!data.get('name')) {
    document.querySelector('#name-error').textContent = '名前は必須です';  // UI更新
    return;
  }
  
  // API送信
  const response = await fetch('/api/contact', { method: 'POST', body: data });
  
  // UI更新
  document.getElementById('successMessage').hidden = false;
}
```

---

## 3. 可読性と意図の明示

### 3.1 コメントの書き方

```javascript
/* ✅ 良いコメント：「なぜ」を説明 */

// Honeypotフィールド：ボットはすべてのフィールドを埋めるため、
// この隠しフィールドに値があればボットと判定できる
const honeypot = form.querySelector('input[name="website"]');
if (honeypot && honeypot.value) {
  return; // ボット検出時は静かに処理を終了（エラー表示しない）
}

// iOS Safariでは100vhが正しく動作しないため、
// JavaScript で実際のビューポート高さを計算
const vh = window.innerHeight * 0.01;
document.documentElement.style.setProperty('--vh', `${vh}px`);

// 税率は法改正で変更される可能性があるため、
// CONFIG に集約して一箇所で管理
const TAX_RATE = CONFIG.TAX_RATE; // 現在10%


/* ❌ 悪いコメント：「何をしているか」の繰り返し */

// honeypotの値を取得
const honeypot = form.querySelector('input[name="website"]');
// 値があるかチェック
if (honeypot && honeypot.value) {
  return; // returnする
}
```

### 3.2 命名規則

```javascript
/* ✅ 意図が伝わる命名 */

// 動詞 + 名詞 で何をするか明確
function validateEmail(email) { ... }
function formatPhoneNumber(number) { ... }
function calculateTotalPrice(items) { ... }

// Boolean は is/has/can/should で始める
const isValid = validateEmail(email);
const hasErrors = errors.length > 0;
const canSubmit = isValid && !hasErrors;

// 定数は大文字スネークケース
const MAX_RETRY_COUNT = 3;
const API_TIMEOUT_MS = 5000;


/* ❌ 曖昧な命名 */
function check(e) { ... }  // 何をチェック？
function process(d) { ... }  // 何を処理？
const flag = true;  // 何のフラグ？
const data = [];  // 何のデータ？
```

### 3.3 CSS命名（BEM）

```css
/* BEM: Block__Element--Modifier */

/* Block: 独立したコンポーネント */
.card { }

/* Element: ブロックの構成要素 */
.card__header { }
.card__title { }
.card__body { }
.card__footer { }

/* Modifier: 状態やバリエーション */
.card--featured { }
.card--disabled { }
.card__title--large { }
```

---

## 4. KISS原則（Keep It Simple, Stupid）

### 4.1 基本概念

> **「シンプルさを維持し、不必要に複雑な構造を避ける」**

```javascript
/* ✅ KISS：シンプルな実装 */
function isAdult(age) {
  return age >= 18;
}


/* ❌ 過度に複雑 */
function isAdult(age) {
  const ADULT_AGE = 18;
  const isValidAge = typeof age === 'number' && !isNaN(age);
  if (!isValidAge) {
    throw new Error('Invalid age');
  }
  const result = age >= ADULT_AGE;
  return Boolean(result);
}
```

### 4.2 条件分岐のシンプル化

```javascript
/* ✅ 早期リターンでネスト削減 */
function processUser(user) {
  if (!user) return null;
  if (!user.isActive) return null;
  if (user.isBanned) return null;
  
  return {
    id: user.id,
    name: user.name
  };
}


/* ❌ 深いネスト */
function processUser(user) {
  if (user) {
    if (user.isActive) {
      if (!user.isBanned) {
        return {
          id: user.id,
          name: user.name
        };
      }
    }
  }
  return null;
}
```

---

## 5. YAGNI原則（You Aren't Gonna Need It）

> **「今必要なものだけを作る。将来の推測で機能を実装しない」**

```javascript
/* ✅ YAGNI：今必要な機能だけ */
function sendContactForm(data) {
  return fetch('/api/contact', {
    method: 'POST',
    body: JSON.stringify(data)
  });
}


/* ❌ 過度な汎用化（使われない機能） */
function sendForm(data, options = {}) {
  const {
    method = 'POST',
    endpoint = '/api/contact',
    headers = {},
    retryCount = 3,
    timeout = 5000,
    onProgress = () => {},
    onRetry = () => {},
    transformRequest = (d) => d,
    transformResponse = (d) => d,
    // ... 今は使わない大量のオプション
  } = options;
  
  // 複雑な実装...
}
```

---

## 6. WET原則（Write Everything Twice）

### 6.1 DRYの過度な適用を避ける

> **「パターンが明確になるまで重複を許容する」**

最初から共通化すると：
- 間違った抽象化になるリスク
- 後から分離が困難に

```javascript
/* 段階的アプローチ */

// Step 1: まずは重複を許容して実装
function validateNameField() { ... }
function validateEmailField() { ... }

// Step 2: パターンを発見
// 「必須チェック」「形式チェック」「危険文字チェック」の共通パターン

// Step 3: 共通化
const validators = { required, email, noScript };
```

---

## 7. チェックリスト

### DRY
- [ ] 同じ値がコード内に散在していない
- [ ] CSS変数で色・サイズ・スペーシングを一元管理
- [ ] 共通ロジックは関数/モジュールに集約
- [ ] 設定値は設定ファイルに集約

### SRP
- [ ] 一つの関数は一つの責任のみ
- [ ] 関数名から責任が明確
- [ ] 変更時の影響範囲が限定的

### 可読性
- [ ] 「なぜ」を説明するコメント
- [ ] 意図が伝わる命名
- [ ] 一貫した命名規則（BEM等）
- [ ] 早期リターンでネスト削減

### KISS/YAGNI
- [ ] 過度な汎用化を避ける
- [ ] 今必要な機能だけを実装
- [ ] シンプルな実装を優先
