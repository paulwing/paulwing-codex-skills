---
name: tron-usdt-flow-tracing
description: Trace TRON USDT/TRC20 fund flows from a transaction hash or address, using public blockchain explorer data such as Tronscan. Use when the user asks to follow stolen, scammed, suspicious, or disputed USDT on TRON, identify downstream addresses, classify exchange/bridge/aggregation labels, prepare evidence summaries, or generate Mermaid fund-flow diagrams. Do not use for wallet recovery promises, private-key handling, evading compliance controls, or deanonymization beyond public labels.
---

# TRON USDT Flow Tracing

## Overview

Trace TRON USDT movements from a starting transaction or address, preserve evidentiary caution, and produce a concise report the user can use with law enforcement, exchanges, or internal compliance teams.

This skill is for public-chain analysis. Never ask for private keys, seed phrases, exchange passwords, 2FA codes, or wallet connection. Never promise recovery or freezing. Keep attribution limited to what the public chain and explorer labels support.

## Workflow

1. Collect the minimum case facts.
   - Ask for the transaction hash, chain, token, amount, known sender, known receiver, and approximate time if missing.
   - If the user provides a Tronscan transaction URL, extract the hash and verify it independently.
   - Confirm that the case is TRON USDT/TRC20 before using TRON-specific assumptions.

2. Verify the starting transaction.
   - Confirm status, timestamp, token contract, decimals, sender, receiver, amount, and explorer labels.
   - For USDT on TRON, expect contract `TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t` and 6 decimals.
   - Record the source link and avoid relying on screenshots alone.

3. Trace downstream transfers.
   - Query the receiver's TRC20 transfer history and sort around the relevant timestamp.
   - Follow same-token outflows first. Preserve the transaction hash, time, amount, from address, to address, block, and labels for each hop.
   - Page through explorer APIs when an address has many transfers. Do not infer from only the first page unless the result is clearly sufficient.
   - Stop at clear exchange hot wallets, bridge contracts, known services, or high-volume aggregation points unless the user asks to continue and public data supports it.

4. Classify confidence.
   - Use `Confirmed transfer` only for direct on-chain transfers between two addresses.
   - Use `Highly related downstream flow` when funds enter an account-model address that mixes balances before later outflows.
   - Use `Possible aggregation/service endpoint` for active hubs, exchange hot wallets, bridge addresses, or addresses with many unrelated flows.
   - Do not claim that every unit in a later mixed outflow is the user's money.
   - Read `references/evidence-standards.md` when the case involves mixed funds, legal wording, or user-facing evidence.

5. Produce the report.
   - Match the user's language. Use English for English users and Chinese for Chinese users.
   - Include a short conclusion first, then a table of key hops, then Mermaid visualization, then recommended next actions.
   - Include transaction links for every key hash.
   - For exchange endpoints, name the public label and address, and advise contacting the exchange or law enforcement with the evidence bundle.
   - Read `references/report-template.md` when preparing the final report.

## Tronscan Workflow

Use public explorer pages when possible. Use Tronscan API only as a convenience for structured public data.

Read `references/tronscan-workflow.md` before using Tronscan API endpoints, paging transfer history, converting amounts, or interpreting address tags.

## Visualization

Generate a Mermaid `flowchart LR` diagram for non-trivial paths.

- Use solid arrows for direct confirmed transfers.
- Use dotted arrows for mixed-account or confidence-limited downstream flows.
- Highlight exchange/service endpoints.
- Keep labels short enough to render cleanly.
- Use `assets/mermaid-template.mmd` as the starting template when helpful.

## Safety And Compliance

- Do not ask the user to connect a wallet.
- Do not request or store secrets.
- Do not instruct users to harass address owners, bypass exchange controls, or perform vigilante actions.
- Warn about recovery scams when appropriate.
- Phrase suspect labels carefully: use "reported by the user", "publicly labeled by Tronscan", "highly related", or "possible" instead of unsupported certainty.
