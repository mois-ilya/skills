---
name: ton-docs
description: >
  Searches and references official TON blockchain documentation, standards (TEPs), and SDK guides
  by reading the public docs over HTTP. Use this skill when answering questions about TON architecture,
  smart contracts, FunC, Tact, Tolk, TL-B schemas, validators, sharding, message routing, wallet
  contracts, jetton standard (TEP-74), NFT standard (TEP-62), TON addresses, BOC encoding, or any
  TON protocol details.
license: MIT
metadata:
  author: ton-tech
  version: "0.3.0"
---

# TON Documentation

Search and reference the official TON blockchain documentation. The docs are public HTTP, indexed by
`llms.txt`, so any agent with a web-fetch tool can read them directly — no MCP server or other hosted
dependency is required.

## Doc URLs

| Resource | URL |
| -------- | --- |
| Documentation index (`llms.txt`) | `https://docs.ton.org/llms.txt` |
| Raw Markdown for a page | `https://docs.ton.org/llms/<path>/content.md` |
| Human-readable page | `https://docs.ton.org/<path>` |

`llms.txt` is a nested list of Markdown links, each already a full `https://docs.ton.org/llms/<path>/content.md`
URL — fetch it as-is, no prefixing needed. When citing sources back to the user, link the human-readable
page URL (drop the `llms/` prefix and the `/content.md` suffix), e.g. `https://docs.ton.org/tolk/overview`.

## When to Use

- User asks about TON blockchain concepts (workchains, sharding, validators, masterchain)
- User asks about smart contract development (FunC, Tact, Tolk, Fift, TL-B schemas)
- User asks about TON standards (TEP, jetton standard TEP-74, NFT standard TEP-62)
- User asks about wallet contracts, message formats, or internal/external messages
- User asks about TON addresses, BOC encoding, cell serialization
- User asks about DeFi protocols, DEX, or token mechanics on TON
- User needs code examples for TON smart contracts or SDK usage
- User asks about TON APIs (TonCenter v2/v3), node operations, or infrastructure

## Workflow

1. Identify the topic and its sub-aspects from the user's question.
2. Fetch the index once: `https://docs.ton.org/llms.txt`. Scan it for entries whose titles/paths match
   the topic and its sub-aspects (e.g. one core concept page + one SDK/tooling page).
3. Select the 2–4 most relevant page paths from the index.
4. Fetch those pages' raw Markdown **in parallel** using the exact URLs found in the index
   (e.g. `https://docs.ton.org/llms/<path>/content.md`).
5. If the pages reference other relevant pages (e.g. a "See also" section), fetch those too.
6. Synthesize the answer from all fetched pages, include code examples, and link to sources using the
   human-readable page URLs (`https://docs.ton.org/<path>`).

## Gotchas

- Use `llms.txt` to discover paths rather than guessing them — the index is the source of truth for
  what pages exist and how they are named.
- A good answer typically requires reading 3–5 pages; a single page rarely covers the full picture.
- For smart contract questions, determine whether the user is working with FunC or Tolk before
  providing examples.
- For SDK questions, determine the language (TypeScript/JavaScript via `@ton/ton`, Python via
  `pytoniq`, etc.).
- TON docs are regularly updated — always fetch fresh content rather than relying on cached knowledge.

## TEPs (TON Enhancement Proposals)

For protocol standards not covered by the docs site, TEPs are on GitHub:

| TEP | Topic | URL |
| --- | ----- | --- |
| TEP-62 | NFT Standard | `https://github.com/ton-blockchain/TEPs/blob/master/text/0062-nft-standard.md` |
| TEP-64 | Token Data Standard | `https://github.com/ton-blockchain/TEPs/blob/master/text/0064-token-data-standard.md` |
| TEP-74 | Jetton Standard | `https://github.com/ton-blockchain/TEPs/blob/master/text/0074-jettons-standard.md` |
| TEP-81 | DNS Standard | `https://github.com/ton-blockchain/TEPs/blob/master/text/0081-dns-standard.md` |
| All TEPs | Index | `https://github.com/ton-blockchain/TEPs/blob/master/README.md` |
