# Design System — Mandalart App LP

> 実測元: 本LP（`docs/second-seminar/sample/`）の `index.html` / `style.css`
> 抽出方法: `style.css` 内の CSS カスタムプロパティ（`:root`）と各コンポーネント定義からの直接抽出
> 本ドキュメントの数値はすべて上記CSSからの実測値です（推測値はセクション9の推奨欄のみ）。
> 備考: `style.css` 冒頭コメントでは "Kakeai-inspired" を名乗るが、本書の値は外部サイトではなく本LP自体から実測したもの。

---

## 1. Visual Theme & Atmosphere

Mandalart App の LP は、**真っ白な背景（`#fff`）にスレートブルー `#5363B4` を主役色として据えた、やわらかく親しみやすい SaaS プロダクトのランディングページ**である。本文色には純黒を避けた濃紺 `#141432` を使い、淡いパステル6系統（青・赤・黄・ミント・ピンク・紫）を機能カードやカテゴリタグの**識別色**として効かせる。任天堂サイトのような「一点アクセント」ではなく、**パステルのマルチアクセントで機能の多さ・楽しさを表現**するのが特徴。

角丸はカードで `1.25rem`（20px）とやや大きめにとり、影は**ブランドカラー由来の淡くにじむソフトシャドウ** `0 0 1.5rem rgba(56,56,116,0.1)` を基調にして、輪郭をきつくせず柔らかい立体感を出す。Hero と CTA セクションには `135deg` の斜めグラデーション（Hero は淡青→白→淡黄、CTA は濃紫→青→明青）を使い、軽やかな奥行きとブランド感を添える。

タイポグラフィは和文に `Noto Sans JP`、英字・数字に `Manrope` を割り当て、見出しは `font-weight:700`、本文は `line-height:1.6`＋`letter-spacing:0.03em` でゆったり整える。数字・ラベル・ロゴ・セクションラベルには `Manrope` を当て、英字の軽快さでアクセントにする。

レイアウトは PC で最大 `1100px`（`--container`）のセンタリンググリッド。セクションは縦 `5rem` パディングで区切り、機能は2カラム、お悩み・利用シーン・料金は3カラムのカードで整列する。「清潔・親しみ・整理された SaaS」の印象を、白地・濃紺テキスト・スレートブルー＋パステルで成立させるデザインシステムである。

**Key Characteristics:**
- **白地 × スレートブルー主役 × パステル6色のマルチアクセント** — 背景 `#fff`、本文 `#141432`、主役 `#5363B4`、カテゴリ識別に淡青/淡赤/淡黄/ミント/ピンク/紫
- **純黒を使わない濃紺テキスト** — 本文は `#141432`、補助 `#626276`、淡い補助 `#9393A1`
- **やや大きめの角丸** — カードは `1.25rem`（20px）、ボタン・小要素は `0.5rem`（8px）、タグは `0.25rem`（4px）、バッジ/ピルは `100px`
- **ブランドカラー由来のソフトシャドウ** — `rgba(56,56,116,0.1)` のにじむ影でカードを浮かせる。輪郭は淡い罫線 `#E8E8EB`
- **斜めグラデーションの差し色** — Hero と CTA に `linear-gradient(135deg, …)` を使う
- **和文 Noto Sans JP × 英字 Manrope** — 数字・ラベル・ロゴは Manrope で軽快に
- **2〜3カラムのカードグリッド** — `repeat(2,1fr)` / `repeat(3,1fr)` を中心に整列

---

## 2. Color Palette & Roles

### Primary — メインブランドカラー
| HEX | 変数 | 用途 |
|---|---|---|
| `#5363B4` | `--primary` | ブランドの核。セクションラベル、Hero タイトルのアクセント、リスト点、フォーカス枠、バッジ、料金おすすめ枠 |
| `#383874` | `--primary-dark` | 濃いバリエーション。バッジ文字色、CTA グラデーション始点、ブランド影の色源 `rgba(56,56,116,…)` |
| `#758BFD` | `--primary-bright` | 明るいバリエーション。CTA グラデーション終点 |

