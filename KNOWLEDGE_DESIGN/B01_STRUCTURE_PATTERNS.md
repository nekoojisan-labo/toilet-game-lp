# B01: 構造パターン

## このファイルの目的
サイトの種類ごとに「勝ちパターン」となる**ページ全体の構成（セクションの順序）**を定義する。

---

## 1. LP（ランディングページ）構造パターン

### LP-A: シンプル短距離型
**用途:** 資料ダウンロード、メルマガ登録、無料トライアル

```json
{
  "pattern_id": "LP-A",
  "name": "シンプル短距離型",
  "target": ["リード獲得", "資料DL", "無料登録"],
  "sections": [
    "hero",
    "benefits_3points",
    "social_proof_logos",
    "cta_form"
  ],
  "characteristics": "スクロール量短め、迷わせない、1ゴール集中",
  "recommended_hero": ["HERO-01", "HERO-02"],
  "recommended_style": ["mood:friendly", "mood:professional"]
}
```

### LP-B: 教育→納得型
**用途:** 高単価サービス、BtoB、コンサル

```json
{
  "pattern_id": "LP-B",
  "name": "教育→納得型",
  "target": ["高単価商品", "BtoB", "コンサル", "スクール"],
  "sections": [
    "hero_problem",
    "problem_deep_dive",
    "solution_overview",
    "features",
    "case_studies",
    "testimonials",
    "process_flow",
    "pricing",
    "faq",
    "cta"
  ],
  "characteristics": "ロングフォーム、教育+信頼構築重視、論理的説得",
  "recommended_hero": ["HERO-01", "HERO-03"],
  "recommended_style": ["mood:serious", "mood:premium"]
}
```

### LP-C: ビジュアル重視型
**用途:** ブランド商品、D2C、ライフスタイル

```json
{
  "pattern_id": "LP-C",
  "name": "ビジュアル重視型",
  "target": ["D2C", "ブランド商品", "ファッション", "コスメ"],
  "sections": [
    "hero_fullscreen",
    "product_visual",
    "lifestyle_gallery",
    "brand_story",
    "reviews",
    "cta"
  ],
  "characteristics": "写真・動画が主役、世界観重視、感情訴求",
  "recommended_hero": ["HERO-02", "HERO-04"],
  "recommended_style": ["mood:premium", "mood:playful"]
}
```

### LP-D: 記事LP型（ノン広告風）
**用途:** 潜在層向け、SNS流入、広告疲れ対策

```json
{
  "pattern_id": "LP-D",
  "name": "記事LP型（ノン広告風）",
  "target": ["潜在層", "SNS流入", "比較検討層"],
  "sections": [
    "article_headline",
    "problem_story",
    "solution_introduction",
    "product_as_answer",
    "social_proof",
    "soft_cta"
  ],
  "characteristics": "メディア記事風、売り込み感排除、情報提供スタイル",
  "recommended_hero": ["HERO-05"],
  "recommended_style": ["mood:friendly", "type:editorial-serif"]
}
```

### LP-E: 診断・インタラクティブ型
**用途:** パーソナライズ、エンゲージメント重視

```json
{
  "pattern_id": "LP-E",
  "name": "診断・インタラクティブ型",
  "target": ["診断コンテンツ", "マッチング", "カスタマイズ商品"],
  "sections": [
    "hero_question",
    "quiz_steps",
    "result_personalized",
    "recommended_product",
    "cta"
  ],
  "characteristics": "ユーザー参加型、没入感、パーソナライズ結果",
  "recommended_hero": ["HERO-06"],
  "recommended_style": ["mood:playful", "ui:interactive"]
}
```

---

## 2. ポートフォリオ構造パターン

### PORTFOLIO-A: 作品グリッド型
**用途:** デザイナー、クリエイター（作品数が多い）

```json
{
  "pattern_id": "PORTFOLIO-A",
  "name": "作品グリッド型",
  "target": ["デザイナー", "フォトグラファー", "イラストレーター"],
  "sections": [
    "hero_minimal",
    "work_grid",
    "about_brief",
    "skills",
    "contact"
  ],
  "characteristics": "作品が主役、グリッド配置、詳細は個別ページ",
  "recommended_hero": ["HERO-02", "HERO-07"],
  "recommended_style": ["mood:premium", "ui:minimal"]
}
```

### PORTFOLIO-B: ストーリーテリング型
**用途:** プロセス重視、採用担当向け

```json
{
  "pattern_id": "PORTFOLIO-B",
  "name": "ストーリーテリング型",
  "target": ["UXデザイナー", "プロダクトデザイナー", "転職活動"],
  "sections": [
    "hero_statement",
    "about_detailed",
    "case_study_featured",
    "process_showcase",
    "skills_tools",
    "testimonials",
    "contact"
  ],
  "characteristics": "制作プロセス重視、思考の可視化、論理的アピール",
  "recommended_hero": ["HERO-01", "HERO-03"],
  "recommended_style": ["mood:serious", "type:sans-modern"]
}
```

### PORTFOLIO-C: 個性全開型
**用途:** フリーランス、独立クリエイター

```json
{
  "pattern_id": "PORTFOLIO-C",
  "name": "個性全開型",
  "target": ["フリーランス", "アーティスト", "独立系クリエイター"],
  "sections": [
    "hero_creative",
    "manifesto",
    "work_experimental",
    "about_personal",
    "contact_casual"
  ],
  "characteristics": "自己表現優先、実験的デザイン、記憶に残る",
  "recommended_hero": ["HERO-04", "HERO-08"],
  "recommended_style": ["mood:playful", "ui:brutalism"]
}
```

---

## 3. コーポレートサイト構造パターン

