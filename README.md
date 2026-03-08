# FFHub FFmpeg Skill

> An AI agent skill for cloud FFmpeg processing — works with Claude Code, OpenClaw, and any [Agent Skills](https://agentskills.io) compatible tool.

Process video and audio files in the cloud via [FFHub.io](https://ffhub.io) API. No local FFmpeg installation required.

## Features

| Skill | Description |
|-------|-------------|
| **Convert** | MP4, WebM, MKV, AVI, MOV, MP3, WAV, AAC, FLAC, etc. |
| **Compress** | Reduce file size with quality control (CRF, bitrate) |
| **Trim** | Cut video/audio by time range |
| **Resize** | Scale to any resolution |
| **Extract Audio** | Pull audio track from video |
| **Thumbnails** | Capture frames as images (JPG, PNG, WebP) |
| **GIF** | Convert video clips to animated GIFs |

## Install

**Claude Code:**

```bash
claude /plugin install ffhub-io/ffhub-ffmpeg
```

**OpenClaw / ClawHub:**

```bash
clawhub install ffhub-ffmpeg
```

## Setup

1. Sign up at [ffhub.io](https://ffhub.io)
2. Get your API key from **Settings > API Keys**
3. Set the environment variable:

```bash
export FFHUB_API_KEY=your_key_here
```

## Usage

```
/ffhub-ffmpeg:ffmpeg compress this video: https://example.com/video.mp4
/ffhub-ffmpeg:ffmpeg convert https://example.com/video.mov to mp4
/ffhub-ffmpeg:ffmpeg extract audio from https://example.com/video.mp4
/ffhub-ffmpeg:ffmpeg trim https://example.com/video.mp4 from 00:01:00 to 00:02:30
```

Or just describe what you want in natural language:

```
/ffhub-ffmpeg:ffmpeg make a 3-second gif starting at 10s: https://example.com/video.mp4
```

## How It Works

1. You describe the video/audio processing task
2. The AI generates the correct FFmpeg command
3. The command is submitted to FFHub.io cloud API
4. FFHub processes the file and returns download URLs

All processing happens in the cloud — fast, reliable, and no local dependencies.

## Requirements

- FFHub.io API key ([free tier available](https://ffhub.io))
- `curl` and `jq` (pre-installed on most systems)

## License

MIT
