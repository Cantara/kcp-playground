# KCP Playground

Twelve interactive, in-browser demo stations for the KCP "defendable agent" — each
runs real `kcp-agent`/`kcp-harness` decision code client-side and signs its verdict
with genuine `crypto.subtle`. Static site served via GitHub Pages: no build, no server.

**Read `knowledge.yaml` first.** It's the canonical agent-navigable manifest for this
repo and federates to the foundation specs this playground actually runs on:
`kcp-core`, `kcp-agent`, `kcp-harness`. Query it the standard KCP way, e.g.
`npx kcp-agent plan '<intent>' --manifest .`

For the shared conventions on how a governed skill unit should be authored
(`action_scope` as a firewall rule, `PROFILE.md`), see
[kcp-skill](https://github.com/Cantara/kcp-skill) — this repo does not vendor
kcp-skill's own skill library, only its authoring conventions.

**Local skills:** `skills/` — repo-specific procedures. Currently just `add-station`:
how to add a new demo station without breaking the numbering below.

## Gotchas

- Every `demos/<slug>/index.html` is self-contained and hardcodes its own
  position/total ("08 / 12") and prev/next nav links — nothing computes these.
  Adding or reordering a station means editing every file's numbering, not just
  `index.html`.
- `assets/shell.js`'s `KCP.printReceipt()` is the only place verdicts get genuinely
  signed (`crypto.subtle`, Ed25519 → ECDSA P-256 fallback) — don't hand-roll receipt
  markup in a new station.
- Two organs (memory, playbook-signing) are *faithfully modeled*, not the real
  decision code — a browser can't hold the project's signing keys. Don't claim
  otherwise in new copy.
- No build, no CI, no test suite here — verification for a change is opening the
  page in a browser and clicking through, not `npm test`.
