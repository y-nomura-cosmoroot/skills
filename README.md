# Claude Code Skills Collection

Claude Code で使えるカスタムスキル（スラッシュコマンド）のコレクションです。
開発ワークフローの自動化・効率化を目的としたスキルを収録しています。

## スキル一覧

| スキル名 | コマンド | 説明 |
|---------|---------|------|
| [commit-changes](commit-changes/) | `/commit-changes` | 未コミットの変更を分析し、コミットメッセージを自動生成 |
| [compress-video](compress-video/) | `/compress-video` | FFmpeg で動画ファイルを圧縮してファイルサイズを削減 |
| [create-pr](create-pr/) | `/create-pr` | ブランチ名とコミット内容からPRを自動作成 |
| [create-slides-html](create-slides-html/) | `/create-slides-html` | reveal.js + Tailwind CSS でAIっぽさを消したHTMLスライドを作成 |
| [review-and-fix](review-and-fix/) | `/review-and-fix` | PRをレビューし、指摘を重要度順に1件ずつ確認・修正 |
| [rewrite-ai-tone](rewrite-ai-tone/) | `/rewrite-ai-tone` | AI が書いた文章を自然な日本語にリライト |
| [rewrite-for-onenote](rewrite-for-onenote/) | `/rewrite-for-onenote` | `from_notebooklm.md` のAI臭を消してOneNote向けに整形し `to_onenote.md` へ出力 |
| [search-skills](search-skills/) | `/search-skills` | 要望に合った Claude Code スキルをキュレーションリストから検索 |
| [update-readme](update-readme/) | `/update-readme` | README を最新の実装内容に基づいて更新 |

## 各スキルの詳細

### commit-changes

`git status` と `git diff` で未コミットの変更を確認し、変更内容を分析してコミットメッセージを生成します。

- 変更を機能追加・改善・バグ修正・設定変更・リファクタリングに分類
- 1行目に概要、3行目以降に詳細な箇条書きのフォーマットでメッセージを生成
- 日本語・実装視点で記述

### compress-video

MP4 などの動画ファイルを FFmpeg で圧縮します。FFmpeg が未導入でも、初回実行時にポータブル版を自動ダウンロードして動作します。

- 対応フォーマット: `.mp4`, `.mov`, `.avi`, `.mkv`, `.wmv`, `.flv`, `.webm`
- 3段階の圧縮レベル（品質優先 / バランス / サイズ優先）
- H.264 / H.265(HEVC) コーデック対応
- 解像度変更オプション（4K→1080p、1080p→720p など）
- フォルダ指定による一括圧縮
- 元ファイルは保持し `_compressed` 付きで出力

### create-pr

ブランチ名・コミット履歴・差分を分析し、PRのタイトルと説明文を自動生成して GitHub PR を作成します。

- ブランチ名のプレフィックス（`feature/`, `fix/`, `refactor/` 等）から変更種別を推定
- 目的・概要・変更内容・設計方針・影響範囲・確認方法を含む説明文を生成
- 未コミット・未プッシュの変更を検出して事前に警告
- PR作成前にプレビューを表示してユーザー確認

### create-slides-html

reveal.js + Tailwind CSS(CDN) で、人が作ったように見える高品質なHTMLスライドを作成します。ヒアリング→レポート確認→AI臭除去→デザインシステム適用という構造化ワークフローで、AIっぽい見た目と文章を排除します。

- 成果物は単一のHTMLファイル（reveal.js / Tailwind / Chart.js を CDN 読み込み）
- 参照PPTX / 参照サイトが指定された場合は `create-design-md(-from-pptx)` でデザイントークンを抽出して Tailwind config に反映
- 参照なしの場合は `references/designs.json` に登録されたデザイン一覧から選択（design-1: Maurelle、design-2: Howard 等）
- スライド作成前にレポート形式で目的・構成・スライド案をユーザー確認
- グラフは Chart.js（CDN）で描画、図解は `div` / SVG で組む（`ul` / `ol` / `li` は禁止）
- カスタムクラスは `c-` プレフィックスで Tailwind config に集約、カラーコード直書きは禁止

デザイン定義は `references/design-N/DESIGN.md`、実装の構造ルールは `references/structure.md` を参照する構成。

### review-and-fix

指定した PR の diff をレビューし、指摘事項を重要度順（Critical → High → Medium → Low → Info）に1件ずつ提示します。

- PR番号の指定、または一覧からの選択に対応
- 5段階の重要度（Critical / High / Medium / Low / Info）で分類
- 各指摘に対して「修正する」「方針を変えて修正」「スキップ」を選択
- レビュー結果を `.claude/reviews/pr-{番号}.md` に保存
- 修正後に README の更新とコミットメッセージの生成まで実施

### rewrite-ai-tone

AI が書いた文章を、人が書いたような自然な日本語に書き直します。ルールは「見つけたら → こうする」の形で定義し、通読→仕分け→書き換え→最終チェックの手順で適用します。

