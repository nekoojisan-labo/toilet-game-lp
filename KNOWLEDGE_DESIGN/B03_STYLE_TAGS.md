# B03: スタイルタグ

## このファイルの目的
デザインの「見た目」を**変数化**し、構造パターン × スタイルタグの組み合わせで多様なデザインを生成できるようにする。

---

## 1. カラータグ（color:）

### ライトベース系

#### color:minimal-light
```yaml
name: ミニマルライト
background: "#FFFFFF" # 白
background_alt: "#F5F7FA" # ペールグレー
text_primary: "#1A1A1A" # ほぼ黒
text_secondary: "#6B7280" # グレー
accent: "#3B82F6" # ブルー（1色のみ）
特徴: 余白重視、情報を際立たせる、クリーン
向いている: SaaS、テック、ミニマルデザイン
```

#### color:warm-beige
```yaml
name: ウォームベージュ
background: "#F6F4E8" # 生成り
background_alt: "#EDE9DB"
text_primary: "#2B2B2B" # 墨色
text_secondary: "#6B6B6B"
accent: "#C5A059" # 金茶
特徴: 温かみ、オーガニック、上品
向いている: カフェ、ナチュラル系、和モダン
```

#### color:pastel
```yaml
name: パステル
background: "#FFF5F5" # 薄ピンク or
background_alt: "#F0F9FF" # 薄ブルー
text_primary: "#374151"
text_secondary: "#9CA3AF"
accent: "#F472B6" # ピンク or "#60A5FA" # ブルー
特徴: 柔らかい、親しみやすい、フェミニン
向いている: コスメ、ベビー、ウェディング
```

### ダークベース系

#### color:dark-tech
```yaml
name: ダークテック
background: "#0F172A" # ダークネイビー
background_alt: "#1E293B"
text_primary: "#F8FAFC" # オフホワイト
text_secondary: "#94A3B8"
accent: "#22D3EE" # シアン or "#A855F7" # パープル
特徴: 先進的、プロフェッショナル、高級感
向いている: IT、ゲーム、クリプト、先端技術
```

#### color:dark-luxury
```yaml
name: ダークラグジュアリー
background: "#1A1A1A" # 漆黒
background_alt: "#262626"
text_primary: "#FAFAFA"
text_secondary: "#A3A3A3"
accent: "#D4AF37" # ゴールド
特徴: 高級感、重厚、エクスクルーシブ
向いている: ジュエリー、高級車、ハイブランド
```

### ビビッド系

#### color:vibrant-pop
```yaml
name: ビビッドポップ
background: "#FFFFFF"
background_alt: "#FEF3C7" # 薄い黄色
text_primary: "#1F2937"
accent_primary: "#F43F5E" # ピンク
accent_secondary: "#8B5CF6" # パープル
accent_tertiary: "#06B6D4" # シアン
特徴: 元気、カラフル、注目を集める
向いている: エンタメ、キッズ、SNSサービス
```

#### color:neon
```yaml
name: ネオン
background: "#111111"
text_primary: "#FFFFFF"
accent_primary: "#DFFF00" # ネオンイエロー
accent_secondary: "#FF00FF" # マゼンタ
accent_tertiary: "#00FFFF" # シアン
特徴: 未来的、クラブ、アンダーグラウンド
向いている: 音楽、ナイトライフ、前衛的プロジェクト
```

### 業界特化系

#### color:corporate-blue
```yaml
name: コーポレートブルー
background: "#FFFFFF"
background_alt: "#F1F5F9"
text_primary: "#1E293B"
accent: "#2563EB" # ロイヤルブルー
特徴: 信頼、安定、誠実
向いている: 金融、保険、BtoB、士業
```

#### color:medical-clean
```yaml
name: メディカルクリーン
background: "#FFFFFF"
background_alt: "#F0FDFA" # 薄いミント
text_primary: "#134E4A"
accent: "#14B8A6" # ティール
特徴: 清潔、安心、専門性
向いている: 医療、クリニック、ヘルスケア
```

