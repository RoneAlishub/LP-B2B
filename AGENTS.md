# AGENTS.md

## What this project is
A single static site: `index.html` (Hebrew, RTL, all CSS/JS inline) plus three
images it references — `hero.png`, `about.png`, `mall.png`. There is no build
step, no package manager, no backend and no database. Nothing here contacts a
local service; the page links out to Google Fonts, WhatsApp (`wa.me`) and
`spicybday.com` from the browser only.

## Running it in the Base44 sandbox
```bash
docker compose -f docker-compose.base44.yml up -d --build
```
- `nginx:alpine` serves the repo root bind-mounted at `/usr/share/nginx/html`.
- Host port **3000** is the web entry point (mapped to container port 80).
- `nginx.base44.conf` (mounted over `conf.d/default.conf`) sets
  `Cache-Control: no-store` so page edits are visible on a plain reload.

## Editing
There is no watcher/dev server — edits to `index.html` or the images are served
from disk on the next request. After a change, refresh the preview
(`reload_preview`) rather than expecting live reload.

## Verifying it works
```bash
curl -sI http://localhost:3000/            # expect 200 + text/html
curl -s  http://localhost:3000/ | head -5  # expect the <!DOCTYPE html> of index.html
curl -sI http://localhost:3000/hero.png    # expect 200 + image/png
docker compose -f docker-compose.base44.yml ps
```
The compose healthcheck already fetches `/` through `wget`.
