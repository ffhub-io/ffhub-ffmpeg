# FFHub FFmpeg Skill

[中文](./docs/README.zh.md) | [日本語](./docs/README.ja.md) | [Español](./docs/README.es.md) | [Русский](./docs/README.ru.md)

> An AI agent skill for cloud FFmpeg processing — works with Claude Code, Cursor, Codex, OpenCode, and [40+ agents](https://github.com/vercel-labs/skills#supported-agents).

Process video and audio files in the cloud via [FFHub.io](https://ffhub.io) API. No local FFmpeg installation required.

## Features

| Feature | Description |
|---------|-------------|
| **Convert** | MP4, WebM, MKV, AVI, MOV, MP3, WAV, AAC, FLAC, etc. |
| **Compress** | Reduce file size with quality control (CRF, bitrate) |
| **Trim** | Cut video/audio by time range |
| **Resize** | Scale to any resolution |
| **Extract Audio** | Pull audio track from video |
| **Thumbnails** | Capture frames as images (JPG, PNG, WebP) |
| **GIF** | Convert video clips to animated GIFs |
| **Upload** | Upload local files, then process in the cloud |

## Install

### Method 1: npx skills (recommended)

Works with Claude Code, Cursor, Codex, OpenCode, Cline, Windsurf, and [40+ agents](https://github.com/vercel-labs/skills#supported-agents).

```bash
# Install to all detected agents
npx skills add ffhub-io/ffhub-ffmpeg

# Install to a specific agent
npx skills add ffhub-io/ffhub-ffmpeg -a claude-code
npx skills add ffhub-io/ffhub-ffmpeg -a cursor
npx skills add ffhub-io/ffhub-ffmpeg -a codex

# Install globally (available across all projects)
npx skills add ffhub-io/ffhub-ffmpeg -g

# List available skills before installing
npx skills add ffhub-io/ffhub-ffmpeg --list
```

### Method 2: Claude Code Plugin

```bash
claude /plugin install ffhub-io/ffhub-ffmpeg
```

## Setup

1. Sign up at [ffhub.io](https://ffhub.io)
2. Get your API key from **Settings > API Keys**
3. Set the environment variable:

```bash
export FFHUB_API_KEY=your_key_here
```

> **Tip:** Add it to your shell profile (`~/.bashrc`, `~/.zshrc`) so it persists across sessions.

## Usage

Invoke the skill with a natural language description of what you want:

```
/ffmpeg compress this video: https://example.com/video.mp4
/ffmpeg convert https://example.com/video.mov to mp4
/ffmpeg extract audio from https://example.com/video.mp4
/ffmpeg trim https://example.com/video.mp4 from 00:01:00 to 00:02:30
/ffmpeg make a 3-second gif starting at 10s: https://example.com/video.mp4
/ffmpeg resize https://example.com/video.mp4 to 1280x720
```

You can also use local files — they will be uploaded automatically:

```
/ffmpeg compress ./my-video.mp4
/ffmpeg convert ~/Downloads/recording.mov to mp4
```

## How It Works

```
You describe the task
        ↓
AI generates FFmpeg command
        ↓
Upload local files (if needed)
        ↓
Submit to FFHub.io cloud API
        ↓
FFHub processes the file (cloud FFmpeg)
        ↓
Returns download URLs + file info
```

All processing happens in the cloud — fast, reliable, and no local dependencies.

## Supported Formats

| Type | Formats |
|------|---------|
| **Video** | `.mp4` `.webm` `.mkv` `.avi` `.mov` `.flv` |
| **Audio** | `.mp3` `.wav` `.aac` `.ogg` `.flac` `.m4a` |
| **Image** | `.gif` `.png` `.jpg` `.jpeg` `.webp` |

## Requirements

- [FFHub.io](https://ffhub.io) API key (free tier available)
- `curl` and `jq` (pre-installed on most systems)

## Manage Skills

```bash
# List installed skills
npx skills list

# Update to latest version
npx skills update ffmpeg

# Remove
npx skills remove ffmpeg
```

## Links

- [FFHub.io](https://ffhub.io) — Cloud FFmpeg service
- [Agent Skills Spec](https://agentskills.io) — Open agent skills standard
- [Skills Directory](https://skills.sh) — Discover more skills
- [npx skills CLI](https://github.com/vercel-labs/skills) — Universal skill installer

## License

MIT
