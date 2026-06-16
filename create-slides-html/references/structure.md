# reveal.js + Tailwind 構造リファレンス

スライドHTMLを書く前にこのファイルを読むこと。構造ルールの「なぜ」と、そのまま使える雛形・サンプルを置いている。

## 目次
1. 出力形態（単一HTML / フルテンプレート）
2. HTML骨格と reveal.js の読み込み
3. Tailwind config（色・フォント・`c-` クラス）
4. section の入れ子と r-stretch ラッパー
5. やってはいけないこと早見表
6. 雛形 `assets/base.html` の使い方
7. スライドパターン例
8. グラフ・チャート（Chart.js）

---

## 1. 出力形態

- 既定は**単一HTMLファイル**。reveal.js と Tailwind を CDN から読み込めば、ビルド不要でブラウザ/ダブルクリックで開ける。`assets/base.html` をコピーして中身を差し替えるのが最短。
- ユーザーが `slide-template/`（reveal.js を submodule、`build.js`、`lib/tailwind/config.js` 等）のようなリポジトリ構成を指定した場合だけ、その構成に合わせて `src/index.html` と `lib/tailwind/config.js` を分けて書く。理想構成は次の通り（参考）:

```
slide-template/
├── src/index.html        # メインスライド
│   └── assets/           # 画像など
├── examples/index.html   # デザインパターン
├── lib/tailwind/         # Tailwind config
├── customizations/       # カスタマイズスクリプト
├── reveal.js/            # reveal.js (submodule)
├── dist/  build.js  package.json
```

単一HTMLの場合、上記の `lib/tailwind/config.js` 相当を HTML 内の `tailwind.config = {...}` にインラインで持つ。

---

## 2. HTML骨格

CDN は安定版を使う。reveal.js は CSS（reveal + テーマ）と JS、Tailwind は v3 の play CDN、フォントは Google Fonts。

```html
<!doctype html>
<html lang="ja">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>スライドタイトル</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js@5/dist/reveal.css">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js@5/dist/theme/white.css" id="theme">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Lato:wght@300;400;700&family=Poppins:wght@600&family=Noto+Sans+JP:wght@400;700&display=swap" rel="stylesheet">
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    // デフォルト（Howard由来）。色は必ずこの名前で参照する
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            bg: '#FFFFFF', surface: '#F2F2F2', ink: '#44546A', sub: '#999999',
            slate: '#3D698F', tealgrey: '#80A7A5', sage: '#ABBE92', mauve: '#775864'
          },
          fontFamily: {
            head: ['Arial', '"Helvetica Neue"', '"游ゴシック"', 'sans-serif'],
            body: ['Lato', '"游ゴシック"', 'sans-serif'],
            accent: ['Poppins', 'Arial', 'sans-serif']
          }
        }
      }
    }
  </script>
  <style>
    .reveal { font-family: 'Lato', '"游ゴシック"', sans-serif; }
    .reveal section { text-align: left; }
    /* c- カスタムクラスは下の @layer components で定義 */
  </style>
</head>
<body>
  <div class="reveal">
    <div class="slides">
      <!-- ここに section を並べる -->
    </div>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/reveal.js@5/dist/reveal.js"></script>
  <script>
    Reveal.initialize({ hash: true, transition: 'none', width: 1280, height: 720, margin: 0.04 });
  </script>
</body>
</html>
```

`transition: 'none'` にするのは、装飾的なトランジションを使わない方針のため。

---

## 3. Tailwind config（色・フォント・`c-` クラス）

- 色は必ず config の名前で参照する（`text-ink`, `bg-slate`, `text-sub` 等）。スライド本文に `#3D698F` のような**カラーコードを直書きしない**。色を変えたいときは config 1か所を直せば全スライドに効くようにするため。
- カスタムクラスは `c-` プレフィックス。例：`c-title`（全大文字見出し）、`c-lead`（リード）、`c-tier1/2/3`（階層）、`c-card`（色面カード）、`c-stat`（統計数値）。`@layer components` で定義する。
- フォントは見出し=`font-head`（Arial Bold・全大文字）、本文=`font-body`（Lato Light）。見出しは太字＋ALL CAPS、本文は細字で対比をつける。

