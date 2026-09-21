# Roma OS

A one person operations room. Scheduled agents watch every place where paid work appears, filter the noise, and push what is real into a single Telegram control room with a ready reply already written.

## The problem it solves

Two live client invitations sat unread on Upwork for half a month. Not because nobody looked, but because there is no single place where work arrives. Upwork, Fiverr, XPlace, Behance, LinkedIn, the site form, four inboxes. Roma OS is the answer to that: one room, one sound, one rule.

## How it works

```
platforms + mail  ->  scheduled agents  ->  filter  ->  Telegram topics  ->  reply ready to send
```

- **Watchers.** Hourly passes over Upwork invitations and messages, Fiverr, XPlace, Behance joblist, LinkedIn and mail, from 08:00 to 23:00 local.
- **Filter.** Mass agency outreach, no stated budget, no payment verified, no hire history — dropped silently. Silence is a valid answer.
- **Routing.** One Telegram supergroup, one topic per source and per product: Upwork, Fiverr, XPlace, Behance, LinkedIn, Site Builder, App Builder, Automation Builder, Video Production, Briefing, Billing, Commands.
- **Signal, not noise.** A red circle means it waits for a human answer and the message is pinned to the top of its topic. Yellow waits but does not burn. Green is information. A thumbs up unpins it.
- **Voice in.** Commands arrive as voice messages and are transcribed, because that is how the owner actually works.
- **Never on my behalf.** Agents write the reply, a human presses send. No proposal, no message, no post goes out automatically.

## Repository layout

```
.github/workflows/   scheduled relay: Telegram in and out, runs on GitHub servers
docs/                architecture notes and the rules the agents read
```

Credentials live in repository secrets, never in the tree. No client data, no order content and no personal records are committed here.

## Status

In daily use since September 2026.
