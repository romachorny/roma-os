# Automation map

What runs by itself, where it runs, and what it reads. One section per automation.

Everything here executes on the server. Google Drive and the laptop hold mirrors, GitHub holds
history; none of the three is the source of truth. The one deliberate exception is the site
watchdog, which lives outside the box on purpose.

**The site deploys from roma-server; the laptop is a mirror.** genvidpro.com is published by
`ops/deploy.sh` in the server's own working tree: guards, commit, build the publish set with
`functions/` at its root, `wrangler pages deploy`, live checks, push. The laptop no longer
deploys. Before any deploy the tree is diffed against the live site, because a tree older than
live would silently roll the site back.

---

## WhatsApp sales agent

An n8n workflow on the server, behind a small gate that verifies Meta's signature before anything
else sees the payload. It answers on the business WhatsApp number in Hebrew, English, Russian and
Arabic, transcribes voice notes, walks a short qualifying brief, and hands the finished lead to
Airtable with an alert to Telegram.

### One registry, and no second copy

Everything the agent says about the work — the name, what is included, the delivery time and the
starting price — comes from a single file, `services.json`, served from the public site. The
site's own order builder reads that same file, so the page and the agent cannot name two different
prices. A change is picked up within ten minutes.

The copy of that file inside the agent's repository is a mirror: it drives the test suite and acts
as a last resort if the live file is unreachable. It is never the source of truth. Publishing a
price means publishing the registry, not editing the workflow.

### The eight services

| # | Service | From |
|---|---|---|
| 1 | Video ads | 3,500 ILS / $1,000 |
| 2 | Living paintings | 2,900 ILS / $850 |
| 3 | Website | 3,500 ILS / $1,000 |
| 4 | Logo and brand | 2,800 ILS / $800 |
| 5 | Your site as an app | 3,500 ILS / $1,000 |
| 6 | WhatsApp bot | 3,500 ILS + 199 ILS/mo |
| 7 | Learn Claude | 1,500 ILS / $450 |
| 8 | Build my OS | 6,500 ILS / $1,850 |

Asked what we do, the agent lists all eight and lets the client tap one. It lists all eight in the
middle of an open brief too: a question about us is a question, not an answer to the question on
screen.

### Rules that live in code, not in prompt text

- **Language follows the client.** The reply is written in the language of the client's last
  message; a voice note counts by the language of its transcript; two languages never share one
  reply.
- **Prices come from the registry.** Never invented, never discounted because someone asked, and
  the entry price is named before the ladder that ends at the most expensive option.
- **Card details never land anywhere.** The number is masked in the first node of the flow, before
  the engine, before the lead record, before any log. Expiry and security code are dropped.
- **Payment links come from the registry only.** A link pasted by a client is never repeated back,
  and with no registry link the answer is that the owner will send the official one.
- **Silence is never an outcome.** If an inbound message produced no reply and nothing chose that
  silence on purpose, a short line goes out in the client's language and a note goes to the owner.

### How it is graded

A written test protocol with named blockers, run by a human tester against the real number, plus a
generated suite that walks every service in every language. The suite runs against the engine in
process, and the blockers run again through the live path using reserved fake numbers that can
never reach a real person.

---

## Telegram bridge

A webhook, not polling, so a command lands in under a second. Voice is transcribed on the server
itself and never leaves it. The task runner executes one job at a time, queued, with a hard
timeout. The sender is checked in code before any model sees the message, so a prompt injection
cannot promote itself.

The group is a forum with one topic per direction: commands in one place, reports in another,
never a single pile.

---

## Scheduled jobs

Systemd timers in local time: a morning brief, an evening check, a mail watch, feed collection.
They report only when something needs a human. Quiet hours hold everything overnight and deliver
one message in the morning — a system that pings all night gets muted, and a muted system is worse
than none.

---

## Site watchdog

The one automation that deliberately does not run on the server: a GitHub Actions job checks the
site every twenty minutes. Anything that checks whether the server is alive has to live somewhere
else, or it dies with it.

---

## Task bridge

A private folder on Drive: a file dropped in the inbox starts a job on the server, and the result
comes back beside it. The folder is private, and that is the whole of its security model — if it
ever gains outside access the bridge is stopped, because a task from it runs with full rights.

---

## Backups and self-check

Nightly, encrypted, verified by a test extract, kept off the box. The self-check runs after every
reboot and stays silent when healthy. It exists because everything once looked correctly
configured for autostart and an actual reboot found a dependency cycle that had quietly kept a
whole service group down.
