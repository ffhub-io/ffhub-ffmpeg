# FFHub FFmpeg Skill

[English](../README.md) | [中文](./README.zh.md) | [Español](./README.es.md) | [Русский](./README.ru.md)

> クラウド FFmpeg 処理のための AI エージェントスキル — Claude Code、Cursor、Codex、OpenCode など [40 以上のエージェント](https://github.com/vercel-labs/skills#supported-agents) に対応。

[FFHub.io](https://ffhub.io) API を通じて、クラウドで動画・音声ファイルを処理できます。ローカルに FFmpeg をインストールする必要はありません。

## 機能

| 機能 | 説明 |
|------|------|
| **フォーマット変換** | MP4、WebM、MKV、AVI、MOV、MP3、WAV、AAC、FLAC など |
| **動画圧縮** | CRF・ビットレート制御でファイルサイズを削減 |
| **トリミング** | 時間範囲を指定して動画・音声をカット |
| **リサイズ** | 任意の解像度にスケーリング |
| **音声抽出** | 動画から音声トラックを抽出 |
| **サムネイル生成** | フレームを画像としてキャプチャ（JPG、PNG、WebP） |
| **GIF 作成** | 動画クリップをアニメーション GIF に変換 |
| **ファイルアップロード** | ローカルファイルをアップロードし、クラウドで処理 |

## インストール

### 方法 1：npx skills（推奨）

Claude Code、Cursor、Codex、OpenCode、Cline、Windsurf など [40 以上のエージェント](https://github.com/vercel-labs/skills#supported-agents) に対応。

```bash
# 検出されたすべてのエージェントにインストール
npx skills add ffhub-io/ffhub-ffmpeg

# 特定のエージェントにインストール
npx skills add ffhub-io/ffhub-ffmpeg -a claude-code
npx skills add ffhub-io/ffhub-ffmpeg -a cursor
npx skills add ffhub-io/ffhub-ffmpeg -a codex

# グローバルインストール（すべてのプロジェクトで利用可能）
npx skills add ffhub-io/ffhub-ffmpeg -g

# インストール前にスキル一覧を確認
npx skills add ffhub-io/ffhub-ffmpeg --list
```

### 方法 2：Claude Code プラグイン

```bash
claude /plugin install ffhub-io/ffhub-ffmpeg
```

## セットアップ

1. [ffhub.io](https://ffhub.io) でアカウントを作成
2. **Settings > API Keys** から API キーを取得
3. 環境変数を設定：

```bash
export FFHUB_API_KEY=your_key_here
```

> **ヒント：** シェル設定ファイル（`~/.bashrc`、`~/.zshrc`）に追加すると、すべてのセッションで有効になります。

## 使い方

自然言語でやりたいことを記述するだけです：

```
/ffmpeg この動画を圧縮して: https://example.com/video.mp4
/ffmpeg https://example.com/video.mov を mp4 に変換
/ffmpeg https://example.com/video.mp4 から音声を抽出
/ffmpeg https://example.com/video.mp4 を 00:01:00 から 00:02:30 までトリミング
/ffmpeg 10 秒から 3 秒間の gif を作成: https://example.com/video.mp4
/ffmpeg https://example.com/video.mp4 を 1280x720 にリサイズ
```

ローカルファイルも使えます — 自動的にアップロードされます：

```
/ffmpeg ./my-video.mp4 を圧縮
/ffmpeg ~/Downloads/recording.mov を mp4 に変換
```

## 仕組み

```
やりたいことを記述
        ↓
AI が FFmpeg コマンドを生成
        ↓
ローカルファイルをアップロード（必要な場合）
        ↓
FFHub.io クラウド API に送信
        ↓
FFHub がクラウドでファイルを処理
        ↓
ダウンロード URL + ファイル情報を返却
```

すべての処理はクラウドで行われます — 高速・安定・ローカル依存なし。

## 対応フォーマット

| 種類 | フォーマット |
|------|-------------|
| **動画** | `.mp4` `.webm` `.mkv` `.avi` `.mov` `.flv` |
| **音声** | `.mp3` `.wav` `.aac` `.ogg` `.flac` `.m4a` |
| **画像** | `.gif` `.png` `.jpg` `.jpeg` `.webp` |

## 必要条件

- [FFHub.io](https://ffhub.io) API キー（無料枠あり）
- `curl` と `jq`（ほとんどのシステムにプリインストール済み）

## スキル管理

```bash
# インストール済みスキルを一覧表示
npx skills list

# 最新バージョンに更新
npx skills update ffmpeg

# アンインストール
npx skills remove ffmpeg
```

## リンク

- [FFHub.io](https://ffhub.io) — クラウド FFmpeg サービス
- [Agent Skills 仕様](https://agentskills.io) — オープンエージェントスキル標準
- [Skills ディレクトリ](https://skills.sh) — その他のスキルを探す
- [npx skills CLI](https://github.com/vercel-labs/skills) — ユニバーサルスキルインストーラー

## ライセンス

MIT
