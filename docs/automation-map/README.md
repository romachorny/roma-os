# Roma's Automation Map

The public one-page map of Roma OS: what runs by itself, what waits for Roma, and what a
client can order. It lives as a claude.ai artifact; this folder is its source.

- Live page: https://claude.ai/artifact/R5Qjar8hx7zmp4ZSt8dHHm
- Current banner: v17 (see `CHANGELOG.md`)

---

## Files

- `page.html` — the page as published. The artifact service adds the `<html>`/`<head>` wrapper,
  so this is the body only. The map image is embedded in it as a base64 PNG.
- `banner_3168x792.png` — the map image on the page: the banner at 2×, byte-identical to the one in `page.html`.
- `banner_1584x396.png` — the LinkedIn banner, 1×.
- `banner_1400x350.png` — Lanczos downscale of the LinkedIn banner.
- `CHANGELOG.md` — one line per change, newest first.

---

## Colour rules

The banner and the legend on the page use the same rules:

- **Green line** (`#46ffa0`) — live data: messages, leads, alerts.
- **Dashed green line** — code and deploys.
- **Short spokes** on Claude's ring — MCP links, services Claude uses directly.
- **White dashed box** (`#d6e2dd`) — the Hetzner server: the WhatsApp agent and the server browser.
- **Amber** (`#ffc861`) — waits for Roma: money, sending, deletion, access, publishing.
- **Shield** — no inbound ports on the server.

---

## Block order

1. Title — kicker, "This is my real automation", lead.
2. Map — the banner; scrolls sideways on phones.
3. "Five things wait for Roma" and "How to read the map", side by side.
4. Who builds this, and how.
5. Every line on the map.
6. Behind the map.
7. What a client can order, and how it runs by itself.
8. Try it — the live WhatsApp agent **+972 53 976 0820** first, then genvidpro.com, then github.com/romachorny.
9. Footer.

---

## Update rule

After any change visible outside or any change to the system (a button or service on
genvidpro.com, an agent, a service or MCP link, the server, webhook, prices, the agent's number,
the approval rules): update docs/automation-map, commit with 'map: <what changed>', add a
CHANGELOG line, republish the page to the same artifact URL. No secrets or personal records.
The LinkedIn banner changes only after Roma says so.

---

## Republishing

1. Edit `page.html` here, add the `CHANGELOG.md` line, commit as `map: <what changed>`.
2. Read the live page with the Artifact tool first (a publish to an artifact not read in the
   session is refused), then publish `docs/automation-map/page.html` with
   `url: https://claude.ai/artifact/R5Qjar8hx7zmp4ZSt8dHHm`.

A new banner comes only after Roma says so. Replace `banner_1584x396.png` and `banner_3168x792.png`,
then rebuild the 1400 × 350 copy and re-embed the 2× image in `page.html` from the repo root:

```python
import base64, re
from PIL import Image

d = "docs/automation-map/"
Image.open(d + "banner_1584x396.png").resize((1400, 350), Image.Resampling.LANCZOS).save(d + "banner_1400x350.png", optimize=True)
b64 = base64.b64encode(open(d + "banner_3168x792.png", "rb").read()).decode()
page = open(d + "page.html", encoding="utf-8").read()
page = re.sub(r"data:image/png;base64,[A-Za-z0-9+/=]+", lambda _: "data:image/png;base64," + b64, page, count=1)
open(d + "page.html", "w", encoding="utf-8").write(page)
```
