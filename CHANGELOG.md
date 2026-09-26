# Changelog

## 2026-09-26

- **WhatsApp sales agent: eight services from one registry.** `services.json` is the single source
  of truth for names, delivery times and starting prices; the site and the agent read the same
  file. Added Learn Claude and Build my OS. Asked what we do, the agent now lists all eight,
  inside an open brief as well as outside one. The reply follows the language of the client's last
  message, a voice note by the language of its transcript, and two languages never share one
  reply. New in `docs/automation-map.md`.
- **Automation map page: source in this repo.** `docs/automation-map/` holds the live page
  (`page.html`), the banner files and the rules for colours, block order and republishing;
  `CLAUDE.md` says when to update it.
- **Automation map page: banner v17.** No decorative rings, a white dashed Hetzner server
  boundary, amber "Roma approves", softer glow, larger text. The page shows it at 2×,
  3168 × 792; the LinkedIn banner files are unchanged.
- **Automation map page: order and the live agent.** Block order fixed; "Try it" opens with the
  live WhatsApp agent, +972 53 976 0820.
- **Automation map page: "Who builds this" rewritten.** Not a programmer; Cowork and Claude Code;
  Roma's own acceptance protocol, 17 tests and 9 blockers, built on τ-bench and the OWASP Top 10
  for LLM apps. Backups go to Roma's laptop, not a PC.
