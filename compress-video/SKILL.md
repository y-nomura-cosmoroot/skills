# compress-video

MP4などの動画ファイルを FFmpeg で圧縮してファイルサイズを削減するスキル。
FFmpeg が未導入でも、初回実行時にポータブル版を自動取得して動作する。

## トリガー

ユーザーが動画の圧縮・サイズ削減を依頼した場合に実行する。
例: 「動画を圧縮して」「MP4のサイズを小さくして」「compress-video」

## 実行環境

Claude Code は Windows 上で **Git Bash（MSYS2）** を介してコマンドを実行する。
- すべてのコマンドは **Bash 構文** で記述すること
- パスは Git Bash 形式（`/c/Users/...`）を使用すること
- PowerShell や cmd の構文は使用しない

## スキルのベースディレクトリ

このスキルが読み込まれると、Claude Code は以下の形式でベースディレクトリを通知する:

```
Base directory for this skill: C:\Users\ユーザー名\.claude\skills\compress-video
```

このパスを Git Bash 形式に変換して `SKILL_DIR` として使用する。
FFmpeg のポータブル版は `${SKILL_DIR}/bin/` に配置する。

**パス変換の例:**
- Windows: `C:\Users\野村祐介\.claude\skills\compress-video`
- Git Bash: `/c/Users/野村祐介/.claude/skills/compress-video`

## 実行手順

### 1. FFmpeg の準備（自動ブートストラップ）

以下の優先順で FFmpeg を探す。

#### 1-1. システムにインストール済みか確認

```bash
which ffmpeg 2>/dev/null
```

見つかればそのパスを使う。ステップ2へ進む。

#### 1-2. スキルフォルダにポータブル版があるか確認

```bash
test -f "${SKILL_DIR}/bin/ffmpeg.exe" && echo "found"
```

存在すればそのパスを使う。ステップ2へ進む。

#### 1-3. ポータブル版を自動ダウンロード

上記どちらも見つからない場合、AskUserQuestion ツールでユーザーに許可を取る:

```
FFmpeg が見つかりません。
ポータブル版（約100MB）を自動ダウンロードしてスキルフォルダに配置します。
システムには影響しません。不要になったらスキルフォルダごと削除できます。
ダウンロード元: https://www.gyan.dev/ffmpeg/builds/ (FFmpeg公式推奨の配布元)
配置先: ${SKILL_DIR}/bin/

ダウンロードしてよいですか？
```

ユーザーが許可した場合、以下を **1コマンドずつ順番に** 実行する:

```bash
# 1. ダウンロード先ディレクトリの作成
mkdir -p "${SKILL_DIR}/bin"
```

```bash
# 2. FFmpeg essentials ビルドをダウンロード
curl -L -o "${SKILL_DIR}/bin/ffmpeg.zip" "https://www.gyan.dev/ffmpeg/builds/ffmpeg-release-essentials.zip"
```

```bash
# 3. 解凍
unzip -o "${SKILL_DIR}/bin/ffmpeg.zip" -d "${SKILL_DIR}/bin/temp"
```

```bash
# 4. exe を取り出す（解凍先はサブフォルダが1階層ある）
cp "${SKILL_DIR}"/bin/temp/ffmpeg-*-essentials_build/bin/ffmpeg.exe "${SKILL_DIR}/bin/ffmpeg.exe"
cp "${SKILL_DIR}"/bin/temp/ffmpeg-*-essentials_build/bin/ffprobe.exe "${SKILL_DIR}/bin/ffprobe.exe"
```

```bash
# 5. 一時ファイルの削除
rm -f "${SKILL_DIR}/bin/ffmpeg.zip"
rm -rf "${SKILL_DIR}/bin/temp"
```

ダウンロード完了後、正常に動作するか確認する:

```bash
"${SKILL_DIR}/bin/ffmpeg.exe" -version
```

失敗した場合はエラー内容をユーザーに伝えて中断する。

#### 1-4. FFmpeg パスの決定

以降のステップでは、特定した FFmpeg のフルパスを変数として保持する:
- システムにある場合: `FFMPEG=ffmpeg` / `FFPROBE=ffprobe`
- ポータブル版の場合: `FFMPEG="${SKILL_DIR}/bin/ffmpeg.exe"` / `FFPROBE="${SKILL_DIR}/bin/ffprobe.exe"`

