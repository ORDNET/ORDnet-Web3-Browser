# Security Policy

## Reporting a vulnerability

Please report security issues privately first. Do not open a public issue
for anything that could put keys, names or funds at risk.

**Preferred channel:** [GitHub private vulnerability reporting](https://github.com/ORDNET/ORDnet-Web3-Browser/security/advisories/new)
— the "Report a vulnerability" button on the Security tab of this
repository. This creates a private advisory only the maintainers can see.

Please include what the issue is, where in `ORDnet_WEB3_Browser.html` it
lives, how to reproduce it, and what an attacker gains.

## What to expect

- **Acknowledgement:** within 3 working days.
- **Assessment:** within 10 working days, with a severity.
- **Credit:** we will name you in the release notes unless you prefer otherwise.

We do not currently operate a bug bounty.

## Threat model

This browser renders **attacker-controlled content by definition**: anything
anyone has ever inscribed on the chain can end up in a tab, one click away
from a key-holding wallet frame. Three boundaries carry the weight:

1. **On-chain content stays in its sandbox.** Inscribed HTML renders in a
   frame without `allow-same-origin`; a structural test keeps that
   attribute gone. Any way for rendered content to reach the host origin,
   the wallet frame, or another tab's state is a vulnerability.
2. **The wallet relay serves only what was declared.** Every RPC method
   maps to a required permission; undeclared and unknown methods fail
   closed, and replies go only to the window and origin that asked. Any
   path around the permission map — or any reply that reaches a window
   that did not ask — is a vulnerability.
3. **What loads is what was inscribed.** Content is addressed by txid;
   the data sources (WhatsOnChain, the ORDnet content router) are
   configuration, not trust. A way to make the browser render bytes other
   than the inscription the txid names is a vulnerability.

Out of scope: phishing content that plays by the rules (correctly
sandboxed, honestly addressed), availability of the public data sources,
and the companion wallet extension (report those against
[ORDnet-Chrome-Wallet](https://github.com/ORDNET/ORDnet-Chrome-Wallet)).

## Known history

The August 2026 external review scored this repository 2/10 — one file, no
tests, no mitigations — and found a full origin breakout: on-chain HTML
rendered with `allow-scripts allow-same-origin`, a cancelled sandbox one
`postMessage` from the wallet frame. That release also closed an inert
permission screen, wildcard reply broadcasting, and two same-cycle
regressions the structural checks could not see. All of it, with the
47-test suite that came out of it, is documented in
[SECURITY-FIXES-2026-08-13.md](SECURITY-FIXES-2026-08-13.md).
