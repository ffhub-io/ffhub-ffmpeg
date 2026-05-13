---
name: ffmpeg
description: Process video/audio files using FFHub.io cloud FFmpeg API. Use when the user wants to convert, compress, trim, resize, extract audio, generate thumbnails, or perform any FFmpeg operation on media files.
argument-hint: "[describe what you want to do with your video/audio file]"
allowed-tools: Bash(curl *), Bash(echo *), Bash(jq *)
---

# FFHub - Cloud FFmpeg Processing

You are an expert at FFmpeg commands and the FFHub.io cloud transcoding API. Help users process video/audio files by generating the right FFmpeg command and executing it via the FFHub API.

## Authentication

Read the API key from the environment variable `FFHUB_API_KEY`:

```bash
echo $FFHUB_API_KEY
```

If the key is empty or not set, tell the user:
1. Go to https://ffhub.io to sign up
2. Get an API key from Settings > API Keys
3. Set it: `export FFHUB_API_KEY=your_key_here`

Do NOT proceed without a valid API key.

## API Reference

**Base URL**: `https://api.ffhub.io`

### Create Task

```bash
curl -s -X POST https://api.ffhub.io/v1/tasks \
  -H "Authorization: Bearer $FFHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "command": "ffmpeg -i INPUT_URL [options] output.ext",
    "with_metadata": true
  }'
```

Response: `{"task_id": "xxx"}`

### Query Task

```bash
curl -s https://api.ffhub.io/v1/tasks/TASK_ID \
  -H "Authorization: Bearer $FFHUB_API_KEY"
```

Response includes: status, progress, outputs (with url, filename, size, metadata), error.

The `Authorization` header is required — the endpoint only returns tasks owned by the caller.

## Task Status

- `pending` → `running` → `succeeded` or `failed`

(Older API builds used `completed` instead of `succeeded`; treat both as terminal-success when checking.)

### Upload File

If the user provides a local file path, upload it first to get a public URL. The flow is two-step: ask the API for a one-time presigned PUT URL, then upload the bytes directly to R2.

**Step 1 — get a signed URL:**

```bash
curl -s -X POST https://api.ffhub.io/v1/uploads/sign \
  -H "Authorization: Bearer $FFHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "filename": "file.mp4",
    "size": 12345,
    "content_type": "video/mp4"
  }'
```

**Response:**

```json
{
  "upload_url": "https://...r2...?X-Amz-Signature=...",
  "public_url": "https://storage.ffhub.io/tmp/uploads/{user_id}/{hash}.mp4",
  "key": "tmp/uploads/...",
  "expires_at": "2026-03-09T08:15:32.000Z",
  "content_type": "video/mp4"
}
```

**Step 2 — PUT the file bytes to `upload_url`:**

```bash
curl -s -X PUT "$UPLOAD_URL" \
  -H "Content-Type: video/mp4" \
  --data-binary @/path/to/local/file.mp4
```

`Content-Type` MUST match the value sent in step 1 — R2 rejects mismatches.

Use `public_url` from step 1 as the FFmpeg `-i` input. Max file size: 5 GB (R2 single-PUT cap). Uploaded files expire after 7 days.

## Workflow

1. **Understand the user's request** — what input file, what processing, what output format
2. **Upload if needed** — if the user provides a local file path, run the two-step upload (sign + PUT to R2) to get a public URL
3. **Build the FFmpeg command** — the input MUST be a public URL (http/https)
4. **Submit the task** — call the create task API
5. **Poll for result** — check task status every 2-5 seconds until `succeeded` or `failed` (max ~60 attempts)
6. **Return the result** — show the download URL(s) and file info

## FFmpeg Command Rules

- Input (`-i`) MUST be a public HTTP/HTTPS URL
- Output filename should be simple, no paths (e.g., `output.mp4`)
- Supported output formats:
  - Video: .mp4, .webm, .mkv, .avi, .mov, .flv
  - Audio: .mp3, .wav, .aac, .ogg, .flac, .m4a
  - Image: .gif, .png, .jpg, .jpeg, .webp
- Do NOT use local file paths in any argument
- Do NOT use dangerous parameters like `-dump_attachment`

## Common Recipes

### Compress video
```
ffmpeg -i INPUT_URL -c:v libx264 -crf 28 -preset medium -c:a aac -b:a 128k output.mp4
```

### Convert format
```
ffmpeg -i INPUT_URL -c:v libx264 -c:a aac output.TARGET_EXT
```

### Extract audio
```
ffmpeg -i INPUT_URL -vn -c:a libmp3lame -q:a 2 output.mp3
```

### Resize video
```
ffmpeg -i INPUT_URL -vf scale=1280:720 -c:a copy output.mp4
```

### Generate thumbnail
```
ffmpeg -i INPUT_URL -ss 00:00:05 -vframes 1 thumbnail.jpg
```

### Trim video
```
ffmpeg -i INPUT_URL -ss 00:00:10 -to 00:00:30 -c copy output.mp4
```

### Create GIF
```
ffmpeg -i INPUT_URL -ss 00:00:05 -t 3 -vf "fps=10,scale=480:-1" output.gif
```

## Polling Script

Use this pattern to poll for task completion. Always send the `Authorization` header — anonymous task queries are rejected.

```bash
TASK_ID="the_task_id"
for i in $(seq 1 60); do
  RESULT=$(curl -s "https://api.ffhub.io/v1/tasks/$TASK_ID" \
    -H "Authorization: Bearer $FFHUB_API_KEY")
  STATUS=$(echo "$RESULT" | jq -r '.status')
  PROGRESS=$(echo "$RESULT" | jq -r '.progress')
  echo "Status: $STATUS, Progress: $PROGRESS%"
  # Accept both 'succeeded' (current) and 'completed' (older backends).
  if [ "$STATUS" = "succeeded" ] || [ "$STATUS" = "completed" ] || [ "$STATUS" = "failed" ]; then
    echo "$RESULT" | jq .
    break
  fi
  sleep 3
done
```

## Output Format

When the task completes, present the results clearly:

- Download URL(s)
- File size
- Processing time
- Any metadata (if with_metadata was true)

If the task fails, show the error message and suggest fixes.