### Accent Colors — アクセントカラー（カテゴリ／機能別の識別色）
| HEX | 変数 | 想定用途 |
|---|---|---|
| `#C0CAFF` | `--pale-blue` | 淡青。Hero バッジ背景、機能タグ（目標管理）背景、利用シーンカード上罫 |
| `#A3B1FE` | `--deep-blue` | 濃いめの青アクセント |
| `#FEA5A5` | `--pale-red` | 淡赤。機能タグ（経歴管理）背景 |
| `#FD7C7C` | `--deep-red` | 濃赤。フォーム必須ラベル、お悩みアイコン文字 |
| `#FFE88F` | `--pale-yellow` | 淡黄。機能タグ（タスク管理）背景、利用シーンカード上罫（2番目） |
| `#FFDE5C` | `--deep-yellow` | 濃黄アクセント |
| `#A4E6C7` | `--pale-mint` | 淡ミント。機能タグ（1on1）背景、ハイライトのチェックアイコン、利用シーンカード上罫（3番目） |
| `#7BDBAD` | `--deep-mint` | 濃ミント。料金チェックマーク `✓` |
| `#FFB0D6` | `--pale-pink` | 淡ピンク。機能タグ（お知らせ）背景 |
| `#E8B5FB` | （ハードコード） | 淡紫。機能タグ（分析）背景 |

> タグ文字色は背景パステルに対する濃色をハードコード: 赤 `#c44` / 黄 `#8a6d00` / ミント `#1a7a4a` / ピンク `#a83279` / 紫 `#6b2fa0`。お悩みアイコンの黄は文字 `#d4a017`・背景 `#fef2f2`(赤)。

### Dark & Neutral — ダーク・ニュートラル系
| HEX | 変数 | 用途 |
|---|---|---|
| `#141432` | `--text` | 本文標準テキスト（純黒ではない濃紺）。ヘッダーロゴ、ボタン地色、フッター地色 |
| `#626276` | `--text-sub` | 補助テキスト（説明文・リード） |
| `#9393A1` | `--text-muted` | さらに淡い補助（キャプション・注記・コピーライト） |
| `#B6B6C0` | （ハードコード） | フッターリンク文字 |

### Interactive — インタラクティブ要素の色
| 状態 | 値 |
|---|---|
| ナビリンク hover | 下線（`border-bottom-color:#626276`）が現れる |
| 主ボタン hover | 地色と文字色を反転（`#141432` ⇄ `#fff`） |
| カード hover | 枠を `--primary` に、影を `--shadow-brand` に。一部は `translateY(-4px)` で浮上 |
| フォーカス | 入力枠が `--primary`（`#5363B4`）に変化 |
| 矢印アイコン hover | `transform: translateX(3px)` |

### Surface & Borders — 背景・ボーダー
| HEX | 変数 | 用途 |
|---|---|---|
| `#FFFFFF` | `--white` | 基本背景・カード地 |
| `#F9F9F9` | `--grey-98` | セクション帯（`.section--grey`） |
| `#F2F2F2` | `--grey-95` | 画像プレースホルダ地、沈んだ面 |
| `#E8E8EB` | `--grey-92` | 標準ボーダー（カード枠・区切り線・タブ枠・FAQ枠） |
| `#EBEBEB` | `--grey-border` | フォーム入力の枠 |
| `#F1F3FF` | `--pale-blue-bg` | 淡青背景（`.section--blue`、Hero グラデ始点、連絡先アイコン地） |
| `#FFFAE7` | `--warm-yellow-bg` | 淡黄背景（Hero グラデ終点、お悩み黄アイコン地） |
| `#2A2A4A` | （ハードコード） | フッター内の区切り罫 |