以降のコマンド例では `${FFMPEG}` / `${FFPROBE}` と表記する。

### 2. 対象ファイルの特定

- ユーザーが指定したファイルパスまたはフォルダパスを使用する
- Windows パス（`C:\...`）が渡された場合は Git Bash 形式（`/c/...`）に変換する
- フォルダが指定された場合は、フォルダ内の `.mp4`, `.mov`, `.avi`, `.mkv`, `.wmv`, `.flv`, `.webm` ファイルを対象とする
- 対象ファイルが見つからない場合はユーザーに確認する

### 3. 元ファイルの情報取得

対象ファイルごとに以下を実行して情報を取得する:

```bash
"${FFPROBE}" -v quiet -print_format json -show_format -show_streams "入力ファイル"
```

取得する情報:
- ファイルサイズ
- 動画コーデック（H.264 / H.265 / VP9 等）
- 解像度（幅 x 高さ）
- ビットレート
- 再生時間
- 音声コーデック・ビットレート

### 4. 圧縮プランの提示

AskUserQuestion ツールを使って圧縮レベルを確認する:

**選択肢:**

| レベル | 説明 | CRF値 | 目安削減率 |
|--------|------|-------|-----------|
| 品質優先 | ほぼ見た目の劣化なし | 20 | 20-30% |
| バランス（推奨） | 軽微な劣化で大幅削減 | 26 | 40-60% |
| サイズ優先 | 多少の劣化を許容して最大削減 | 32 | 60-80% |

加えて、以下のオプションも確認する:
- **コーデック**: 元が H.264 の場合、H.265(HEVC) への変換を提案する（さらに30-50%削減可能だが圧縮に時間がかかる）
- **解像度変更**: 4K→1080p、1080p→720p など解像度ダウンの必要があるか

### 5. 圧縮の実行

選択に応じて FFmpeg コマンドを構築して実行する。

#### H.264 で圧縮する場合:
```bash
"${FFMPEG}" -i "入力.mp4" -c:v libx264 -crf {CRF値} -preset medium -c:a aac -b:a 128k -movflags +faststart "出力ファイル"
```

#### H.265(HEVC) で圧縮する場合:
```bash
"${FFMPEG}" -i "入力.mp4" -c:v libx265 -crf {CRF値} -preset medium -tag:v hvc1 -c:a aac -b:a 128k -movflags +faststart "出力ファイル"
```

#### 解像度を変更する場合（例: 1080p）:
```bash
"${FFMPEG}" -i "入力.mp4" -c:v libx264 -crf {CRF値} -preset medium -vf "scale=-2:1080" -c:a aac -b:a 128k -movflags +faststart "出力ファイル"
```

**出力ファイル名のルール:**
- 元のファイル名に `_compressed` を付与する
- 例: `video.mp4` → `video_compressed.mp4`
- 元のファイルは削除しない（上書きしない）

**注意:**
- `-movflags +faststart` を必ず付ける（Web再生の高速化）
- `-preset medium` をデフォルトとする（速度と圧縮率のバランス）
- 実行前にコマンドをユーザーに表示して確認を取る
- 長時間かかる可能性がある場合はその旨を伝える

### 6. 結果の報告

圧縮完了後、以下を報告する:

```
## 圧縮結果

| 項目 | 圧縮前 | 圧縮後 |
|------|--------|--------|
| ファイルサイズ | XX MB | XX MB (XX% 削減) |
| コーデック | H.264 | H.265 |
| 解像度 | 1920x1080 | 1920x1080 |
| ビットレート | XX Mbps | XX Mbps |

出力ファイル: path/to/video_compressed.mp4
```

### 7. 複数ファイルの一括処理

フォルダが指定された場合:
- 対象ファイルの一覧と合計サイズを表示する
- 圧縮レベルの選択は一括で行う（ファイルごとには聞かない）
- 各ファイルの圧縮結果を順次報告する
- 最後に全体のサマリーを表示する:

```
## 一括圧縮サマリー

処理ファイル数: X 件
合計サイズ: XX MB → XX MB (XX% 削減)
```