### CORPORATE-A: 信頼重視型
**用途:** BtoB企業、金融、医療、士業

```json
{
  "pattern_id": "CORPORATE-A",
  "name": "信頼重視型",
  "target": ["BtoB", "金融", "医療", "士業", "大企業"],
  "sections": [
    "hero_corporate",
    "message",
    "service_overview",
    "strength",
    "case_studies",
    "company_info",
    "news",
    "contact"
  ],
  "characteristics": "予測可能な構造、信頼感、情報の網羅性",
  "recommended_hero": ["HERO-01", "HERO-09"],
  "recommended_style": ["mood:serious", "color:corporate-blue"]
}
```

### CORPORATE-B: ブランド訴求型
**用途:** 消費財メーカー、IT企業、スタートアップ

```json
{
  "pattern_id": "CORPORATE-B",
  "name": "ブランド訴求型",
  "target": ["IT企業", "スタートアップ", "消費財"],
  "sections": [
    "hero_brand",
    "vision",
    "products_services",
    "culture",
    "team",
    "news",
    "careers",
    "contact"
  ],
  "characteristics": "世界観重視、採用も意識、動きのあるデザイン",
  "recommended_hero": ["HERO-02", "HERO-04"],
  "recommended_style": ["mood:premium", "ui:modern"]
}
```

### CORPORATE-C: 地域密着型
**用途:** 中小企業、店舗、地域サービス

```json
{
  "pattern_id": "CORPORATE-C",
  "name": "地域密着型",
  "target": ["中小企業", "店舗", "地域サービス", "クリニック"],
  "sections": [
    "hero_friendly",
    "about_story",
    "service",
    "staff",
    "voice",
    "access",
    "contact"
  ],
  "characteristics": "親しみやすさ、顔が見える、アクセス情報重視",
  "recommended_hero": ["HERO-01", "HERO-10"],
  "recommended_style": ["mood:friendly", "color:warm"]
}
```

---

## 4. 2025年のトレンド構造

### 4-1. PC: 3カラムレイアウト
従来の縦長1カラムではなく、横幅を活用：

```json
{
  "pattern_id": "LP-3COL",
  "name": "3カラムレイアウト（PC専用）",
  "layout": {
    "left": "メインコンテンツ（テキスト中心）",
    "center": "ビジュアル（画像・動画）",
    "right": "CTA・補足情報・追従要素"
  },
  "benefits": [
    "スクロール量削減",
    "ファーストビューの情報伝達効率最大化",
    "CTAが常に視界に入る"
  ],
  "best_for": ["BtoB", "SaaS", "高単価サービス"]
}
```

### 4-2. スマホ: スワイプ型LP
Instagramストーリーズのように横スワイプで進む形式：

```json
{
  "pattern_id": "LP-SWIPE",
  "name": "スワイプ型LP（スマホ専用）",
  "interaction": "横スワイプで次のスライドへ",
  "benefits": [
    "没入感（インタラクティブ性）",
    "能動的操作による離脱防止",
    "ストーリー性のある体験"
  ],
  "structure": ["問題提起", "共感", "解決策", "証拠", "CTA"],
  "best_for": ["D2C", "コスメ", "若年層向け", "SNS流入"]
}
```

### 4-3. ノン広告風デザイン（記事LP）
「広告臭」を消し、メディア記事のような見た目：

```json
{
  "pattern_id": "LP-ARTICLE",
  "name": "記事LP（ノン広告風）",
  "characteristics": [
    "派手な装飾を排除",
    "余白と配色を活かしたミニマルデザイン",
    "情報提供型のコンテンツ",
    "CTAは後半に控えめに配置"
  ],
  "best_for": ["潜在層向け", "SNS広告からの流入", "高単価・検討期間長い商材"]
}
```

### 4-4. 媒体別最適化の考え方

| 流入元 | ユーザー状態 | 推奨LP構造 |
|--------|-------------|-----------|
| Google検索（顕在層）| 解決策を探している | LP-A, LP-B（ストレート訴求）|
| Instagram（潜在層）| 興味本位、暇つぶし | LP-D, LP-E（診断・記事型）|
| Facebook広告 | やや顕在寄り | LP-B（教育→納得型）|
| LINE広告 | リピーター・既存顧客 | LP-A（シンプル短距離）|
| TikTok | 衝動的、短時間 | LP-SWIPE（体験重視）|

---

## 5. 構造パターンの選び方

### 判断フロー

```
Q1: サイトの目的は？
├── 単一アクション（購入・登録）→ LP
├── 自己紹介・作品紹介 → ポートフォリオ
└── 会社情報・複数サービス → コーポレート

Q2: ターゲットの状態は？
├── 顕在層（今すぐ欲しい）→ 短距離型
├── 潜在層（まだ悩んでる）→ 教育型・記事LP型
└── 比較検討中 → 信頼重視型

Q3: 何で差別化する？
├── 論理・実績 → ストーリーテリング型
├── 世界観・感情 → ビジュアル重視型
└── 安心感・誠実さ → 信頼重視型
```

---

## 6. 構造パターン × スタイルの組み合わせ例

| ケース | 構造 | スタイル |
|--------|------|----------|
| SaaS LP | LP-B（教育→納得型）| color:minimal-light, mood:professional |
| D2Cコスメ | LP-C（ビジュアル重視）| color:pastel, mood:premium |
| UIデザイナー転職 | PORTFOLIO-B（ストーリーテリング）| color:dark-tech, mood:serious |
| 地方クリニック | CORPORATE-C（地域密着）| color:warm, mood:friendly |
| ITスタートアップ | CORPORATE-B（ブランド訴求）| color:tech-gradient, mood:innovative |
