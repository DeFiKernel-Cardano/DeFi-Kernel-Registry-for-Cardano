---
name: cardano-swaps
developer: fallen_icarus
category: dex
script_hashes:
  - label: Spending Script (placeholder — update from compiled source)
    hash: 0a3e3a8f8d9b1c2e4f5a6b7c8d9e0f1a2b3c4d5e6f7a8b9c0d1e2f3a
links:
  homepage: https://github.com/fallen-icarus/cardano-swaps
  source: https://github.com/fallen-icarus/cardano-swaps
  docs: https://github.com/fallen-icarus/cardano-swaps#readme
---

## Description

`cardano-swaps` is a distributed order-book DEX that uses composable atomic swaps with full delegation control and endogenous liquidity. Limit orders and one-way liquidity swaps live as UTxOs tagged with beacon tokens, queryable by any Cardano node.

The true power of the protocol emerges from composability: takers can chain swaps together to create arbitrarily complex multi-hop trades (e.g. ADA → ERGO → DUST) in a single transaction, and any other dApp on the kernel can compose its own actions with a swap in the same transaction. This is the settlement layer the rest of the kernel pivots around — every kernel-compatible application can tap directly into its liquidity, and bootstrapping is no longer the hard part of launching a new dApp.

## Datum Schema

```haskell
-- SwapDatum (one-way)
data SwapDatum = SwapDatum
  { beaconId    :: PolicyId
  , pairBeacon  :: TokenName
  , offerId     :: PolicyId
  , offerName   :: TokenName
  , offerBeacon :: TokenName
  , askId       :: PolicyId
  , askName     :: TokenName
  , askBeacon   :: TokenName
  , swapPrice   :: Rational
  , prevInput   :: Maybe TxOutRef
  }
```

## Integration Guide

1. Query the chain for beacon tokens matching your trading pair.
2. Filter UTxOs by `pairBeacon` to assemble the live order book.
3. Build a transaction spending one or more swap UTxOs at the encoded `swapPrice`.
4. Burn spent beacons and mint new ones if you reroll the order. Sign and submit.
5. Compose with loans, options, or aftermarket spends in the same transaction — all atomic.

## Off-Chain Libraries

| Library           | Language    | URL                                                  |
| ----------------- | ----------- | ---------------------------------------------------- |
| cardano-swaps CLI | Haskell     | https://github.com/fallen-icarus/cardano-swaps       |
| Lucid Evolution   | TypeScript  | https://github.com/Anastasia-Labs/lucid-evolution    |
| MeshJS            | TypeScript  | https://meshjs.dev                                   |
| PyCardano         | Python      | https://github.com/Python-Cardano/pycardano          |
