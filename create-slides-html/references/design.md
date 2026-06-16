# Design System Inspired by Howard

Howard.pptx（16:9・26.66″×15″の大判、全8枚）から抽出したデザインシステム。スレートブルーを主役にした、落ち着いたコーポレート系。グラデーションは使わず、ベタ塗りの色面と全大文字の太字見出しで構成する。表紙はスレートブルー一色＋白文字、本文スライドは白背景＋スレートの見出し、という2系統。

## 1. Visual Theme & Atmosphere

白を基調に、スレートブルー `#3D698F` を主役に据えた端正なデザイン。差し色はミュートなセージ系（ティールグレー `#80A7A5`、セージグリーン `#ABBE92`）で、彩度を抑えた落ち着いたトーン。見出しは Arial Bold の全大文字、本文は Lato Light の細身サンセリフで、太さのコントラストで階層を作る。見出しと本文がともにサンセリフな点が特徴（セリフは使わない）。グラデーション・影・角丸を多用せず、ベタ塗りの矩形と広い余白で構成するフラット寄りの設計。

**Key Characteristics:**
- 主役はスレートブルー `#3D698F`、差し色はティールグレー `#80A7A5` とセージグリーン `#ABBE92`
- 見出しは Arial Bold の全大文字（ALL CAPS）、本文は Lato Light
- グラデーションを使わず、ベタ塗りの色面で構造を見せる
- 本文・見出しの標準色は純黒ではなくスレート `#44546A`
- 表紙はスレートブルー一色＋白の大見出し（150pt）
- 統計・数値は Arial Bold のスレート色（48–66pt）
- カード/サーフェスはライトグレー `#F2F2F2`

## 2. Color Palette & Roles

### Primary — ブランド主色
- Slate Blue — `#3D698F`（RGB 61,105,143）：表紙背景、主要色面、見出しアクセント、強調円

### Accent Colors
- Teal Grey — `#80A7A5`（RGB 128,167,165）：2番手の色面・カード
- Sage Green — `#ABBE92`（RGB 171,190,146）：3番手の色面・カード
- Mauve — `#775864`（RGB 119,88,100）：限定的な濃色アクセント
- Plum — `#320C2D`（RGB 50,12,45）：ごく限定的な最濃アクセント

### Dark & Neutral — ダーク・ニュートラル系
- Ink / Headline — `#44546A`（RGB 68,84,106）：見出し・本文・統計の標準色（純黒ではないスレート）
- Mid Grey — `#999999`（RGB 153,153,153）：補助テキスト・キャプション

### Interactive — リンク・ホバー等
元資料に明確な定義なし。拡張時は Slate Blue `#3D698F` をリンク色、Teal Grey `#80A7A5` をホバー色として推奨。

### Surface & Borders
- Surface（白）— `#FFFFFF`（RGB 255,255,255）：本文スライド背景
- Surface（淡灰）— `#F2F2F2`（RGB 242,242,242）：カード・図表のブロック背景
- Border — ほぼ未使用。必要時はミッドグレー `#999999` の極細線

### Shadow Colors
基本は使用しない。統計カードなど一部で淡い影が見られる程度。原則フラット。

### Status — Before/After 等
明示的なステータス配色はなし。良し悪しは Slate Blue（強調）と Sage Green（中庸）で表現。

### Gradient
使用しない。背景・色面はすべてベタ塗り。

## 3. Typography Rules

### Font Family
- Headline（見出し・タイトル・数値）：`Arial`（Bold・全大文字）。フォールバック `Arial, 'Helvetica Neue', '游ゴシック', sans-serif`
- Body / Lead（本文・リード）：`Lato`（Light）。フォールバック `'Lato', '游ゴシック', sans-serif`
- Accent（小見出しに一部）：`Poppins SemiBold`（任意。なければ Arial Bold で代替）
- ※theme 既定は Calibri / 游ゴシックだが、実際の描画は上記 Arial / Lato

### Hierarchy

| Level | Size | Weight | Font | Color | Use |
|-------|------|--------|------|-------|-----|
| Cover Title | 150pt | Bold | Arial（全大文字） | `#FFFFFF` | 表紙の社名・大見出し |
| Section Title | 66pt | Bold | Arial（全大文字） | `#44546A` | 各スライドの見出し |
| Big Letter / Stat | 80 / 66pt | Bold | Arial | `#FFFFFF` / `#44546A` | 図解の記号・大きな統計 |
| Name / Sub-head | 48pt | Bold | Arial | `#44546A` | 人名・サブ見出し |
| Callout | 40pt | Bold | Arial | `#44546A` | 図表内の小見出し |
| Body / Lead | 28pt | Light | Lato Light | `#44546A` / `#999999` | 本文・説明 |
| Card Title | ~24pt | Bold | Arial | `#FFFFFF`(色面) / `#44546A`(白地) | カード内タイトル |

### Principles
- 見出しは Arial Bold の全大文字、本文は Lato Light で太さのコントラストをつける
- 本文色は純黒ではなくスレート `#44546A`
- 太字（見出し）と細字（本文）の対比で階層を作る。セリフ体は使わない
- 数値は Arial Bold、色は色面上なら白、白地ならスレート

## 4. Component Stylings