### Shadow Colors — シャドウに使用する色
| 変数 | 値 | 用途 |
|---|---|---|
| `--shadow-light` | `0 0 4px rgba(0,0,0,0.1)` | ヘッダー（スクロール時）、機能カード、フォーム、ハイライト画像の軽い影 |
| `--shadow-brand` | `0 0 1.5rem rgba(56,56,116,0.1)` | カード hover・利用シーンカード・料金おすすめ・Hero 画像/フロートカードの**ブランド色のにじむ影** |
| `--shadow-strong` | `0 0 12px rgba(0,0,0,0.25)` | 最も強い影（予備） |

### Status — ステータス色
| HEX | 用途 |
|---|---|
| `#FD7C7C`（`--deep-red`） | フォーム必須ラベル `必須` |
| `#7BDBAD`（`--deep-mint`） | 料金表のチェックマーク `✓`（肯定・含む） |

---

## 3. Typography Rules

### Font Family
```css
/* 和文ベース（本文・UI） */
font-family: 'Noto Sans JP', 'Hiragino Kaku Gothic ProN', sans-serif;

/* 英字・数字・ラベル・ロゴ（.font-en） */
font-family: 'Manrope', sans-serif;
```
- 読み込み: Google Fonts `Manrope`（400/500/600/700）＋ `Noto Sans JP`（300/400/500/600/700）
- `Noto Sans JP` = 本文・ボタン・FAQ・フォームなど全UIの標準
- `Manrope` = ロゴ、セクションラベル、統計数字、料金、`PRODUCT`/`FOR HR` 等の英字ラベルに当てて軽快さを出す
- ベース: `color:#141432`、`line-height:1.6`、`letter-spacing:0.03em`、`-webkit-font-smoothing:antialiased`

### Hierarchy
| 用途 | font-size | weight | font | color | letter-spacing |
|---|---|---|---|---|---|
| Hero タイトル | `2.5rem`（40px） | 700 | Noto Sans JP | `#141432`（accent語のみ `#5363B4`） | `0.01em` |
| 料金 価格・統計数字 | `2.5rem` / `1.75rem` | 700 | Manrope | `#141432` / `#5363B4` | — |
| CTA タイトル | `2rem` | 700 | Noto Sans JP | `#fff` | — |
| セクションタイトル | `1.75rem`（28px）／SP `1.375rem` | 700 | Noto Sans JP | `#141432` | `0.02em` |
| 機能ハイライト／連絡先見出し | `1.5rem` | 700 | Noto Sans JP | `#141432` | — |
| 機能カードタイトル | `1.125rem` | 600 | Noto Sans JP | `#141432` | — |
| 利用シーン／お悩みタイトル | `1.0625rem` / `1rem` | 600 | Noto Sans JP | `#141432` | — |
| 本文（リード） | `1rem`〜`1.0625rem` | 400 | Noto Sans JP | `#626276` | — |
| 本文（密・カード内） | `0.875rem` | 400 | Noto Sans JP | `#626276` | — |
| セクションラベル | `0.75rem` | 600 | Manrope | `#5363B4` | `0.1em`＋`uppercase` |
| キャプション・注記 | `0.75rem`〜`0.8125rem` | 400–500 | Noto Sans JP | `#9393A1` | — |

### Principles
- **見出しは `700`、小見出しは `600`**: `h1`〜`h4` は `font-weight:700; line-height:1.3`。カードタイトル類は `600`
- **本文は純黒を使わず濃紺/グレー**: `#141432` →補助 `#626276` →淡 `#9393A1` の3段で濃度を落とす
- **行間は用途で切替**: 見出し `1.25`〜`1.3`、UI/本文 `1.6`〜`1.7`、読み物（Hero 説明・FAQ 回答）`1.8`
- **英字・数字は Manrope**: ラベルは `letter-spacing:0.1em`＋`uppercase` で記号的に、数字は大きく `700` で躍動感
- **字間は控えめ**: 標準 `0.03em`、ボタン `0.04em`、ラベル `0.05`〜`0.1em`

