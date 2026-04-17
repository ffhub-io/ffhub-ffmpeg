# FFHub FFmpeg Skill

[English](../README.md) | [日本語](./README.ja.md) | [Español](./README.es.md) | [Русский](./README.ru.md)

> AI 代理技能，用于云端 FFmpeg 处理 — 支持 Claude Code、Cursor、Codex、OpenCode 等 [40+ 代理工具](https://github.com/vercel-labs/skills#supported-agents)。

通过 [FFHub.io](https://ffhub.io) API 在云端处理视频和音频文件，无需本地安装 FFmpeg。

## 功能

| 功能 | 说明 |
|------|------|
| **格式转换** | MP4、WebM、MKV、AVI、MOV、MP3、WAV、AAC、FLAC 等 |
| **视频压缩** | 通过 CRF、码率控制来减小文件体积 |
| **视频裁剪** | 按时间范围剪切视频/音频 |
| **调整分辨率** | 缩放到任意分辨率 |
| **提取音频** | 从视频中提取音频轨道 |
| **生成缩略图** | 截取帧画面为图片（JPG、PNG、WebP） |
| **制作 GIF** | 将视频片段转换为 GIF 动图 |
| **上传文件** | 上传本地文件，然后在云端处理 |

## 安装

### 方式一：npx skills（推荐）

支持 Claude Code、Cursor、Codex、OpenCode、Cline、Windsurf 等 [40+ 代理工具](https://github.com/vercel-labs/skills#supported-agents)。

```bash
# 安装到所有已检测到的代理
npx skills add ffhub-io/ffhub-ffmpeg

# 安装到指定代理
npx skills add ffhub-io/ffhub-ffmpeg -a claude-code
npx skills add ffhub-io/ffhub-ffmpeg -a cursor
npx skills add ffhub-io/ffhub-ffmpeg -a codex

# 全局安装（所有项目可用）
npx skills add ffhub-io/ffhub-ffmpeg -g

# 安装前查看可用技能列表
npx skills add ffhub-io/ffhub-ffmpeg --list
```

### 方式二：Claude Code 插件

```bash
claude /plugin install ffhub-io/ffhub-ffmpeg
```

## 配置

1. 在 [ffhub.io](https://ffhub.io) 注册账号
2. 在 **Settings > API Keys** 页面获取 API Key
3. 设置环境变量：

```bash
export FFHUB_API_KEY=your_key_here
```

> **提示：** 将上述命令添加到 shell 配置文件（`~/.bashrc`、`~/.zshrc`）中，使其在所有终端会话中生效。

## 使用方法

用自然语言描述你想要的操作：

```
/ffmpeg 压缩这个视频: https://example.com/video.mp4
/ffmpeg 把 https://example.com/video.mov 转换为 mp4
/ffmpeg 提取音频: https://example.com/video.mp4
/ffmpeg 裁剪 https://example.com/video.mp4 从 00:01:00 到 00:02:30
/ffmpeg 从第 10 秒开始制作 3 秒的 gif: https://example.com/video.mp4
/ffmpeg 将 https://example.com/video.mp4 缩放到 1280x720
```

也支持本地文件 — 会自动上传：

```
/ffmpeg 压缩 ./my-video.mp4
/ffmpeg 转换 ~/Downloads/recording.mov 为 mp4
```

## 工作原理

```
描述你想要的操作
        ↓
AI 生成 FFmpeg 命令
        ↓
上传本地文件（如需要）
        ↓
提交到 FFHub.io 云端 API
        ↓
FFHub 在云端处理文件
        ↓
返回下载链接 + 文件信息
```

所有处理均在云端完成 — 快速、可靠，无需本地依赖。

## 支持的格式

| 类型 | 格式 |
|------|------|
| **视频** | `.mp4` `.webm` `.mkv` `.avi` `.mov` `.flv` |
| **音频** | `.mp3` `.wav` `.aac` `.ogg` `.flac` `.m4a` |
| **图片** | `.gif` `.png` `.jpg` `.jpeg` `.webp` |

## 系统要求

- [FFHub.io](https://ffhub.io) API Key（有免费额度）
- `curl` 和 `jq`（大多数系统已预装）

## 技能管理

```bash
# 查看已安装的技能
npx skills list

# 更新到最新版本
npx skills update ffmpeg

# 卸载
npx skills remove ffmpeg
```

## 相关链接

- [FFHub.io](https://ffhub.io) — 云端 FFmpeg 服务
- [Agent Skills 规范](https://agentskills.io) — 开放代理技能标准
- [Skills 目录](https://skills.sh) — 发现更多技能
- [npx skills CLI](https://github.com/vercel-labs/skills) — 通用技能安装工具

## 许可证

MIT
