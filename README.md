# Claude Code Skills Collection

Claude Code で使えるカスタムスキル（スラッシュコマンド）のコレクションです。
開発ワークフローの自動化・効率化を目的としたスキルを収録しています。

## スキル一覧

| スキル名 | コマンド | 説明 |
|---------|---------|------|
| [commit-changes](commit-changes/) | `/commit-changes` | 未コミットの変更を分析し、コミットメッセージを自動生成 |
| [compress-video](compress-video/) | `/compress-video` | FFmpeg で動画ファイルを圧縮してファイルサイズを削減 |
| [create-pr](create-pr/) | `/create-pr` | ブランチ名とコミット内容からPRを自動作成 |
| [review-and-fix](review-and-fix/) | `/review-and-fix` | PRをレビューし、指摘を重要度順に1件ずつ確認・修正 |
| [rewrite-ai-tone](rewrite-ai-tone/) | `/rewrite-ai-tone` | AI が書いた文章を自然な日本語にリライト |
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

### review-and-fix

指定した PR の diff をレビューし、指摘事項を重要度順（Critical → High → Medium → Low → Info）に1件ずつ提示します。

- PR番号の指定、または一覧からの選択に対応
- 5段階の重要度（Critical / High / Medium / Low / Info）で分類
- 各指摘に対して「修正する」「方針を変えて修正」「スキップ」を選択
- レビュー結果を `.claude/reviews/pr-{番号}.md` に保存
- 修正後に README の更新とコミットメッセージの生成まで実施

### rewrite-ai-tone

AI が書いた文章を、人が書いたような自然な日本語に書き直します。

- Markdown 記法・過剰な記号を排除
- テンプレ感・説明書感・過剰な丁寧さを除去
- 前置き宣言や安全クッション表現を削除
- 抽象語を動詞中心の具体的表現に置換
- 意味と事実関係は変えずにリライト

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
├── commit-changes/    # コミットメッセージ自動生成
│   └── SKILL.md
├── compress-video/    # 動画圧縮
│   └── SKILL.md
├── create-pr/         # PR自動作成
│   └── SKILL.md
├── review-and-fix/    # PRレビュー＆修正
│   └── SKILL.md
├── rewrite-ai-tone/   # AI臭リライト
│   └── SKILL.md
├── search-skills/     # スキル検索
│   └── SKILL.md
└── update-readme/     # README更新
    └── SKILL.md
```

## 動作環境

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) がインストールされていること
- `create-pr` / `review-and-fix` は [GitHub CLI (gh)](https://cli.github.com/) が必要
- `compress-video` は [FFmpeg](https://ffmpeg.org/) が必要（未導入の場合は自動ダウンロード）
