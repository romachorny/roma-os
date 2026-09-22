# Roma OS

A one-person operations system. Everything runs on my own server, around the clock.
Telegram is the only control surface — including voice. My laptop is not part of the loop.

**Status:** in daily use since September 2026.

---

## What it is

I run AI video production and small web products alone. The work arrives from many places
at once — marketplaces, email, the website, WhatsApp — and it used to arrive as noise.
Roma OS turns that into one conversation: I speak into Telegram, the server does the work,
the answer comes back in the topic where it belongs.

Not a chatbot. An operations layer with hard boundaries, backups and a self-check.

---

## Architecture

```
            voice / text
                 │
            Telegram  ──────────────  the only control surface
                 │
        Cloudflare Tunnel            named tunnel, fixed address
                 │
         ┌───────▼────────┐
         │  own server    │          runs 24/7, no laptop involved
         │                │
         │  webhook  ─────┼──► speech-to-text  (local, nothing leaves the box)
         │     │          │
         │     ▼          │
         │  Claude Code ──┼──► executes the task
         │     │          │
         │   n8n  ────────┼──► business flows: WhatsApp agent, lead capture, relays
         │     │          │
         │  headless Chrome           persistent logged-in browser
         └─────┬──────────┘
               │
     Google Drive ◄──► laptop        two parallel mirrors, not the source of truth
```

**Key inversion:** the server is the executor, not a helper. Cloud schedulers and the laptop
were both removed from the critical path — each was a single point of failure.

---

## Components

| Piece | What it does |
|---|---|
| **Telegram bridge** | Webhook, not polling. Commands land in under a second. |
| **Speech-to-text** | whisper.cpp on the server. Voice never leaves the machine and costs nothing per minute. |
| **Task runner** | Claude Code headless, one task at a time, queued, hard timeout. |
| **n8n** | Business flows: WhatsApp agent, demo chat, relays, typing keeper. Pinned version. |
| **Cloudflare Tunnel** | Named tunnel on my own domain. No open inbound ports. |
| **Scheduled jobs** | Morning brief, evening check, mail watch — systemd timers in local time. |
| **Mirrors** | Google Drive and the laptop hold copies. Neither is the source of truth. |
| **Backups** | Nightly, encrypted, verified by test-extract, kept off-box. |
| **Self-check** | Runs after every reboot, reports itself, stays silent when healthy. |

---

## Control surface

The Telegram group is a forum. **One topic per direction**, so nothing lands in a general pile:

- **Marketplaces** — one topic per platform, incoming orders and threads
- **Products** — one topic per product line, requests from the site
- **Personal tracks** — long-running matters with deadlines
- **Commands** — where I speak; everything else is where I read
- **Server** — reports, self-checks, alarms

I send a voice message to Commands. Within a second I see 👀, then *typing…* while it works,
then the answer as a reply in the same topic.

---

## Design decisions worth stating

**Guardrails are configuration, not prompt text.** "Don't do X" written in a prompt is a
suggestion a model can be talked out of. The real limits live in settings and in code:
denied domains, denied commands, sender checked before the model ever sees the message.

**The sender check happens outside the model.** Chat and user identity are verified in code,
before any LLM call. A prompt injection cannot promote itself.

**Answer the webhook first, work second.** Telegram retries anything slow, and a retried
command is a command executed twice.

**Deletion is not propagated.** Sync refuses to delete rather than mirror a deletion, and
says so. Overwritten versions are kept. A sync that stops loudly beats one that quietly erases.

**Silence is the healthy state.** Jobs report only when something needs a human. Quiet hours
hold everything overnight and deliver one message in the morning. A system that pings all
night gets muted, and a muted system is worse than none.

**Verify by reboot, not by inspection.** Everything looked correctly configured for autostart.
An actual reboot found a dependency cycle that had silently kept a whole service group down.

---

## Security posture

- Secrets live outside the repository and outside synced folders. Nothing sensitive is committed.
- Tokens are never passed as command arguments — they would be visible in the process list.
- No inbound ports open to the internet beyond SSH; internal services are reachable only
  over a private network.
- Backup archives are encrypted; the passphrase is held separately from the backups.
- Client data and personal records are not in this repository and never will be.

---

## What this is not

Not a product, not a framework, not for sale. It is one person's operations system, published
because the architecture is more interesting than the code, and because most write-ups of
"personal AI assistants" stop at the demo and never mention backups, reboots or what happens
at three in the morning.
