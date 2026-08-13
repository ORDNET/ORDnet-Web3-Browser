# Changelog — ORD/browser (ORDnet Web3 Browser)

All notable changes to the single-file web3 browser.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
The application ships as one HTML file; versions here are repository
versions, matched by the release tags.

---

## [1.1.0] — 2026-08-13 — external security audit

The review scored this repository 2/10 — the lowest in the organisation —
and it was right to: the file had no tests and no mitigations. Full detail
in [SECURITY-FIXES-2026-08-13.md](SECURITY-FIXES-2026-08-13.md).

### Security

- **Full origin breakout next to a key-holding wallet frame.** On-chain HTML
  rendered in a `srcdoc` iframe with `allow-scripts allow-same-origin` — a
  cancelled sandbox in the host origin, one `postMessage` away from the
  wallet frame. `allow-same-origin` is gone; a structural test keeps it gone.
- **Attacker-controlled content type into `innerHTML`.** Both call sites now
  route through `escapeHtml()`.
- **A permission screen that gated nothing.** Declared permissions were
  displayed, stored, and never consulted. `METHOD_PERMISSION` now maps every
  RPC method to a required permission; the relay refuses anything the plugin
  did not declare; unknown methods fail closed.
- **Wallet replies broadcast to any window.** Responses (addresses,
  signatures) went out with `targetOrigin: '*'`. Replies now go to the
  window and origin that actually asked, recorded per request id; the one
  remaining `'*'` targets the opaque-origin content frame and carries no data.
- **CSP added, SRI prepared.** `script-src` limited to `'self'` and two named
  CDNs, no `unsafe-eval`; `'unsafe-inline'` remains and is stated plainly
  (the app is one inline script). `integrity` attributes are deliberately
  empty — run `tools/generate-sri.sh` and paste real hashes; a fabricated
  hash silently blocks the script it guards.

### Fixed

- Two regressions from this same fix cycle, both of the kind the structural
  suite could not see: `replyTarget()` treated the opaque-origin string
  `"null"` as a real origin, silently dropping **every** wallet response;
  and the permission map invented `wallet:write`/`wallet:sign` — a
  vocabulary nothing else in the file uses — refusing pay, inscribe and sign
  for every normally installed wallet. A permission model that blocks the
  happy path is a broken product, not a safe one.

### Tests

- 47 in total: 20 structural assertions (no `allow-same-origin` on the
  content frame, content type escaped, no `unsafe-eval`, no fabricated SRI)
  **plus a 27-test behavioural suite** that lifts the relay, permission map
  and reply routing out of the file and executes them against a simulated
  window — added because both regressions above kept the code *looking*
  right while the behaviour was broken. Structure proves a pattern exists;
  behaviour proves it works.

---

## [1.0.0] — initial public release

The complete browser in one HTML file, inscribed on-chain as 1Sat Ordinals:
name resolution for the recognised TLD set, on-chain content loading by
txid, tabs/history/app-grid chrome, viewers for PDF/Word/Excel/image/
audio/video, QR sharing, and a developer mode.