### Cover / Title Slide
スレートブルー `#3D698F` 一色の背景。社名は150pt Arial Bold 全大文字の白。複数行に分けて大きく置く。

### Section Header / Watermark
顕著なウォーターマーク文字は未検出。各スライド上部に66pt Arial Bold 全大文字のセクションタイトルを置く。

### Tag Label / Headline / Lead / Body
- Section Title：66pt Arial Bold 全大文字、`#44546A`
- Lead/Body：28pt Lato Light、`#44546A`（主）/ `#999999`（補助）

### Image Block
製品スライドで配置。角丸なし、枠線なし。

### Stat / Number Card
白またはライトグレー `#F2F2F2` のカードに、48–66pt Arial Bold スレート `#44546A` の数値（70%, 250K 等）。色面に置く場合は白文字。

### Color Block / Step Card
Slate Blue `#3D698F` / Teal Grey `#80A7A5` / Sage Green `#ABBE92` の3色をベタ塗りで横並び。各ブロックに白の Arial Bold タイトルと Lato Light の説明。

### Diagram Letter
円または矩形（`#3D698F` や `#F2F2F2`）に80pt Arial Bold の記号（A/B 等）。色面上は白、淡灰上はスレート。

### Buttons / Forms
元資料に定義なし。拡張時は Slate Blue を主ボタン、Teal Grey をホバー、角丸4px・影なしを推奨。

## 5. Layout Principles

### Spacing System
広い余白。スライド四辺に大きめのマージン、テキストブロック間は本文サイズの1.5倍以上。

### Grid & Container
16:9（大判 26.66″×15″）。本文スライドは「66pt 全大文字見出し → 28pt リード → 色面カード/統計」の縦積み。複数要素は3カラムの均等グリッドが基本。

### Whitespace Philosophy
ベタ塗りの色面と白の対比で構造を見せる。詰め込まず、1スライド1メッセージ。

### Border Radius Scale
角丸はほぼ未使用（0）。図解の円バッジを除き矩形は直角。

## 6. Depth & Elevation
原則フラット。統計カードに淡い影が付く程度で、強い立体表現は避ける。

| Level | Shadow |
|-------|--------|
| Base | none |
| Card | 任意で淡い影（`0 2px 8px rgba(68,84,106,0.08)`）まで |

## 7. Do's and Don'ts

### Do
- 表紙はスレートブルー `#3D698F` 一色＋白の大見出し
- 見出しは Arial Bold の全大文字、本文は Lato Light
- 差し色はスレートブルー＋セージ系（ティールグレー・セージグリーン）に集約
- 統計はベタ塗りカードに Arial Bold の数値
- 本文色はスレート `#44546A`、補助は `#999999`
- 余白を広く取り、1スライド1メッセージ

### Don't
- グラデーション背景を使わない（ベタ塗りで構成）
- セリフ体の見出しを使わない（Arial Bold で統一）
- 多色を散らさない（ブルー＋セージ系に限定）
- 強い影・ベベル・3D効果を使わない
- 純黒 `#000000` をベタ塗りで多用しない
- 見出しを小文字にしない（セクション見出しは全大文字）

## 8. Responsive Behavior

### Breakpoints
固定アスペクト16:9。Web化時は幅1280pxを基準にスケール。

### Touch Targets
インタラクティブ要素は最低44px。

### Collapsing Strategy
3カラムは狭幅で縦積みに。色面の塗りは維持。

### Image Behavior
画像は cover フィット、角丸・枠なしを維持。

## 9. Agent Prompt Guide

### Quick Color Reference
- 表紙背景／主要色面：Slate Blue `#3D698F`
- 差し色面：Teal Grey `#80A7A5` / Sage Green `#ABBE92`
- 見出し/本文インク：`#44546A`
- 補助テキスト：`#999999`
- カード淡灰：`#F2F2F2`
- 背景：`#FFFFFF`

### Example Component Prompts
1. 「表紙：スレートブルー `#3D698F` 一色の背景。中央に150pt Arial Bold 全大文字の白い社名を2行で」
2. 「本文スライド：白背景。上部に66pt Arial Bold 全大文字 `#44546A` のセクションタイトル、28pt Lato Light `#44546A` のリード」
3. 「3カラム：Slate Blue / Teal Grey / Sage Green の3色面を均等配置、各面に白の Arial Bold タイトルと Lato Light の説明」
4. 「統計カード：ライトグレー `#F2F2F2` のカードに48pt Arial Bold `#44546A` の数値（例 70%）、下に28pt Lato Light グレーのラベル」
5. 「図解：`#3D698F` の円に80pt Arial Bold 白の記号（A/B）、横に40pt Arial Bold の小見出しと Lato Light の説明」

### Iteration Guide
1. まず表紙（スレートブルー一色）と本文（白地）の2系統を作り分ける
2. 見出し=Arial Bold 全大文字、本文=Lato Light を必ず守る（セリフ体にしない）
3. 差し色はスレートブルー＋セージ系に限定し、マウブ `#775864`・プラム `#320C2D` は最小限
4. グラデーション・強い影・角丸を足したくなったら、ベタ塗り色面と余白で差をつける
5. 本文色は `#44546A`、補助は `#999999`。純黒は避ける
                                                                                                                                                                                                                                                                                                                                                   