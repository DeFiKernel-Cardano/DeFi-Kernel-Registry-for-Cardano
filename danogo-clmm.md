---
name: danogo-clmm
developer: Danogo (Dano Finance)
category: amm
script_hashes:
  - label: Pool Script (Mainnet)
    hash: d8b69fc53637bcfadbc4469083f706bc293f4d9d2296646c5ca167bb
  - label: Protocol Script (Mainnet)
    hash: fa991bc2f9c4206e72d713bc3487a72e7901057cabb8d364bebeef8f
links:
  homepage: https://danogo.io
  source: https://github.com/dano-finance/clmm-sdk
  docs: https://docs.danogo.io/developers/integration/concentrated-liquidity-pool-integration
---

## Description

`danogo-clmm` is a capital-efficient AMM where liquidity providers concentrate their capital within custom price ranges defined by `sqrt_lower_price` and `sqrt_upper_price` — Uniswap V3 style mechanics adapted for Cardano's eUTxO model. LPs earn far higher fee yield per unit of capital compared to flat constant-product pools, because their liquidity is only deployed where the price is actively trading.

Multi-pool swaps execute in a single transaction via a compact byte-array redeemer that lets one transaction hop across many pools simultaneously. Takers can chain pools atomically with no batcher in the middle, which preserves both composability with the rest of the kernel and the censorship-resistance properties that make the kernel attractive in the first place.

## Datum Schema

```rust
// PoolDatum
type PoolDatum {
    token_X:              TupleAsset,
    token_Y:              TupleAsset,
    lp_fee_rate:          Basis,
    platform_fee_X:       Int,
    platform_fee_Y:       Int,
    total_swap_fee:       Int,
    sqrt_lower_price:     PRational,
    sqrt_upper_price:     PRational,
    min_x_change:         Int,
    min_y_change:         Int,
    circulating_lp_token: Int,
    last_withdraw_epoch:  Int,
}
```

## Integration Guide

1. Call `GET /api/v1/concentrated/pools` to discover pools by `tokenA` and `tokenB`.
2. Call `GET /api/v1/concentrated/pool-params` with each `lpToken` to retrieve reserves, sqrt-prices, and fee config.
3. Build the `ExchangeActionRedeemer` (ByteArray) — Swap action = `0x03`, with `in_idx` and pool indices.
4. Spend the pool UTxO; emit `pool_out` with updated reserves and platform fee accumulators.
5. Add a withdraw-zero from the pool script reward address. Submit. TTL must be ≤ 6 minutes.

## Off-Chain Libraries

| Library            | Language    | URL                                                                    |
| ------------------ | ----------- | ---------------------------------------------------------------------- |
| clmm-sdk           | TypeScript  | https://github.com/dano-finance/clmm-sdk                               |
| Lucid Evolution    | TypeScript  | https://github.com/Anastasia-Labs/lucid-evolution                      |
| Danogo Indexer API | REST        | https://dapp-indexers.api.danogo.io/api/v1/concentrated/pools          |
