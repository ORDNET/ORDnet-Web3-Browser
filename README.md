# ORD/browser — the ORDnet Web3 Browser

[![tests](https://github.com/ORDNET/ORDnet-Web3-Browser/actions/workflows/test.yml/badge.svg)](https://github.com/ORDNET/ORDnet-Web3-Browser/actions/workflows/test.yml)
[![test count](https://img.shields.io/badge/tests-20_structural_+_27_behavioural-2b8a3e?style=flat-square)](#tests)
[![form](https://img.shields.io/badge/form-single_HTML_file-364fc7?style=flat-square)](#verify-your-copy)
[![distribution](https://img.shields.io/badge/also_lives-on--chain-5f3dc4?style=flat-square)](#verify-your-copy)
[![license](https://img.shields.io/badge/license-source--available-6a737d?style=flat-square)](LICENSE)

A complete web3 browser in **one HTML file**: type `name.web3` and browse
on-chain sites, files, and apps served straight from the BSV blockchain —
tabs, history, a viewer for on-chain PDF/Word/Excel/image/audio/video
content, an app launcher for the on-chain ORDnet tools, QR sharing, and a
developer mode. No install, no build, no server of its own: open the file
and you're browsing the chain.

**The browser itself lives on-chain.** This exact application is inscribed
on the BSV blockchain as 1Sat Ordinals — the code has been fully public
from day one by construction. This repository is its official,
discoverable home: the canonical source, published by its author.

## Try it

1. Download `ORDnet_WEB3_Browser.html`.
2. Open it in any modern browser (desktop or mobile — it installs as a
   home-screen web app on iOS/Android).
3. Type a web3 name — `ordnet.web3` — and go.

## What's inside

- **Name resolution** for the recognised TLD set (`web3, bitcoin, crypto,
  blockchain, ordnet` + resolve-only `bsv, bitcoinsv`), per the ODNCA
  standards ([ODNCA-standards](https://github.com/ORDNET/ODNCA-standards)).
- **On-chain content loading** — sites and files are 1Sat Ordinals
  inscriptions, fetched by txid and rendered in the browser: HTML, images,
  PDF, Word, Excel, audio, video.
- **Browser chrome** — tabs, history, bookmarks-style app grid, light/dark
  theme, security indicator, QR sharing of any on-chain address.
- **On-chain app launcher** — the ORDnet tool family (HTML creator,
  inscribers, QR generator, …), each itself an on-chain app opened by
  inscription id.
- **Developer mode** — inspect what you're looking at: txids, outpoints,
  content types.

Public chain data arrives via WhatsOnChain and the ORDnet content router;
both are configuration in the file's constants, not trust — the content is
addressed by txid, so what loads is what was inscribed.

## Verify your copy

A browser that handles on-chain identity deserves the same scrutiny it
applies to the chain. The application is one file, so its integrity is one
command:

```bash
sha256sum ORDnet_WEB3_Browser.html
# v1.4.0 -> cb45eab7fa69dfcd47e80cbfb49335b52178f56cf7733ea64771f06192022af4
```

The hash of the file is listed in the release notes of every release from
v1.4.0 on — check the copy you downloaded against the tag it came from, or
against the on-chain inscription itself: the same bytes, addressed by txid.

## Tests

```bash
node tests/structure-tests.mjs
# -> 20 passed, 0 failed
node tests/behaviour-tests.mjs
# -> 27 passed, 0 failed
```

Two suites, both on bare Node: structural assertions (the content sandbox
carries no `allow-same-origin`, the CSP contains no `unsafe-eval`, no
fabricated SRI hash ships) and a behavioural suite that lifts the RPC relay,
permission map and reply routing out of the file and executes them. See
[SECURITY-FIXES-2026-08-13.md](SECURITY-FIXES-2026-08-13.md) and
[SECURITY.md](SECURITY.md).

## License

**Source-available, not open source.** The code is published here (and
permanently on-chain) for transparency and audit — read it, verify it,
learn from how it works — but copying, modification, redistribution, and
use in other products require written permission from ORDnet. See
[LICENSE](LICENSE).

## Related

- [ORDnet-Chrome-Wallet](https://github.com/ORDNET/ORDnet-Chrome-Wallet) — ORD/plug, the companion wallet extension
- [ODNCA-standards](https://github.com/ORDNET/ODNCA-standards) — the naming standards this browser implements
- [ORDnet-SNS-client](https://github.com/ORDNET/ORDnet-SNS-client) — the standalone resolution library (MIT) for building your own