```html
<style type="text/tailwindcss">
  @layer components {
    /* セクション見出し：Arial Bold 全大文字。白地は ink、色面は text-white を併用 */
    .c-title { @apply font-head font-bold uppercase tracking-wide text-[2.4rem] text-ink leading-tight; }
    /* リード/本文：Lato Light */
    .c-lead  { @apply font-body font-light text-[1.1rem] text-ink; }
    /* ロジックツリー階層 */
    .c-tier1 { @apply font-head font-bold text-[1.1rem] text-ink mt-4; }
    .c-tier2 { @apply font-body font-light text-[0.95rem] text-ink ml-6 mt-1.5; }
    .c-tier3 { @apply font-body font-light text-[0.85rem] text-sub ml-12 mt-1; }
    .c-note  { @apply font-body text-[0.7rem] text-sub; }
    /* 色面カード：ベタ塗りに白文字 */
    .c-card  { @apply text-white grid content-center gap-2 p-6; }
    /* 統計数値：Arial Bold */
    .c-stat  { @apply font-head font-bold text-[2rem]; }
  }
</style>
```

グラデーションは使わない。表紙・セクション扉は `bg-slate`（スレートブルー一色）を section に当てるだけでよい。`type="text/tailwindcss"` を付けると play CDN が `@apply` を解釈する。

---

## 4. section の入れ子と r-stretch ラッパー（最重要）

reveal.js は `.slides > section` を1枚のスライドとして扱う。`section` をさらに入れ子にすると縦方向のスライドグループになる。本スキルでは見通しのため **`.slides > section > section`** の入れ子を基本形にする。

**全 `<section>` の直下には r-stretch ラッパーを1つ置く。** r-stretch は reveal.js が利用可能な高さいっぱいに要素を伸ばす仕組み。これに乗せないと縦位置がスライドごとにばらつく。

- 上ぞろえ： `<div class="r-stretch grid">`
- 中央ぞろえ： `<div class="r-stretch grid place-items-center">`

守るべき制約とその理由：
- **`section` に padding/margin をつけない** — reveal.js のオートスケールと干渉して、画面サイズで余白がずれるため。余白はラッパー側（`p-*`）で取る。
- **r-stretch と同じ要素に `gap` をつけない** — gap が r-stretch の高さ計算を壊す。gap が要るときは内側にもう1段 `div` を噛ませ、その div で `gap-*` を使う。

```html
<section>
  <section class="bg-slate">   <!-- 表紙・扉はスレートブルー一色 -->
    <div class="r-stretch grid place-items-center p-12">
      <div class="grid gap-4">   <!-- gap はここ。r-stretch と別要素 -->
        <div class="c-title text-white text-[3.2rem]">タイトルスライドの見出し</div>
        <div class="c-note text-white/80">2026.06 / 作成者</div>
      </div>
    </div>
  </section>

  <section>                      <!-- 本文は白地 -->
    <div class="r-stretch grid p-12">       <!-- 上ぞろえ。gap なし -->
      <div class="grid gap-3">
        <div class="c-title">結論を1文で言い切る</div>
        <div class="grid gap-2">
          <div class="c-tier1">主論点その1</div>
          <div class="c-tier2">それを支える根拠</div>
          <div class="c-tier3">裏づけデータ</div>
        </div>
      </div>
    </div>
  </section>
</section>
```

---

## 5. やってはいけないこと早見表

| NG | 代わりに |
|----|----------|
| `<ul><li>…` で箇条書き | `div` のインデント＋`c-tier*` |
| `<section style="padding…">` | ラッパー `div` で `p-*` |
| `<div class="r-stretch grid gap-4">` | r-stretch と gap を別要素に分ける |
| `text-[#3D698F]` 直書き | `text-slate`（config 名で参照） |
| 見出しが細字・小文字 | 見出し`font-head font-bold uppercase`／本文`font-body font-light` |
| グラデ背景 | `bg-slate` のベタ塗り |
| 絵文字 🎉 | ユーザー要求時のみ |
| 影・装飾枠で飾る | ベタ塗り色面（`bg-slate`/`bg-surface`）と余白で差をつける |
| 手書きSVG/画像/CSS擬似グラフ | Chart.js（CDN）で描く（§8）|

---

## 6. 雛形 `assets/base.html` の使い方

`assets/base.html` は上記をまとめた動く雛形。手順：
1. `assets/base.html` を成果物の出力先にコピーする。
2. `<div class="slides">` の中身を、フェーズ2.5で整えたテキストで `section` を組んで差し替える。
3. 必要なら `tailwind.config` の色名・`c-` クラスを増やす（カラーコード直書きはしない）。
4. ブラウザで開いて、各スライドの縦位置・余白・1スライド1メッセージを確認する。

---

## 7. スライドパターン例

タイトル（中央ぞろえ）、標準コンテンツ（上ぞろえ＋ロジックツリー）、3カラム色面、統計カードの最小例は `assets/base.html` 内にコメント付きで入れてある。新しいパターンが要るときも、必ず「section 直下に r-stretch ラッパー」「gap は内側」「色は config 名」を守る。

---

## 8. グラフ・チャート（Chart.js）

数値の比較・推移・構成比をグラフで見せるときは、**必ず Chart.j