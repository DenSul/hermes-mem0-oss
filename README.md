# mem0_oss — Self-Hosted Mem0 Memory Provider

Plugin for [Hermes Agent](https://github.com/NousResearch/hermes-agent) that connects to a **self-hosted Mem0 OSS server** instead of Mem0 Platform (cloud).

---

## Why this exists

Hermes already has a built-in `mem0` plugin. But it uses the `mem0ai` SDK which:
- Requires a Mem0 Platform API key
- Ignores `MEM0_BASE_URL` — always connects to `https://api.mem0.ai`
- Calls `/v2/...` endpoints that don't exist in self-hosted Mem0 OSS

**This plugin** calls your Mem0 OSS server directly via REST — no SDK, no cloud, no API key needed for the server itself.

---

## Features

| Operation | Method | Endpoint |
|-----------|--------|----------|
| Search memories | `POST` | `/v1/memories/search/` |
| User profile | `GET` | `/v1/memories/?user_id={user_id}` |
| Store fact | `POST` | `/v1/memories/` |

- Semantic search with relevance scores
- Thread-safe httpx client
- Circuit breaker on failures
- Background sync

---

## Installation

```bash
hermes plugins install DenSul/mem0-oss
```

---

## Configuration

In `~/.hermes/.env`:

```env
MEM0_BASE_URL=http://YOUR_MEM0_SERVER_IP:8420
MEM0_API_KEY=any_key
MEM0_USER_ID=denis
MEM0_AGENT_ID=hermes
```

Then:

```bash
hermes plugins enable mem0_oss
hermes gateway restart
```

---

## Mem0 OSS Server

Current server: `http://YOUR_MEM0_SERVER_IP:8420`

The server runs under a systemd service. To restart:

```bash
ssh root@YOUR_MEM0_SERVER_IP
systemctl restart mem0
journalctl -u mem0 -f
```

---

## Repository Structure

```
mem0-oss/
├── __init__.py    # Mem0OSSMemoryProvider
└── plugin.yaml    # Plugin manifest
```

---

## License

MIT