#### color:eco-green
```yaml
name: エコグリーン
background: "#FAFDF7"
background_alt: "#ECFCCB"
text_primary: "#1A2E05"
accent: "#65A30D" # ライムグリーン
特徴: 自然、サステナブル、健康
向いている: オーガニック、環境、農業
```

#### color:japanese-traditional
```yaml
name: 和風
background: "#F6F4E8" # 生成り
text_primary: "#2B2B2B" # 墨色
accent_primary: "#C32121" # 緋色
accent_secondary: "#1D365D" # 藍色
accent_gold: "#C5A059" # 金泥
特徴: 伝統、高級、日本らしさ
向いている: 和食、旅館、伝統工芸、日本酒
```

---

## 2. タイポグラフィタグ（type:）

#### type:sans-modern
```yaml
name: モダンサンセリフ
heading_font: "Noto Sans JP" # or Inter, Helvetica
body_font: "Noto Sans JP"
heading_weight: 700
body_weight: 400
letter_spacing: "0.02em" # 少し広め
line_height: 1.7
特徴: 現代的、読みやすい、汎用性高い
向いている: SaaS、コーポレート、ほとんどのサイト
```

#### type:editorial-serif
```yaml
name: エディトリアルセリフ
heading_font: "Noto Serif JP" # or Shippori Mincho
body_font: "Noto Sans JP" # 本文はサンセリフ
heading_weight: 600
heading_style: "normal" # or italic（英語のみ）
letter_spacing: "0.05em"
line_height: 1.8
特徴: エレガント、知的、雑誌風
向いている: ブランド、ラグジュアリー、メディア
```

#### type:bold-impact
```yaml
name: ボールドインパクト
heading_font: "Noto Sans JP" # 極太
body_font: "Noto Sans JP"
heading_weight: 900
heading_size_ratio: 1.5 # 通常より大きく
letter_spacing: "-0.02em" # 詰め気味
特徴: 力強い、インパクト、主張
向いている: スポーツ、キャンペーン、LP
```

#### type:mono-tech
```yaml
name: モノスペーステック
heading_font: "Source Code Pro" # or JetBrains Mono
body_font: "Noto Sans JP"
heading_weight: 600
特徴: 技術的、コード感、ギーク
向いている: 開発者向け、SaaS、テック系
```

#### type:handwritten-casual
```yaml
name: 手書き風カジュアル
heading_font: "Yusei Magic" # or 手書き風フォント
body_font: "Noto Sans JP"
特徴: 親しみ、カジュアル、温かみ
向いている: カフェ、ハンドメイド、子供向け
注意: 使いすぎると読みにくくなる
```

---

## 3. UIスタイルタグ（ui:）

#### ui:flat
```yaml
name: フラット
shadow: "none"
border_radius: "4px" # 小さめ角丸
border: "1px solid #E5E7EB" # 薄い線
特徴: シンプル、クリーン、標準的
向いている: ほとんどのサイト、BtoB
```

#### ui:soft-shadow
```yaml
name: ソフトシャドウ
shadow: "0 4px 6px -1px rgba(0,0,0,0.1)"
border_radius: "8px"
border: "none"
特徴: 浮遊感、モダン、優しい
向いている: SaaS、カード型UI
```

#### ui:neumorphism
```yaml
name: ニューモーフィズム
background: "#E0E5EC"
shadow_light: "-5px -5px 10px #FFFFFF"
shadow_dark: "5px 5px 10px #A3B1C6"
border_radius: "12px"
特徴: 凹凸、柔らかい、3D感
向いている: 計算機アプリ風、実験的UI
注意: アクセシビリティに難あり
```

#### ui:glassmorphism
```yaml
name: グラスモーフィズム
background: "rgba(255,255,255,0.2)"
backdrop_filter: "blur(10px)"
border: "1px solid rgba(255,255,255,0.3)"
border_radius: "16px"
特徴: 透明感、未来的、レイヤー感
向いている: ダッシュボード、テック系、iOS風
```

#### ui:brutalism
```yaml
name: ブルータリズム
shadow: "4px 4px 0 #000000" # ハードシャドウ
border: "3px solid #000000"
border_radius: "0" # 角丸なし
特徴: 生々しい、パンク、記憶に残る
向いている: アート、実験的、個性派
```

