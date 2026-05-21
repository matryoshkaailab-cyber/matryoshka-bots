# ALEX Connection Monitor & Recovery

## Проблема (зафиксировано 21.05.2026)
ALEX подключён (alex_connected: true) но НЕ отвечает на задачи через ws_server. HTTP API работает, ws_server работает, но ALEX не возвращает результаты.

## Симптомы
- ws://127.0.0.1:8446 — соединение установлено
- curl http://127.0.0.1:8450/api/status → alex_connected: true
- Но задачи через /api/delegate возвращают timeout
- Значит ALEX получает задачи но НЕ отправляет ответы обратно

## Диагностика на ALEX (Windows)
1. Проверь ws_client.py — запущен ли процесс
2. Проверь ws_server.py на Windows — порт 8446
3. Логи ws_client: C:\matryoshka\bots\alex\logs\
4. Тест: запусти ws_client.py вручную, отправь ping

## Восстановление на VPS
```bash
# Проверка ws_server
ss -tlnp | grep 8446
ps aux | grep ws_server

# Перезапуск если нужно
cd /root/matryoshka && python3 ws_server.py &
```

## Восстановление на Windows (ALEX)
```powershell
# Проверь процесс ws_client
tasklist | findstr python

# Перезапусти ws_client
python C:\matryoshka\ws_client.py

# Или PowerShell
Start-Process python -ArgumentList "C:\matryoshka\ws_client.py" -WindowStyle Hidden
```

## Автоматический мониторинг (cron на VPS)
```
*/5 * * * * curl -s http://127.0.0.1:8450/api/status | grep -q '"alex_connected": true' || (cd /root/matryoshka && python3 ws_server.py &)
```

## FIX: Добавить heartbeat в ws_client (ALEX)
ws_client.py должен отправлять heartbeat каждые 30 секунд, чтобы ws_server знал что он alive.

## FIX: Добавить auto-reconnect
ws_client.py должен переподключаться автоматически если соединение потеряно.