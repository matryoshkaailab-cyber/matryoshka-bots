# ALEX CONNECTION BIBLE
## Статус: ONLINE (проверено 21.05.2026)

## Архитектура (ФАКТЫ, проверено)
```
HERMES (VPS) ←→ ws_server.py :8446 (HTTP :8450) ←→ ws_client.py (Windows) :4096 ←→ opencode serve (qwen3.6-plus-free)
```

**Что использует АЛЕКС:**
- НЕ OpenRouter — НЕ использует
- OpenCode Zen → qwen3.6-plus-free (локальная модель на Windows)
- ws_client.py v25 (чистый WebSocket, не websockets library)
- opencode serve на порту 4096 (Windows)

**Порты:**
- VPS ws_server: :8446 (WebSocket), :8450 (HTTP API)
- Windows opencode: :4096

**Токен:** hermes-ws-secret-2026

## Проверка связи
```bash
# VPS статус
curl -s http://127.0.0.1:8450/api/status
# Должно: {"alex_connected": true, ...}

# VPS здоровье
curl -s http://127.0.0.1:8450/api/health

# Тест задачи АЛЕКСУ
curl -s -X POST http://127.0.0.1:8450/api/delegate \
  -H "Content-Type: application/json" \
  -d '{"task_id":"test","text":"Return OK","timeout":20}'
```

## Процедура восстановления связи
1. `curl -s http://127.0.0.1:8450/api/status` → alex_connected?
2. Если false → ws_server упал → перезапусти:
   ```bash
   cd /root/matryoshka && python3 ws_server.py &
   ```
3. Проверь port: `ss -tlnp | grep 8446`
4. Проверь АЛЕКС на Windows: tasklist | findstr python

## Если АЛЕКС подключён но не отвечает
ALEX подключён (alex_connected: true) но задачи timeout → opencode serve завис.
Команда АЛЕКСУ: перезапустить opencode serve на Windows.

## Контакты
- VPS: webmaster@85.137.166.209
- Windows: C:\matryoshka\bots\alex\
- GitHub: matryoshkaailab-cyber/matryoshka-bots
- Obsidian: /root/vaults/matryoshka-bots/alex/