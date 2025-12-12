# B18: プライバシーコンプライアンス

## 概要
GDPR・改正個人情報保護法対応のCookie同意バナー実装ガイド。

---

## 1. 対応が必要なケース

| 条件 | Cookie同意 | 対応レベル |
|------|-----------|-----------|
| Google Analytics使用 | 必要 | 標準 |
| YouTube埋め込み | 必要 | 標準 |
| 広告トラッキング | 必要 | 厳格 |
| EU向けサービス | 必要 | GDPR準拠 |
| 日本国内のみ | 推奨 | 改正個人情報保護法 |
| 外部サービスなし | 不要 | - |

---

## 2. Cookie同意バナー実装

### 2.1 HTML

```html
<!-- Cookie同意バナー -->
<div id="cookieConsent" class="cookie-consent" role="dialog" aria-labelledby="cookieTitle" aria-describedby="cookieDesc">
  <div class="cookie-consent__content">
    <h3 id="cookieTitle" class="cookie-consent__title">Cookieの使用について</h3>
    <p id="cookieDesc" class="cookie-consent__text">
      当サイトでは、サービス向上のためCookieを使用しています。
      Cookieの使用に同意いただける場合は「同意する」をクリックしてください。
      詳細は<a href="privacy-policy.html">プライバシーポリシー</a>をご確認ください。
    </p>
    <div class="cookie-consent__actions">
      <button id="cookieAccept" class="btn btn-primary">同意する</button>
      <button id="cookieDecline" class="btn btn-secondary">拒否する</button>
    </div>
  </div>
</div>
```

### 2.2 CSS

```css
.cookie-consent {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: #fff;
  box-shadow: 0 -4px 20px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
  z-index: 9999;
  transform: translateY(100%);
  transition: transform 0.3s ease;
}

.cookie-consent.active {
  transform: translateY(0);
}

.cookie-consent__content {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 1rem;
}

.cookie-consent__title {
  font-size: 1rem;
  font-weight: 600;
  margin: 0;
  flex: 0 0 100%;
}

.cookie-consent__text {
  flex: 1;
  margin: 0;
  font-size: 0.875rem;
  color: #666;
}

.cookie-consent__text a {
  color: var(--color-primary);
  text-decoration: underline;
}

.cookie-consent__actions {
  display: flex;
  gap: 0.5rem;
  flex-shrink: 0;
}

.cookie-consent__actions .btn {
  padding: 0.5rem 1.5rem;
  font-size: 0.875rem;
}

@media (max-width: 768px) {
  .cookie-consent__content {
    flex-direction: column;
    text-align: center;
  }
  
  .cookie-consent__actions {
    width: 100%;
    justify-content: center;
  }
}
```

### 2.3 JavaScript

```javascript
// Cookie同意管理
const CookieConsent = {
  COOKIE_NAME: 'cookie_consent',
  COOKIE_DAYS: 365,
  
  init() {
    const consent = this.getConsent();
    
    if (consent === null) {
      // 未選択：バナー表示
      this.showBanner();
    } else if (consent === 'accepted') {
      // 同意済み：トラッキング有効化
      this.enableTracking();
    }
    // 拒否済みの場合は何もしない
    
    this.bindEvents();
  },
  
  getConsent() {
    const match = document.cookie.match(new RegExp('(^| )' + this.COOKIE_NAME + '=([^;]+)'));
    return match ? match[2] : null;
  },
  
  setConsent(value) {
    const date = new Date();
    date.setTime(date.getTime() + (this.COOKIE_DAYS * 24 * 60 * 60 * 1000));
    document.cookie = `${this.COOKIE_NAME}=${value};expires=${date.toUTCString()};path=/;SameSite=Lax`;
  },
  
  showBanner() {
    const banner = document.getElementById('cookieConsent');
    if (banner) {
      setTimeout(() => banner.classList.add('active'), 500);
    }
  },
  
  hideBanner() {
    const banner = document.getElementById('cookieConsent');
    if (banner) {
      banner.classList.remove('active');
    }
  },
  
  enableTracking() {
    // Google Analytics有効化
    if (typeof gtag !== 'undefined') {
      gtag('consent', 'update', {
        'analytics_storage': 'granted'
      });
    }
    
    // その他のトラッキング有効化
    window.dispatchEvent(new CustomEvent('cookieConsentGranted'));
  },
  
  disableTracking() {
    // Google Analytics無効化
    if (typeof gtag !== 'undefined') {
      gtag('consent', 'update', {
        'analytics_storage': 'denied'
      });
    }
    
    window.dispatchEvent(new CustomEvent('cookieConsentDenied'));
  },
  
  bindEvents() {
    const acceptBtn = document.getElementById('cookieAccept');
    const declineBtn = document.getElementById('cookieDecline');
    
    if (acceptBtn) {
      acceptBtn.addEventListener('click', () => {
        this.setConsent('accepted');
        this.enableTracking();
        this.hideBanner();
      });
    }
    
    if (declineBtn) {
      declineBtn.addEventListener('click', () => {
        this.setConsent('declined');
        this.disableTracking();
        this.hideBanner();
      });
    }
  }
};

// 初期化
document.addEventListener('DOMContentLoaded', () => {
  CookieConsent.init();
});
```

---

## 3. Google Analytics + Consent Mode v2

### 3.1 デフォルト拒否で初期化