---

## 4. Component Stylings

### Buttons（`.btn`）
**ベース（共通）:**
```css
.btn {
  display: inline-flex; align-items: center; justify-content: center;
  gap: 0.5rem;
  border: 1px solid var(--text);          /* #141432 */
  border-radius: var(--radius-base);       /* 0.5rem = 8px */
  font-family: 'Noto Sans JP', sans-serif;
  font-size: 1rem; font-weight: 500; letter-spacing: 0.04em;
  transition: color 0.15s ease, background-color 0.15s ease;
}
```
**バリアント:**
| クラス | 仕様 |
|---|---|
| `--primary` | `background:#141432; color:#fff; padding:0.875rem 2rem;` → hover で `background:#fff; color:#141432;`（反転） |
| `--outline` | `background:#fff; color:#141432;` → hover で `background:#141432; color:#fff;`（反転） |
| `--white` | `border:#fff; background:#fff; color:#141432;` → hover で `background:transparent; color:#fff;`（CTA/濃地で使用） |
| `--small` | `padding:0.625rem 1.25rem; font-size:0.875rem;`（ヘッダーCTA） |

- 内包矢印（`.btn-arrow`）は `0.75rem` 角の SVG。`.btn:hover` で `translateX(3px)`

### Cards & Containers
- **角丸**: 機能カード/利用シーン/料金/フォーム/Hero画像 は `1.25rem`（20px、`--radius-xl`）。お悩みカード/FAQ/ボタン は `0.5rem`（8px、`--radius-base`）
- **影**: 標準は `--shadow-light`、hover で `--shadow-brand`（ブランド色のにじみ）。利用シーン・料金おすすめは初期から `--shadow-brand`
- **罫線**: 多くは `1px solid #E8E8EB`（`--grey-92`）。hover で `--primary` に変わる
- **グリッド**: お悩み/利用シーン/料金 `repeat(3,1fr)`、機能 `repeat(2,1fr)`、フッター `1.5fr repeat(3,1fr)`、ギャップ `1.5rem`〜`2rem`

**機能カード（`.feature-card`）:** 上部に `aspect-ratio:16/9` の画像、本文 `padding:1.5rem`。先頭に丸タグ＋タイトル＋説明＋`•`付きリスト（点は `5px` 円・`--primary`）。hover で影と画像 `scale(1.03)`。

**利用シーンカード（`.usecase-card`）:** `border-top:3px solid` を `nth-child` で淡青→淡黄→淡ミントに変える。hover で `translateY(-4px)`。末尾に左罫線付きの引用（`italic`・`#9393A1`）。

**料金カード（`.pricing-card`）:** `padding:2.5rem 2rem`、中央寄せ。`--featured` は枠 `--primary`＋ブランド影、上に `おすすめ` ピルバッジ（`#5363B4` 地・白字）。特徴は `✓`（`#7BDBAD`）付きの左寄せリスト。

### Tags / Badges
- **機能タグ（`.feature-card__tag`）:** `font-size:0.625rem; font-weight:600; letter-spacing:0.05em; padding:0.125rem 0.5rem; border-radius:0.25rem;` カテゴリ別にパステル地＋濃色文字（§2参照）
- **ハイライトタグ（`.feature-highlight__tag`）:** `border-radius:100px`（ピル）、淡青/淡ミント地
- **Hero バッジ（`.hero__badge`）:** ピル形、淡青地・`#383874` 文字、先頭に `6px` の `--primary` ドット
- **料金バッジ:** ピル形、`--primary` 地・白字、カード上辺に重ねて配置

### Navigation（`.header`）
- `position:fixed`、`height:4rem`、半透明白 `rgba(255,255,255,0.96)`＋`backdrop-filter:blur(8px)`、下罫 `#E8E8EB`。スクロールで `--shadow-light` を付与
- ロゴは Manrope `700` `1.25rem`＋画像。ナビリンクは `0.8125rem` `500`、hover で下線が現れる
- `991px` 以下でナビ/CTA を隠しハンバーガー（3本線 `22×2px`）に切替

