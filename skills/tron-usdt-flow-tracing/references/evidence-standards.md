# Evidence Standards

Use this reference when preparing a TRON USDT tracing report or deciding how strongly to word a downstream flow.

## Classification Levels

### Confirmed transfer

Use when a transaction directly transfers the tracked token from one address to another.

Record:

- transaction hash
- timestamp with timezone
- token and amount
- from address
- to address
- block number
- explorer label, if any

Wording:

- "Address A transferred 4,092 USDT to address B."
- "Tronscan labels address A as Binance-Hot 11."

### Highly related downstream flow

Use when the tracked funds enter an account-model address that also holds or receives other funds before later outflows.

TRON uses an account model. Once USDT enters an address, it mixes with the address balance. Later outgoing transfers can be strongly relevant but cannot be described as containing the exact original units unless the balance history makes that conclusion unavoidable.

Wording:

- "After receiving the funds, this address later sent 8,806 USDT to address C."
- "Because the address had mixed activity, this is a highly related downstream outflow, not a unit-level attribution."

Avoid:

- "The stolen 4,092 USDT was definitely inside this 8,806 USDT transfer."
- "This proves the exchange account owner is the scammer."

### Possible aggregation or service endpoint

Use for high-volume addresses, exchange hot wallets, bridge contracts, deposit hubs, or service labels.

Wording:

- "The flow reaches an address publicly labeled as OKX Hot Wallet 8."
- "This endpoint is operationally useful for a freeze or information-preservation request, but account attribution requires the service provider or law enforcement."

## Legal And User-Facing Caution

Never overstate identity, intent, or criminal responsibility from chain data alone. Address labels can be incomplete, stale, or service-level rather than user-level.

Prefer:

- "reported scam payment"
- "recipient address"
- "downstream address"
- "publicly labeled exchange hot wallet"
- "highly related flow"

Avoid:

- naming a person or company as the scammer without external evidence
- claiming recovery is guaranteed
- implying Codex can freeze assets directly

## Minimum Evidence Bundle

For police reports or exchange compliance tickets, include:

- initial transaction hash and explorer link
- victim source evidence from the exchange or wallet, if available
- destination address
- downstream key hashes
- final labeled exchange or bridge endpoint, if found
- amount, token contract, chain, and timestamps
- a note explaining any mixed-funds limitation