#### ui:minimal-line
```yaml
name: ミニマルライン
shadow: "none"
border: "none"
separator: "1px solid #E5E7EB" # 線だけで区切る
特徴: 極限までシンプル、余白重視
向いている: ギャラリー、ポートフォリオ、ハイブランド
```

---

## 4. ムードタグ（mood:）

#### mood:friendly
```yaml
name: フレンドリー
impression: "親しみやすい、温かい、気軽"
color_tendency: "暖色、パステル、明るい"
typography: "丸め、やや大きめ"
imagery: "笑顔、日常、明るい写真"
animation: "柔らかい、バウンス"
向いている: 地域サービス、カフェ、ファミリー向け
```

#### mood:professional
```yaml
name: プロフェッショナル
impression: "信頼、実績、専門性"
color_tendency: "青系、グレー、落ち着いた色"
typography: "直線的、読みやすい"
imagery: "オフィス、チーム、グラフ"
animation: "控えめ、フェード"
向いている: BtoB、コンサル、士業
```

#### mood:premium
```yaml
name: プレミアム
impression: "高級、エクスクルーシブ、上質"
color_tendency: "黒、白、金、深い色"
typography: "セリフ、細身、字間広め"
imagery: "高品質写真、ディテール、素材感"
animation: "遅め、エレガント"
向いている: ラグジュアリー、高単価サービス
```

#### mood:playful
```yaml
name: プレイフル
impression: "楽しい、元気、ワクワク"
color_tendency: "ビビッド、多色使い"
typography: "太字、大きめ、遊び心"
imagery: "イラスト、ポップ、動き"
animation: "バウンス、回転、派手め"
向いている: ゲーム、子供向け、エンタメ
```

#### mood:serious
```yaml
name: シリアス
impression: "真剣、重要、信頼"
color_tendency: "ダーク、モノトーン"
typography: "硬め、読みやすい"
imagery: "抽象、データ、落ち着いた"
animation: "最小限"
向いている: 金融、法律、医療、ニュース
```

#### mood:innovative
```yaml
name: イノベーティブ
impression: "先進的、革新、未来"
color_tendency: "グラデーション、ネオン、テック系"
typography: "幾何学的、太め"
imagery: "3D、抽象、宇宙"
animation: "パーティクル、グリッチ、没入"
向いている: AI、テック、スタートアップ
```

#### mood:calm
```yaml
name: カーム
impression: "静寂、癒し、リラックス"
color_tendency: "パステル、アースカラー、彩度低め"
typography: "細身、余白多め"
imagery: "自然、空、水、ぼかし"
animation: "ゆっくり、フェード"
向いている: ウェルネス、スパ、瞑想アプリ
```

---

## 5. タグの組み合わせ例

### SaaS LP
```yaml
color: minimal-light
type: sans-modern
ui: soft-shadow
mood: professional
```

### D2Cコスメ
```yaml
color: pastel
type: editorial-serif
ui: minimal-line
mood: premium
```

### テックスタートアップ
```yaml
color: dark-tech
type: mono-tech
ui: glassmorphism
mood: innovative
```

### 地方クリニック
```yaml
color: medical-clean
type: sans-modern
ui: flat
mood: friendly
```

### 和食レストラン
```yaml
color: japanese-traditional
type: editorial-serif
ui: minimal-line
mood: premium
```

### ゲームアプリ
```yaml
color: neon
type: bold-impact
ui: brutalism
mood: playful
```

---

## 6. タグ選択のガイド

### ターゲット × ムード
| ターゲット | 推奨mood |
|-----------|----------|
| BtoB | professional, serious |
| 若年層 | playful, innovative |
| 高所得層 | premium, calm |
| ファミリー | friendly, playful |
| 高齢者 | calm, friendly |

### 業界 × カラー
| 業界 | 推奨color |
|------|----------|
| IT/テック | dark-tech, minimal-light |
| 金融/保険 | corporate-blue |
| 医療 | medical-clean |
| 飲食 | warm-beige, vibrant-pop |
| 美容 | pastel, dark-luxury |
| 不動産 | minimal-light, corporate-blue |