### Forms（`.contact__form`）
- カード化: 白地・`1px #E8E8EB` 枠・角丸 `1.25rem`・`--shadow-light`・`padding:2rem`
- 入力（`.form-input/select/textarea`）: 枠 `1px #EBEBEB`、角丸 `0.5rem`、`padding:0.625rem 0.875rem`、`font-size:0.875rem`。フォーカスで枠 `--primary`
- ラベルは `0.8125rem` `600`、必須は `#FD7C7C` の小バッジ

### Headings（セクション見出しの定型）
中央寄せの3点セット: ① セクションラベル（Manrope・`#5363B4`・`uppercase`・`0.1em`）→ ② セクションタイトル（`1.75rem` `700`）→ ③ サブタイトル（`1rem`・`#626276`・`max-width:640px`・下 `3rem`）。

---

## 5. Layout Principles

### Spacing System
`rem`（16px 基準）の倍数体系。代表値:
- セクション縦パディング: `5rem`（80px）／SP `3.5rem`（56px）
- カード内パディング: `1.5rem`（カード本文）/ `2rem 1.5rem`（お悩み・利用シーン）/ `2.5rem 2rem`（料金）
- コンテナ横パディング: `1.5rem`（SP `1rem`）
- グリッドギャップ: `1.5rem` / `2rem` / `3rem`
- 要素間: `0.5rem` / `0.75rem` / `1rem` / `1.25rem`

### Grid & Container
| 役割 | 幅 |
|---|---|
| 最大コンテナ | `--container: 1100px` |
| 料金/連絡先の内側 | `960px` |
| 画面ショット表示 | `900px` |
| FAQ リスト | `720px` |
| サブタイトル行長 | `640px` |
- グリッド: お悩み/利用シーン/料金 `repeat(3,1fr)`、機能 `repeat(2,1fr)`、Hero/ハイライト `1fr 1fr`、連絡先 `1fr 1.2fr`、フッター `1.5fr repeat(3,1fr)`

### Whitespace Philosophy
- **余白と淡い影で区切る**: セクションは大きな縦パディング、面差は `--grey-98`/`--grey-95` の薄帯、カードはにじむ影で分離
- **中央寄せ基調**: `1100px`/`960px`/`720px` をセンタリングし、見出しブロックは中央揃え
- **読み物はゆったり**: Hero 説明・FAQ 回答は `line-height:1.8`、UI は `1.6`〜`1.7`

### Border Radius Scale
| 値 | 変数 | 用途 |
|---|---|---|
| `0.25rem`（4px） | `--radius-sm` | 機能タグ、ロゴ画像 |
| `0.5rem`（8px） | `--radius-base` | ボタン、お悩みカード、FAQ、入力、Hero フロートカード |
| `0.625rem`（10px） | `--radius-lg` | 予備 |
| `1.25rem`（20px） | `--radius-xl` | 機能カード、利用シーン、料金、フォーム、各画像枠 |
| `1.5rem`（24px） | `--radius-2xl` | 予備 |
| `100px` | — | バッジ・ピル・タグ（ハイライト） |
| `50%` | — | 円形アイコン・ドット |

---

## 6. Depth & Elevation

任天堂サイトのような「ほぼ影なし」ではなく、Mandalart LP は**淡いソフトシャドウを積極活用**する。ただし影は黒の強い影ではなく、ブランド色由来 `rgba(56,56,116,0.1)` の**にじむ淡影**が中心で、輪郭をきつくしない。