```html
<!-- Google tag (gtag.js) - Consent Mode v2対応 -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  
  // デフォルトは拒否状態
  gtag('consent', 'default', {
    'analytics_storage': 'denied',
    'ad_storage': 'denied',
    'ad_user_data': 'denied',
    'ad_personalization': 'denied',
    'wait_for_update': 500
  });
  
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX', {
    'anonymize_ip': true
  });
</script>
```

### 3.2 同意後の更新

```javascript
// Cookie同意後に呼び出し
function updateGoogleConsent(granted) {
  if (typeof gtag !== 'undefined') {
    gtag('consent', 'update', {
      'analytics_storage': granted ? 'granted' : 'denied',
      'ad_storage': granted ? 'granted' : 'denied',
      'ad_user_data': granted ? 'granted' : 'denied',
      'ad_personalization': granted ? 'granted' : 'denied'
    });
  }
}
```

---

## 4. プライバシーポリシーテンプレート

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>プライバシーポリシー | {サイト名}</title>
  
  <!-- セキュリティヘッダー -->
  <meta http-equiv="Content-Security-Policy" content="
    default-src 'self';
    style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
    font-src 'self' https://fonts.gstatic.com;
    frame-ancestors 'self';
    base-uri 'self';
  ">
  <meta name="referrer" content="strict-origin-when-cross-origin">
</head>
<body>
  <main class="legal-page">
    <div class="container">
      <h1>プライバシーポリシー</h1>
      <p class="last-updated">最終更新日：{YYYY年MM月DD日}</p>
      
      <section>
        <h2>1. はじめに</h2>
        <p>
          {会社名/サービス名}（以下「当社」）は、お客様の個人情報の保護を重要な責務と考え、
          以下のプライバシーポリシーに従って個人情報を取り扱います。
        </p>
      </section>
      
      <section>
        <h2>2. 収集する情報</h2>
        <p>当社は、以下の情報を収集することがあります：</p>
        <ul>
          <li><strong>お問い合わせ情報</strong>：お名前、メールアドレス、電話番号など</li>
          <li><strong>利用情報</strong>：アクセスログ、閲覧履歴、IPアドレスなど</li>
          <li><strong>Cookie情報</strong>：下記「Cookieの使用について」をご参照ください</li>
        </ul>
      </section>
      
      <section>
        <h2>3. 情報の利用目的</h2>
        <p>収集した情報は、以下の目的で利用します：</p>
        <ul>
          <li>お問い合わせへの対応</li>
          <li>サービスの提供・改善</li>
          <li>統計データの作成（個人を特定しない形式）</li>
        </ul>
      </section>
      
      <section>
        <h2>4. Cookieの使用について</h2>
        <p>
          当サイトでは、サービス向上のためCookieを使用しています。
          Cookieとは、ウェブサイトがお客様のブラウザに保存する小さなテキストファイルです。
        </p>
        
        <h3>4.1 使用するCookie</h3>
        <table>
          <thead>
            <tr>
              <th>種類</th>
              <th>目的</th>
              <th>提供元</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>分析Cookie</td>
              <td>アクセス解析</td>
              <td>Google Analytics</td>
            </tr>
          </tbody>
        </table>
        
        <h3>4.2 Cookieの管理</h3>
        <p>
          ブラウザの設定でCookieを無効にすることができます。
          ただし、無効にした場合、一部機能が利用できなくなる可能性があります。
        </p>
      </section>
      
      <section>
        <h2>5. 第三者への提供</h2>
        <p>
          当社は、法令に基づく場合を除き、お客様の同意なく個人情報を第三者に提供することはありません。
        </p>
      </section>
      
      <section>
        <h2>6. セキュリティ</h2>
        <p>
          当社は、個人情報の漏洩、滅失、毀損を防止するため、適切なセキュリティ対策を講じています。
        </p>
      </section>
      
      <section>
        <h2>7. お問い合わせ</h2>
        <p>
          プライバシーポリシーに関するお問い合わせは、以下までご連絡ください。<br>
          {会社名}<br>
          メール：{email@example.com}
        </p>
      </section>
      
      <section>
        <h2>8. 改定について</h2>
        <p>
          当社は、必要に応じて本ポリシーを改定することがあります。
          重要な変更がある場合は、サイト上でお知らせします。
        </p>
      </section>
    </div>
  </main>
</body>
</html>
```

---

## 5. チェックリスト

### Cookie同意バナー
- [ ] バナーが初回訪問時に表示される
- [ ] 同意/拒否ボタンがある
- [ ] 選択がCookieに保存される
- [ ] 再訪問時はバナーが表示されない
- [ ] プライバシーポリシーへのリンクがある

### Google Analytics対応
- [ ] Consent Mode v2でデフォルト拒否
- [ ] 同意後にのみ有効化
- [ ] IPアドレス匿名化設定

### プライバシーポリシー
- [ ] 収集情報の記載
- [ ] 利用目的の記載
- [ ] Cookie使用の説明
- [ ] 第三者提供の方針
- [ ] 連絡先の記載
- [ ] 最終更新日の記載

---

## 6. 地域別対応

| 地域 | 法律 | 要件 |
|------|------|------|
| EU | GDPR | 明示的同意、オプトイン必須 |
| 日本 | 改正個人情報保護法 | 利用目的の通知、同意推奨 |
| 米国（カリフォルニア） | CCPA | オプトアウト権の提供 |

### GDPR準拠の追加要件
- オプトイン（事前同意）必須
- 同意撤回機能の提供
- データ削除リクエスト対応
- データポータビリティ対応
