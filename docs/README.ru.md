# FFHub FFmpeg Skill

[English](../README.md) | [中文](./README.zh.md) | [日本語](./README.ja.md) | [Español](./README.es.md)

> Навык AI-агента для облачной обработки FFmpeg — совместим с Claude Code, Cursor, Codex, OpenCode и [40+ агентами](https://github.com/vercel-labs/skills#supported-agents).

Обработка видео и аудио файлов в облаке через API [FFHub.io](https://ffhub.io). Локальная установка FFmpeg не требуется.

## Возможности

| Возможность | Описание |
|-------------|----------|
| **Конвертация** | MP4, WebM, MKV, AVI, MOV, MP3, WAV, AAC, FLAC и др. |
| **Сжатие** | Уменьшение размера файла с контролем качества (CRF, битрейт) |
| **Обрезка** | Вырезка фрагмента видео/аудио по временному диапазону |
| **Изменение размера** | Масштабирование до любого разрешения |
| **Извлечение аудио** | Извлечение аудиодорожки из видео |
| **Миниатюры** | Захват кадров в виде изображений (JPG, PNG, WebP) |
| **GIF** | Преобразование видеоклипов в анимированные GIF |
| **Загрузка файлов** | Загрузка локальных файлов с последующей обработкой в облаке |

## Установка

### Способ 1: npx skills (рекомендуется)

Совместим с Claude Code, Cursor, Codex, OpenCode, Cline, Windsurf и [40+ агентами](https://github.com/vercel-labs/skills#supported-agents).

```bash
# Установить для всех обнаруженных агентов
npx skills add ffhub-io/ffhub-ffmpeg

# Установить для конкретного агента
npx skills add ffhub-io/ffhub-ffmpeg -a claude-code
npx skills add ffhub-io/ffhub-ffmpeg -a cursor
npx skills add ffhub-io/ffhub-ffmpeg -a codex

# Глобальная установка (доступно во всех проектах)
npx skills add ffhub-io/ffhub-ffmpeg -g

# Просмотр доступных навыков перед установкой
npx skills add ffhub-io/ffhub-ffmpeg --list
```

### Способ 2: Плагин Claude Code

```bash
claude /plugin install ffhub-io/ffhub-ffmpeg
```

## Настройка

1. Зарегистрируйтесь на [ffhub.io](https://ffhub.io)
2. Получите API-ключ в разделе **Settings > API Keys**
3. Установите переменную окружения:

```bash
export FFHUB_API_KEY=your_key_here
```

> **Подсказка:** Добавьте эту строку в конфигурацию вашего шелла (`~/.bashrc`, `~/.zshrc`), чтобы она сохранялась между сессиями.

## Использование

Опишите задачу на естественном языке:

```
/ffmpeg сжать это видео: https://example.com/video.mp4
/ffmpeg конвертировать https://example.com/video.mov в mp4
/ffmpeg извлечь аудио из https://example.com/video.mp4
/ffmpeg обрезать https://example.com/video.mp4 с 00:01:00 по 00:02:30
/ffmpeg создать gif 3 секунды начиная с 10-й секунды: https://example.com/video.mp4
/ffmpeg изменить размер https://example.com/video.mp4 до 1280x720
```

Также можно использовать локальные файлы — они будут загружены автоматически:

```
/ffmpeg сжать ./my-video.mp4
/ffmpeg конвертировать ~/Downloads/recording.mov в mp4
```

## Как это работает

```
Вы описываете задачу
        ↓
ИИ генерирует команду FFmpeg
        ↓
Загрузка локальных файлов (при необходимости)
        ↓
Отправка в облачный API FFHub.io
        ↓
FFHub обрабатывает файл в облаке
        ↓
Возвращает ссылки для скачивания + информация о файле
```

Вся обработка происходит в облаке — быстро, надёжно, без локальных зависимостей.

## Поддерживаемые форматы

| Тип | Форматы |
|-----|---------|
| **Видео** | `.mp4` `.webm` `.mkv` `.avi` `.mov` `.flv` |
| **Аудио** | `.mp3` `.wav` `.aac` `.ogg` `.flac` `.m4a` |
| **Изображения** | `.gif` `.png` `.jpg` `.jpeg` `.webp` |

## Требования

- API-ключ [FFHub.io](https://ffhub.io) (есть бесплатный тариф)
- `curl` и `jq` (предустановлены на большинстве систем)

## Управление навыками

```bash
# Список установленных навыков
npx skills list

# Обновить до последней версии
npx skills update ffmpeg

# Удалить
npx skills remove ffmpeg
```

## Ссылки

- [FFHub.io](https://ffhub.io) — Облачный сервис FFmpeg
- [Agent Skills Spec](https://agentskills.io) — Открытый стандарт навыков агентов
- [Каталог Skills](https://skills.sh) — Больше навыков
- [npx skills CLI](https://github.com/vercel-labs/skills) — Универсальный установщик навыков

## Лицензия

MIT
