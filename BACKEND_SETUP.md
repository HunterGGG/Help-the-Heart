# Help the Heart ❤️ — Leaderboard Backend Setup

## 1) Install dependencies

```bash
npm install
```

## 2) Run locally

```bash
npm start
```

Server starts on:

- `http://localhost:3000`

API endpoints:

- `GET /api/leaderboard`
- `POST /api/score`

Healthcheck:

- `GET /health`

## 3) SQLite schema

Schema is in `schema.sql` and auto-applied at startup to `leaderboard.db`.

## 4) Frontend integration

Frontend uses same-origin API by default (`/api/*`).

If backend is on another domain, set before game script runs:

```html
<script>
  window.LEADERBOARD_API_BASE = "https://your-api-domain.com";
</script>
```

## 5) Deploy notes

### Render / Railway

- Build command: `npm install`
- Start command: `npm start`
- Environment variables (optional):
  - `PORT` (set by platform automatically)
  - `DB_PATH` (default: `./leaderboard.db`)

### VPS (Ubuntu example)

```bash
npm install
npm start
```

Use `pm2` or `systemd` for process management and add Nginx reverse proxy to expose HTTPS.

## 6) Security / anti-cheat implemented

- Nickname validation: 2–18 chars
- Nickname sanitization to prevent script injection
- Score validation: positive integer only
- Score hard cap: `<= 1,000,000`
- 1 record per `deviceId`
- Update record only if new score is higher
- Rate limit on submissions: max 5 requests / minute / IP
- CORS enabled
