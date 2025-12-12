# ウェブサイト構築GPTシステム v7.1

## 概要

3つのGPTを連携させて、高品質なウェブサイトを構築するシステムです。

```
GPT-A（要件定義）→ 要件定義JSON
     ↓
GPT-B（デザイン設計）→ デザイン設計書JSON
     ↓
GPT-C（実装）→ HTML/CSS/JS
```

---

## v7.1 の特徴

### 1. デザイン方向性を引き出す質問（Phase 0 改善）

従来の抽象的な質問から、デザインに直結する具体的な質問に変更：
- お客さんの気持ち → サイトの「温度感」
- 競合サイトの印象 → 差別化ポイント
- 好きなサイト → スタイルの方向性
- 絶対NG → 地雷回避
- 第一印象の狙い → ゴール設定
- **流入経路（LPのみ）** → 媒体別最適化

### 2. 構造化されたデザインナレッジ

```
Layer 0: 必須基盤（モバイルファースト、アクセシビリティ）
Layer 1: 構造パターン（LP/ポートフォリオ/コーポレート）
Layer 2: セクションパターン（ヒーロー、About、サービス等）
Layer 3: スタイルタグ（color/type/ui/mood）
Layer 4: 世界観プリセット（10種類の完成形）
```

### 3. 2025年トレンド対応

- **3カラムレイアウト（PC）**: スクロール量削減、情報伝達効率最大化
- **スワイプ型LP（スマホ）**: ストーリーズ風の没入体験
- **記事LP/ノン広告風**: 広告疲れ対策
- **シームレスCTA**: LINE連携、カレンダー予約埋め込み
- **UGC動画活用**: 信頼性とリアリティの担保
- **媒体別最適化**: 流入元に応じた構造・CTA選択

### 4. 組み合わせで多様性を実現

```
構造: LP-B（教育→納得型）
ヒーロー: HERO-01（左テキスト×右ビジュアル）
スタイル: STYLE-03（プレミアム・ミニマル）
CTA: CTA-05（カレンダー予約）
```
↓ これだけで一貫したデザインが決まる

---

## ファイル構成

```
WEBSITE_GPT_v7_3SPLIT_NEW/
├── GPT-A_REQUIREMENTS.md      # 要件定義エンジン（プロンプト）
├── GPT-B_DESIGN.md            # デザイン設計エンジン（プロンプト）
├── GPT-C_IMPLEMENTATION.md    # 実装エンジン（プロンプト）
├── KNOWLEDGE_DESIGN/          # GPT-B用ナレッジ（5ファイル）
│   ├── B00_DESIGN_FUNDAMENTALS.md  # デザインの基盤と原則
│   ├── B01_STRUCTURE_PATTERNS.md   # 構造パターン（LP/ポートフォリオ等）
│   ├── B02_SECTION_PATTERNS.md     # セクションパターン（ヒーロー等）
│   ├── B03_STYLE_TAGS.md           # スタイルタグ（color/type/ui/mood）
│   └── B04_STYLE_PRESETS.md        # 世界観スタイル10種類
└── KNOWLEDGE/                 # GPT-C用ナレッジ（7ファイル）
    ├── B16_SECURITY_BASICS.md      # セキュリティ基礎
    ├── B17_FORM_SECURITY.md        # フォームセキュリティ
    ├── B18_PRIVACY_COMPLIANCE.md   # プライバシー対応
    ├── B19_ACCESSIBILITY.md        # アクセシビリティ
    ├── B20_MAINTAINABILITY.md      # 保守性
    ├── B21_CLIENT_WORK_STANDARDS.md # クライアントワーク品質
    └── B22_CODE_TEMPLATE.md        # コードテンプレート
```

---

## GPTs 設定方法

### GPT-A: 要件定義エンジン

1. **Name**: ウェブサイト要件定義エンジン
2. **Instructions**: `GPT-A_REQUIREMENTS.md`の内容をコピペ
3. **Knowledge**: なし（対話特化）

### GPT-B: デザイン設計エンジン

1. **Name**: ウェブサイトデザイン設計エンジン
2. **Instructions**: `GPT-B_DESIGN.md`の内容をコピペ
3. **Knowledge**: `KNOWLEDGE_DESIGN/`内の5ファイルをアップロード
   - B00_DESIGN_FUNDAMENTALS.md
   - B01_STRUCTURE_PATTERNS.md
   - B02_SECTION_PATTERNS.md
   - B03_STYLE_TAGS.md
   - B04_STYLE_PRESETS.md

### GPT-C: 実装エンジン

1. **Name**: ウェブサイト実装エンジン
2. **Instructions**: `GPT-C_IMPLEMENTATION.md`の内容をコピペ
3. **Knowledge**: `KNOWLEDGE/`内の7ファイルをアップロード
   - B16_SECURITY_BASICS.md
   - B17_FORM_SECURITY.md
   - B18_PRIVACY_COMPLIANCE.md
   - B19_ACCESSIBILITY.md
   - B20_MAINTAINABILITY.md
   - B21_CLIENT_WORK_STANDARDS.md
   - B22_CODE_TEMPLATE.md

---

## 使用フロー

### Step 1: GPT-A で要件定義

```
ユーザー: サイトを作りたい
GPT-A: Phase 0の質問（5問）
      → Phase 1-5の質問
      → 要件定義JSON出力
```

### Step 2: GPT-B でデザイン設計

```
ユーザー: 要件定義JSONを貼り付け
GPT-B: ナレッジ参照
      → 構造・セクション・スタイル選択
      → デザイン設計書JSON出力
```

### Step 3: GPT-C で実装

```
ユーザー: デザイン設計書JSONを貼り付け
GPT-C: ナレッジ参照
      → HTML/CSS/JS生成
      → 品質チェック済みコード出力
```

---

## ナレッジサイズ

| GPT | ナレッジ数 | 合計サイズ |
|-----|----------|-----------|
| GPT-A | 0 | 0KB |
| GPT-B | 5 | 約40KB |
| GPT-C | 7 | 約75KB |

---

## スタイルプリセット一覧

1. **モダン・エディトリアル** - 雑誌風、洗練、知的
2. **ポップ・デジタル** - SNS世代、親しみやすい、カラフル
3. **プレミアム・ミニマル** - 高級感、静寂、余白重視
4. **テック・フューチャー** - 先進的、ダークテーマ、グラデーション
5. **和モダン・ジャポニズム** - 伝統と現代、優雅、金箔
6. **スポーツ・エナジー** - 疾走感、力強い、斜めカット
7. **ナチュラル・オーガニック** - 自然、健康、サステナブル
8. **クラシック・ポップアート** - 彫刻×ポップ、大胆、ユーモア
9. **マテリアル・アーキテクチャ** - 素材感、建築的、3D感
10. **ワンライン・アート** - 一筆書き、優雅、大人シック

---

## 更新履歴

- **v7.1**: Phase 0質問改善、デザインナレッジ再構築
- **v7.0**: 3分割版、セキュリティ・アクセシビリティ・保守性ナレッジ追加
