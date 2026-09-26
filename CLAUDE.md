# CLAUDE.md

## Automation map

After any change visible outside or any change to the system (a button or service on genvidpro.com, an agent, a service or MCP link, the server, webhook, prices, the agent's number, the approval rules): update docs/automation-map, commit with 'map: <what changed>', add a CHANGELOG line, republish the page to the same artifact URL. No secrets or personal records. The LinkedIn banner changes only after Roma says so.

- Live page: https://claude.ai/artifact/R5Qjar8hx7zmp4ZSt8dHHm
- Page source, colour rules, block order, how to republish: `docs/automation-map/README.md`
- What each automation does, in prose: `docs/automation-map.md`
- Changelog: `CHANGELOG.md` in the repo root

## Where a spec lives

A spec the server works from lives where the server can read it: a repo or Google Drive.
A claude.ai artifact is a showcase, never a source. If a task links to something the server
cannot read, stop and ask for the file in one line; never wait for it and never write a
second version.

Why: on 26.09.2026 a task pointed at `docs/automation-map` in this repo and told the server to
wait while a cloud session created it. The session created a live artifact page instead of a
file. The server waited 50 minutes for a file that was never coming, then wrote its own version,
and there were two documents about one thing. The protocol the task referred to twice never
opened for the server at all (`read(...) → deny: access`), so a spec everyone treated as given
was invisible to the one executing it.