| Level | Treatment | Use |
|---|---|---|
| 0（基準面） | 影なし、`#fff` 背景＋`#E8E8EB` 罫線 | ページ標準。フォーム入力・FAQ・お悩みカードの初期状態 |
| 1（薄面） | `#F9F9F9` / `#F2F2F2` 背景 | セクション帯（`--grey`）、画像プレースホルダ |
| 2（軽い浮き） | `--shadow-light`：`0 0 4px rgba(0,0,0,0.1)` | 機能カード・フォーム・ハイライト画像・スクロール時ヘッダー |
| 3（ブランド浮き） | `--shadow-brand`：`0 0 1.5rem rgba(56,56,116,0.1)` | カード hover、利用シーン、料金おすすめ、Hero 画像・フロートカード |
| 4（強い浮き） | `--shadow-strong`：`0 0 12px rgba(0,0,0,0.25)` | 予備（現状未使用に近い最強影） |
| 特殊（グラデ面） | `linear-gradient(135deg, …)` | Hero（淡青→白→淡黄）・CTA（濃紫→青→明青）の奥行き演出 |

**Shadow Philosophy:** 影は「浮かせて触れる感」を出すために使うが、色は黒よりブランド紫を優先し、`blur` を広く `spread` を持たせて柔らかく。hover で影を一段濃く（light → brand）して反応を示す。

---

## 7. Do's and Don'ts

### Do
- **白地 `#fff`・濃紺テキスト `#141432`・主役 `#5363B4`** の骨格を保つ
- **カテゴリ識別はパステル6色**（淡青/淡赤/淡黄/ミント/ピンク/紫）を、機能タグ・カード上罫など**識別の範囲**で使う
- **角丸はカード `1.25rem`／ボタン・小要素 `0.5rem`／タグ `0.25rem`／ピル `100px`** を守る
- **影はブランド色のにじみ**（`--shadow-brand`）を基調に、hover で一段濃くする
- **見出しは `700`、小見出し `600`**、本文は `#626276`、淡補助は `#9393A1`
- **数字・英字ラベル・ロゴは Manrope**、和文は Noto Sans JP
- **Hero/CTA のグラデーションは `135deg`**、それ以外の面はフラットに保つ
- **コンテナは `1100px`**（料金/連絡先は `960px`）にセンタリング

### Don't
- ❌ 本文に純黒 `#000` を多用しない（`#141432` を使う）
- ❌ パステルを広い面のベタ塗りに使わない（識別用。広面は白/薄グレー/グラデに限定）
- ❌ 角を立てすぎない（`2px` 等のシャープな角は世界観に合わない。最低でも `0.25rem`）
- ❌ 黒の強い影で立体を作らない（影はブランド色のにじみを優先）
- ❌ 主役色 `#5363B4` を本文テキスト全般に広げない（アクセント・ラベル・リンク的用途に留める）
- ❌ グラデーションを多用しない（Hero と CTA の2か所に限定）
- ❌ 見出しに細字（`<600`）を使わない

---

## 8. Responsive Behavior

### Breakpoints
| 名称 | 条件 | 主な変化 |
|---|---|---|
| Desktop | （標準・`992px` 以上） | フルグリッド。ナビ横並び |
| Tablet | `max-width: 991px` | Hero 1カラム＋中央寄せ、利用シーンは2カラム、ハイライト1カラム、ナビ/CTA を隠しハンバーガー表示 |
| Mobile | `max-width: 767px` | セクション `3.5rem`、見出し `1.375rem`、お悩み/機能/利用シーン/料金すべて1カラム、Hero フロートカード非表示、フォーム1カラム、フッター1カラム |
| Small | `max-width: 479px` | ルート `font-size:15px`、コンテナ `padding:0 1rem`、統計数字 `1.375rem` |

> 機能グリッドは Tablet までは `repeat(2,1fr)` を維持し、Mobile で 1カラムへ。設計の起点は「PC（〜`991px` 手前）」と「SP（`767px` 以下）」の2軸。

### Touch Targets
- ボタン標準 `padding:0.875rem 2rem`（高さ約46px）でタップしやすさを確保
- ナビは `991px` 以下でハンバーガー（`22×2px` 3本線、`padding:4px`）に集約

