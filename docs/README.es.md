# FFHub FFmpeg Skill

[English](../README.md) | [中文](./README.zh.md) | [日本語](./README.ja.md) | [Русский](./README.ru.md)

> Habilidad de agente IA para procesamiento FFmpeg en la nube — compatible con Claude Code, Cursor, Codex, OpenCode y [40+ agentes](https://github.com/vercel-labs/skills#supported-agents).

Procesa archivos de video y audio en la nube a través de la API de [FFHub.io](https://ffhub.io). No requiere instalación local de FFmpeg.

## Funcionalidades

| Funcionalidad | Descripción |
|---------------|-------------|
| **Convertir** | MP4, WebM, MKV, AVI, MOV, MP3, WAV, AAC, FLAC, etc. |
| **Comprimir** | Reducir el tamaño con control de calidad (CRF, bitrate) |
| **Recortar** | Cortar video/audio por rango de tiempo |
| **Redimensionar** | Escalar a cualquier resolución |
| **Extraer audio** | Extraer la pista de audio de un video |
| **Miniaturas** | Capturar fotogramas como imágenes (JPG, PNG, WebP) |
| **GIF** | Convertir clips de video en GIFs animados |
| **Subir archivos** | Subir archivos locales y procesarlos en la nube |

## Instalación

### Método 1: npx skills (recomendado)

Compatible con Claude Code, Cursor, Codex, OpenCode, Cline, Windsurf y [40+ agentes](https://github.com/vercel-labs/skills#supported-agents).

```bash
# Instalar en todos los agentes detectados
npx skills add ffhub-io/ffhub-ffmpeg

# Instalar en un agente específico
npx skills add ffhub-io/ffhub-ffmpeg -a claude-code
npx skills add ffhub-io/ffhub-ffmpeg -a cursor
npx skills add ffhub-io/ffhub-ffmpeg -a codex

# Instalación global (disponible en todos los proyectos)
npx skills add ffhub-io/ffhub-ffmpeg -g

# Ver habilidades disponibles antes de instalar
npx skills add ffhub-io/ffhub-ffmpeg --list
```

### Método 2: Plugin de Claude Code

```bash
claude /plugin install ffhub-io/ffhub-ffmpeg
```

## Configuración

1. Regístrate en [ffhub.io](https://ffhub.io)
2. Obtén tu API key en **Settings > API Keys**
3. Configura la variable de entorno:

```bash
export FFHUB_API_KEY=your_key_here
```

> **Consejo:** Agrégalo a tu archivo de configuración del shell (`~/.bashrc`, `~/.zshrc`) para que persista entre sesiones.

## Uso

Describe lo que quieres hacer en lenguaje natural:

```
/ffmpeg comprimir este video: https://example.com/video.mp4
/ffmpeg convertir https://example.com/video.mov a mp4
/ffmpeg extraer audio de https://example.com/video.mp4
/ffmpeg recortar https://example.com/video.mp4 de 00:01:00 a 00:02:30
/ffmpeg crear un gif de 3 segundos desde el segundo 10: https://example.com/video.mp4
/ffmpeg redimensionar https://example.com/video.mp4 a 1280x720
```

También puedes usar archivos locales — se subirán automáticamente:

```
/ffmpeg comprimir ./mi-video.mp4
/ffmpeg convertir ~/Downloads/grabacion.mov a mp4
```

## Cómo funciona

```
Describes la tarea
        ↓
La IA genera el comando FFmpeg
        ↓
Sube archivos locales (si es necesario)
        ↓
Envía a la API en la nube de FFHub.io
        ↓
FFHub procesa el archivo en la nube
        ↓
Devuelve URLs de descarga + información del archivo
```

Todo el procesamiento ocurre en la nube — rápido, confiable y sin dependencias locales.

## Formatos soportados

| Tipo | Formatos |
|------|----------|
| **Video** | `.mp4` `.webm` `.mkv` `.avi` `.mov` `.flv` |
| **Audio** | `.mp3` `.wav` `.aac` `.ogg` `.flac` `.m4a` |
| **Imagen** | `.gif` `.png` `.jpg` `.jpeg` `.webp` |

## Requisitos

- API key de [FFHub.io](https://ffhub.io) (nivel gratuito disponible)
- `curl` y `jq` (preinstalados en la mayoría de los sistemas)

## Gestión de habilidades

```bash
# Listar habilidades instaladas
npx skills list

# Actualizar a la última versión
npx skills update ffmpeg

# Desinstalar
npx skills remove ffmpeg
```

## Enlaces

- [FFHub.io](https://ffhub.io) — Servicio FFmpeg en la nube
- [Agent Skills Spec](https://agentskills.io) — Estándar abierto de habilidades de agentes
- [Directorio de Skills](https://skills.sh) — Descubre más habilidades
- [npx skills CLI](https://github.com/vercel-labs/skills) — Instalador universal de habilidades

## Licencia

MIT
