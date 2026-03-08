# FFHub FFmpeg Plugin

Cloud FFmpeg processing powered by [FFHub.io](https://ffhub.io).

Process video and audio files in the cloud — no local FFmpeg installation required.

## Features

- **Convert** — MP4, WebM, MKV, AVI, MOV, MP3, WAV, AAC, etc.
- **Compress** — Reduce file size with quality control
- **Trim** — Cut video/audio by time range
- **Resize** — Scale to any resolution
- **Extract Audio** — Pull audio track from video
- **Generate Thumbnails** — Capture frames as images
- **Create GIFs** — Convert video clips to animated GIFs

## Install

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

## Usage

```
/ffhub-ffmpeg:ffmpeg compress this video: https://example.com/video.mp4
/ffhub-ffmpeg:ffmpeg convert https://example.com/video.mov to mp4
/ffhub-ffmpeg:ffmpeg extract audio from https://example.com/video.mp4
/ffhub-ffmpeg:ffmpeg trim https://example.com/video.mp4 from 00:01:00 to 00:02:30
```

Or just describe what you want:

```
/ffhub-ffmpeg:ffmpeg make a 3-second gif starting at 10s: https://example.com/video.mp4
```

## Requirements

- FFHub.io API key (free tier available)
- `curl` and `jq` (available on most systems)

## License

MIT
