# ALEX CONNECTION BIBLE
## Статус: ONLINE (проверено 21.05.2026)
## ws_server: ws://127.0.0.1:8446 | HTTP API: http://127.0.0.1:8450 | token: hermes-ws-secret-2026

## Диагностика
- ws_server v6 запущен на VPS (port 8446, HTTP 8450)
- ALEX: ws_client v31, opencode serve stable
- OpenRouter: qwen3.6-plus-free

## Процедура восстановления связи:
1. Проверь статус: curl http://127.0.0.1:8450/api/status
2. Если alex_connected=false → ws_server.py упал
3. Перезапусти: cd /root/matryoshka && python3 ws_server.py &
4. Проверь port: ss -tlnp | grep 8446
5. Проверь WebSocket: python3 -c "import asyncio,websockets,json; asyncio.run(websockets.connect('ws://127.0.0.1:8446'))"
6. Проверь OpenRouter: curl -s https://openrouter.ai/api/v1/models | grep qwen

## Автоматический мониторинг (cron):
- */5 * * * * curl -s http://127.0.0.1:8450/api/status | grep alex_connected
- Если false 3 раза подряд → перезапусти ws_server и отправь alert в Telegram

## Контакты:
- VPS: webmaster@85.137.166.209
- Windows: C:\matryoshka\bots\alex\
- GitHub: matryoshkaailab-cyber/matryoshka-bots
- Obsidian: /root/vaults/matryoshka-bots/alex/

## Алерты:
- Если ALEX не отвечает > 3 минут → Telegram уведомление
- Если ws_server упал → перезапуск + уведомление