- 予告文・文書メタ文・総括や洞察の段落・締めの定型句を削除（固有名詞や決定事項を含む部分は残す）
- 確度を元文に戻す（「決定された」→「開始しようとしている」、「即時実施する」→「提案された」など）
- 名詞の積み上げを動詞にほどく（「作り直しを実施する」→「作り直す」、担当の括弧表記を本文に組み込む）
- 大げさ語・カタカナ業務語を日常語に置換する対応表（高度化・転換・ガバナンス・検閲・アクセシビリティなど）
- 抽象名詞を飾るだけの強調語を削除、比較や数値を伴う強調は残す
- 見出しから 分析・評価・総括・洞察 などの分析枕詞を外す
- 元文が Markdown なら見出し・箇条書き・コード記法を保ち、強調の太字と機械的な箇条書きだけ外す。プレーンテキストなら記号を足さない
- 絵文字・ダッシュ・矢印・「： 」・「！」を排除
- 書き言葉でしか使わない語を普段の言い方に（我々→私たち、〜への転換期にある→〜へと変わりつつある、当該→その）
- 同じ語尾の3連続や「〜との話」の機械的な付加を避ける
- 固有名詞・数字・人名・日付は落とさず足さない。長さは増やさず、事実を落とさない限り短くしてよい
- `tests/` に5ジャンルのテスト文章と、修正前・修正後スキルの出力比較（`tests/comparison.md`）を同梱

### rewrite-for-onenote

カレントディレクトリの `from_notebooklm.md` を読み込み、AI臭を除去したうえで OneNote 向けの書式に整形して `to_onenote.md` へ書き出します。NotebookLM の出力を社内の議事録メモに貼り込むワークフローを想定。

- `/rewrite-ai-tone` のルールでAI臭を除去（会議の要約として、発言者と決定事項を保つことを優先）
- 大見出しは ━ の罫線で挟み、サブ見出しには ■、列挙は全角スペース＋中黒
- 段落間の空行を入れない（OneNote では空行が二重行間になるため）
- 一文一行、行末の句点は付けない
- 敬称の「氏」は会議参加者だけ「さん」に置換（外部の著名人は「氏」のまま）
- 議事録として「誰が何をした」「何が決まった」「何が未決か」が読み分けられる書き方にする。文末に「〜との話」を足して報告調にせず、断定が強すぎるときは動詞を直す
- 見出しは何を話したかを普通の語で書く（分析・評価・総括・洞察などの語や「エグゼクティブ・サマリー」は避ける）
- 書き出す前に出力前チェック（一文一行、報告語尾の回数、見出し語、Markdown 記号、固有名詞の残存）を行う
- `to_onenote.md` に既存内容があれば AskUserQuestion で上書き確認

### search-skills

ユーザーの要望に合った Claude Code スキルを公開キュレーションリストから検索して提案します。

- 検索ソース: [awesome-claude-skills](https://github.com/BehiSecc/awesome-claude-skills)、GitHub 検索
- セキュリティチェック付き（高リスク / 中リスク / 低リスク の3段階で評価）
- 安全性ラベル（安全 / 注意 / 非推奨）を付与して提示
- インストール方法の案内

### update-readme

プロジェクトの実装内容を分析し、README.md を最新の状態に更新します。

- プロジェクト構造と実装内容の自動解析
- 既存 README との差分を検出
- Mermaid 図がある場合はロジックとの整合性も確認

## インストール方法

各スキルのディレクトリにある `SKILL.md` を Claude Code のスキルディレクトリに配置してください。

**グローバル（全プロジェクト共通）:**

```
~/.claude/skills/{skill-name}/SKILL.md
```

**プロジェクト固有:**

```
.claude/skills/{skill-name}/SKILL.md
```

## フォルダ構成

```
skills/
├── commit-changes/       # コミットメッセージ自動生成
│   └── SKILL.md
├── compress-video/       # 動画圧縮
│   └── SKILL.md
├── create-pr/            # PR自動作成
│   └── SKILL.md
├── create-slides-html/   # reveal.js + Tailwind HTMLスライド作成
│   ├── SKILL.md
│   ├── assets/           # base.html などの雛形
│   └── references/       # design.md / structure.md / designs.json / design-N/
├── review-and-fix/       # PRレビュー＆修正
│   └── SKILL.md
├── rewrite-ai-tone/      # AI臭リライト
│   ├── SKILL.md
│   └── tests/            # テスト文章（inputs/）と修正前・修正後の出力比較（comparison.md）
├── rewrite-for-onenote/  # OneNote向け整形（from_notebooklm.md → to_onenote.md）
│   └── SKILL.md
├── search-skills/        # スキル検索
│   └── SKILL.md
└── update-readme/        # README更新
    └── SKILL.md
```

## 動作環境

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) がインストールされていること
- `create-pr` / `review-and-fix` は [GitHub CLI (gh)](https://cli.github.com/) が必要
- `compress-video` は [FFmpeg](https://ffmpeg.org/) が必要（未導入の場合は自動ダウンロード）
- `create-slides-html` の成果物HTMLはブラウザで開いて閲覧（reveal.js / Tailwind / Chart.js は CDN 経由のためオフライン閲覧は不可）
- `rewrite-for-onenote` はカレントディレクトリに `from_notebooklm.md` を配置してから実行