### Collapsing Strategy
- 多くのグリッドは `repeat(2〜3,1fr)` → Mobile で `1fr` へ
- Hero/ハイライト/連絡先の2カラムは Tablet 以下で縦積み、Hero は中央寄せに
- 見出し: `1.75rem` → SP `1.375rem`、セクション余白 `5rem` → `3.5rem`

### Image Behavior
- 画像は `max-width:100%; height:auto; display:block`、枠は `aspect-ratio`（Hero/ハイライト `4/3`、機能/スクショ `16/9`）で比率維持・`object-fit:cover`
- Hero フロートカードは Mobile で非表示にして情報量を絞る

---

## 9. Agent Prompt Guide

### Quick Color Reference
```
Primary        #5363B4   ← 主役・ラベル・リンク的アクセント・フォーカス
Primary Dark   #383874   ← バッジ文字・CTAグラデ始点・影の色源(rgba 56,56,116)
Primary Bright #758BFD   ← CTAグラデ終点
Text (ink)     #141432   ← 本文（純黒ではない濃紺）・ボタン地・フッター地
Text Sub       #626276   ← 説明・リード
Text Muted     #9393A1   ← キャプション・注記
Pale Blue      #C0CAFF   ← 機能タグ(目標管理)・Heroバッジ・上罫
Pale Red       #FEA5A5   ← 機能タグ(経歴)        Deep Red  #FD7C7C ← 必須ラベル
Pale Yellow    #FFE88F   ← 機能タグ(タスク)・上罫  Deep Mint #7BDBAD ← 料金✓
Pale Mint      #A4E6C7   ← 機能タグ(1on1)・チェック・上罫
Pale Pink      #FFB0D6   ← 機能タグ(お知らせ)     Purple    #E8B5FB ← 機能タグ(分析)
Surface Grey   #F9F9F9 / #F2F2F2   ← 帯・プレースホルダ
Border         #E8E8EB（カード） / #EBEBEB（入力）
Pale Blue BG   #F1F3FF   Warm Yellow BG #FFFAE7   ← Heroグラデ両端・淡背景
Background     #FFFFFF
```

### Example Component Prompts
そのまま ClaudeDesign／モック生成に渡せる粒度の指示例（実測値入り）:

1. **主CTAボタン**
   「濃紺地・白文字のCTAボタン。`background:#141432; color:#fff; border:1px solid #141432; border-radius:0.5rem; padding:0.875rem 2rem; font-family:'Noto Sans JP'; font-weight:500; letter-spacing:0.04em;` ホバーで `background:#fff; color:#141432;` に反転、`transition:color .15s ease, background-color .15s ease`。右に `0.75rem` 角の矢印SVGを置き、hover で `translateX(3px)`。」

2. **機能カード（2カラムグリッド）**
   「`display:grid; grid-template-columns:repeat(2,1fr); gap:2rem;` のカードグリッド。各カードは `background:#fff; border-radius:1.25rem; box-shadow:0 0 4px rgba(0,0,0,0.1); overflow:hidden;`。上に `aspect-ratio:16/9` の画像、本文 `padding:1.5rem`。先頭にパステルの丸タグ（`font-size:0.625rem; font-weight:600; padding:0.125rem 0.5rem; border-radius:0.25rem;` 例: 淡青 `#C0CAFF` 地＋`#383874` 文字）、タイトル `1.125rem/600`、説明 `0.875rem/#626276`、`5px` の `#5363B4` ドット付きリスト。hover で `box-shadow:0 0 1.5rem rgba(56,56,116,0.1)` ＋画像 `scale(1.03)`。」

3. **セクション見出し（中央3点セット）**
   「中央寄せの見出しブロック。① ラベル：`font-family:'Manrope'; font-size:0.75rem; font-weight:600; letter-spacing:0.1em; text-transform:uppercase; color:#5363B4;` ② タイトル：`font-size:1.75rem; font-weight:700; letter-spacing:0.02em; color:#141432;` ③ サブタイトル：`font-size:1rem; color:#626276; max-width:640px; margin:0 auto 3rem;`。」

