---
name: ffmpeg
description: Process video/audio files using FFHub.io cloud FFmpeg API. Use when the user wants to convert, compress, trim, resize, extract audio, generate thumbnails, or perform any FFmpeg operation on media files.
argument-hint: "[describe what you want to do with your video/audio file]"
allowed-tools: Bash(curl *), Bash(echo *), Bash(jq *), Bash(sleep *)
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
2. Get an API key from Dashboard > API Keys
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

## Credits

Tasks cost credits, reserved when the task is created and settled from actual processing time.

If create-task returns **HTTP 402** (`insufficient credits`), stop and tell the user to top up at
https://ffhub.io/pricing — retrying will not help. To check the balance first:

```bash
curl -s https://api.ffhub.io/v1/me -H "Authorization: Bearer $FFHUB_API_KEY" | jq '.available_credits'
```

## Upload File

If the user provides a local file path, upload it first to get a public URL. The flow is two-step: ask the API for a one-time presigned PUT URL, then upload the bytes directly to R2.

**Step 1 — get a signed URL:**

```bash
curl -s -X POST https://api.ffhub.io/v1/uploads/sign \
  -H "Authorization: Bearer $FFHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"filename": "file.mp4"}'
```

Only `filename` is required — the extension decides the content type. Pass `content_type`
explicitly only when the extension is misleading.

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

`Content-Type` MUST match the `content_type` returned in step 1 — R2 rejects mismatches.

Use `public_url` from step 1 as the FFmpeg `-i` input. Max file size: 5 GB (R2 single-PUT cap). Uploaded files expire after 7 days.

## Workflow

1. **Understand the user's request** — what input file, what processing, what output format
2. **Upload if needed** — if the user provides a local file path, run the two-step upload (sign + PUT to R2) to get a public URL
3. **Build the FFmpeg command** — the input MUST be a public URL (http/https)
4. **Submit the task** — call the create task API (HTTP 402 means out of credits, see above)
5. **Poll for result** — check task status every 2-5 seconds until `succeeded` or `failed` (max ~60 attempts)
6. **Return the result** — show the download URL(s) and file info

## FFmpeg Command Rules

- Input (`-i`) MUST be a public HTTP/HTTPS URL — `localhost` and private/internal IPs are rejected
- Output filename should be simple, no paths (e.g., `output.mp4`)
- No shell operators (`|`, `&&`, `;`, `>`, `<`) — the command is parsed, not run through a shell
- Filters may reference remote files only: `movie=https://...` is fine, a local path is not
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
  if [ "$STATUS" = "succeeded" ] || [ "$STATUS" = "failed" ]; then
    echo "$RESULT" | jq .
    break
  fi
  sleep 3
done
```

This gives up after ~3 minutes, but the task keeps running server-side (the backend allows up to
an hour). If the loop ends while the task is still `pending` or `running`, do NOT report a
failure — give the user the `task_id` and the query command so they can check back later.

## Output Format

When the task completes, present the results clearly:

- Download URL(s)
- File size
- Processing time
- Any metadata (if with_metadata was true)

If the task fails, show the error message and suggest fixes.
