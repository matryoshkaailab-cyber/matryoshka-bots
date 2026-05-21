# MATRYOSHKA BOTS — Структура ботов

## Боты
- **hermes** — Дирижёр Оркестра (VPS, MiniMax-M2.7)
- **alex** — Технический инженер (Windows ПК, opencode)
- **alisa** — Маркетинг / Контент (@AlisaMatryBot)
- **alina** — Ассистент (Telegram бот)
- **alfa** — Стратег (VPS, MVP)

## Структура каждого бота
```
bots/<bot>/
├── config/    # Конфигурации, промпты, настройки
├── memory/    # Память, контекст, знания
├── tasks/     # Задачи, очереди, результаты
├── logs/      # Логи работы
└── projects/  # Проектные файлы, документация
```

## Синхронизация
- Windows: `C:\matryoshka\bots\`
- Obsidian: `OBSIDIAN\bots\`
- GitHub: `matryoshka-digital/bots/`

---
*MATRYOSHKA DIGITAL — 2026-05-21*
