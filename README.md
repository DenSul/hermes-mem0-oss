# mem0_oss — Self-Hosted Mem0 Memory Provider

Плагин для [Hermes Agent](https://github.com/NousResearch/hermes-agent). Позволяет использовать **свой сервер Mem0 OSS** вместо облака Mem0 Platform.

---

## Зачем

Hermes уже имеет встроенный плагин `mem0`. Но он использует `mem0ai` SDK, который:
- Требует API ключ Mem0 Platform
- Игнорирует `MEM0_BASE_URL` — всегда стучится на `https://api.mem0.ai`
- Вызывает эндпоинты `/v2/...` которых нет в self-hosted версии

**Этот плагин** работает напрямую через REST API твоего сервера — без SDK, без облака.

---

## Что умеет

| Операция | Метод | Эндпоинт |
|----------|-------|----------|
| Поиск по памяти | `POST` | `/v1/memories/search/` |
| Профиль пользователя | `GET` | `/v1/memories/?user_id={user_id}` |
| Сохранить факт | `POST` | `/v1/memories/` |

- Поиск с релевантностью и порогом
- Тредобезопасный httpx клиент
- Circuit breaker при сбоях
- Фоновая синхронизация

---

## Установка

```bash
hermes plugins install DenSul/mem0-oss
```

---

## Настройка

В `~/.hermes/.env`:

```env
MEM0_BASE_URL=http://YOUR_MEM0_SERVER_IP:8420
MEM0_API_KEY=любой_ключ
MEM0_USER_ID=denis
MEM0_AGENT_ID=hermes
```

```bash
hermes plugins enable mem0_oss
hermes gateway restart
```

---

## Сервер Mem0 OSS

Текущий сервер: `http://YOUR_MEM0_SERVER_IP:8420`

На сервере работает systemd-служба, uvicorn на порту 8420. Для перезапуска:

```bash
ssh root@YOUR_MEM0_SERVER_IP
systemctl restart mem0
journalctl -u mem0 -f
```

---

## Структура репозитория

```
mem0-oss/
├── __init__.py    # Mem0OSSMemoryProvider
└── plugin.yaml    # Манифест плагина
```

---

## Лицензия

MIT