4. **Hero（左テキスト／右ビジュアル＋グラデ背景）**
   「`background:linear-gradient(135deg,#F1F3FF 0%,#fff 60%,#FFFAE7 100%); padding:8rem 0 5rem;`。`grid-template-columns:1fr 1fr; gap:3rem;`。左：ピルバッジ（淡青 `#C0CAFF` 地・`#383874` 文字・先頭に `6px` の `#5363B4` ドット）→ `font-size:2.5rem; line-height:1.25` の見出し（一部を `color:#5363B4`）→ `1.0625rem/#626276/line-height:1.8` の説明 → ボタン2つ → Manrope の統計数字（`1.75rem/700/#5363B4`）。右：`aspect-ratio:4/3; border-radius:1.25rem; box-shadow:0 0 1.5rem rgba(56,56,116,0.1)` の画像に、白いフロートカード（`border-radius:0.5rem`＋同じブランド影）を重ねる。`991px` 以下で1カラム中央寄せ。」

5. **料金カード（3枚・中央おすすめ強調）**
   「`grid-template-columns:repeat(3,1fr); gap:1.5rem; max-width:960px;`。各カード `background:#fff; border:1px solid #E8E8EB; border-radius:1.25rem; padding:2.5rem 2rem; text-align:center;`。中央のおすすめは `border-color:#5363B4; box-shadow:0 0 1.5rem rgba(56,56,116,0.1);` ＋上辺にピルバッジ（`#5363B4` 地・白字・`border-radius:100px`）。プラン名 Manrope `0.875rem/600/#626276`、価格 Manrope `2.5rem/700/#141432`、特徴は `✓`（`#7BDBAD`）付きの左寄せリスト。」

6. **FAQ アコーディオン**
   「`border:1px solid #E8E8EB; border-radius:0.5rem;` のアイテムを `gap:0.75rem` で縦に。質問行は `padding:1.25rem 1.5rem; font-weight:600; font-size:0.9375rem; color:#141432;`、右に `+`（`#9393A1`）。開いたら `+` を `rotate(45deg)`、回答 `0.875rem/#626276/line-height:1.8` を表示。hover で枠 `#626276`。」

7. **CTA 帯（濃グラデ）**
   「`padding:5rem 0; background:linear-gradient(135deg,#383874 0%,#5363B4 50%,#758BFD 100%); color:#fff; text-align:center;`。見出し `2rem`、本文 `opacity:0.85; max-width:560px;`、白ボタン（`--white`：白地・濃紺字、hover で `background:transparent; color:#fff`）。」

### Iteration Guide
1. **まず白地・濃紺文字 `#141432`・主役 `#5363B4` の3トーン**で全体を組む。パステルはカテゴリ識別が要る所だけ後から足す。
2. **角丸はカード `1.25rem`／ボタン `0.5rem`／タグ `0.25rem`／ピル `100px`** で固定。シャープな `2px` 角にしない。
3. **影はまず `--shadow-light`、強調したい所だけ `--shadow-brand`（ブランド色のにじみ）**。黒い強い影に逃げない。hover で light → brand に一段上げる。
4. **本文色は純黒にしない**。`#141432`→`#626276`→`#9393A1` の3段で濃度を落として階層を作る。
5. **数字・英字ラベル・ロゴは Manrope、和文は Noto Sans JP**。ラベルは `uppercase`＋`letter-spacing:0.1em`。
6. **グラデーションは Hero と CTA の2か所だけ**。`135deg`。それ以外はフラットに保つ。
7. **パステルが広い面のベタ塗りになっていないか毎回確認**。識別の範囲（タグ・上罫・小バッジ）を超えていたら戻す。
8. **レスポンシブは PC / SP(`767px`) の2軸**。グリッドは SP で1カラム、機能だけ Tablet まで2カラムを維持。
