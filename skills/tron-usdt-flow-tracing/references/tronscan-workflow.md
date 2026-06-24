# Tronscan Workflow

Use public Tronscan data to verify TRON USDT/TRC20 transfers and downstream address activity.

## Starting Transaction

Given a Tronscan URL like:

```text
https://tronscan.org/transaction/<hash>/overview
```

Extract `<hash>` and verify:

- transaction status is successful and confirmed
- token is USDT/TRC20
- token contract is `TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t`
- amount is divided by `10^6`
- sender and receiver match the user's claim
- explorer labels are recorded as labels, not as identity proof

Useful public API shape:

```text
https://apilist.tronscanapi.com/api/transaction-info?hash=<hash>
```

Important fields often include:

- `timestamp`
- `block`
- `trc20TransferInfo`
- `tokenTransferInfo`
- `addressTag`
- `contractRet`
- `confirmed`

## Address Transfer History

Use the TRC20 transfer endpoint to inspect a relevant address:

```text
https://apilist.tronscanapi.com/api/token_trc20/transfers?limit=50&start=0&sort=-timestamp&count=true&relatedAddress=<address>
```

Notes:

- `limit` is commonly capped at 50.
- Page with `start=0,50,100,...` when the address has many transfers.
- Use `sort=-timestamp` for newest-first and `sort=timestamp` for oldest-first. Verify actual API behavior with sample output.
- Filter by USDT contract when the address has multiple tokens.
- Convert `quant` or `amount_str` with 6 decimals for USDT.

## Trace Heuristics

1. Start from the initial receiver and find outgoing USDT transfers after the deposit timestamp.
2. Prefer exact or near-time movements, but account for batching and aggregation.
3. For active hubs, compute or inspect the balance before the relevant incoming transfer when possible. If prior balance or other incoming funds exist, downgrade certainty to "highly related downstream flow."
4. Follow the earliest plausible outflow and any large outflow that appears to sweep or aggregate funds.
5. Stop and summarize when funds reach:
   - exchange hot wallets
   - bridge contracts
   - known service labels
   - highly active aggregation addresses

## Time Handling

Tronscan timestamps are usually milliseconds since Unix epoch. Convert to the user's timezone when reporting.

When the user is Chinese-speaking, Beijing time / China Standard Time is usually the clearest unless they request another timezone.

## Output Fields To Preserve

For each key hop, preserve:

- local time
- UTC time if useful
- transaction hash
- token and amount
- from address and tag
- to address and tag
- block
- confidence level
- Tronscan link
