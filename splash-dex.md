---
name: splash-dex
developer: Splash Protocol
category: amm
script_hashes:
  - label: CPAMM-G3 Validator (placeholder — update from compiled source)
    hash: splash-cpamm-g3-script-hash-update-from-source
links:
  homepage: https://splash.trade
  source: https://github.com/splashprotocol
  docs: https://github.com/splashprotocol/splash-core
---

## Description

`splash-dex` is a capital-efficient market making protocol on Cardano with multiple curve families exposed through a single protocol surface. General trading pairs use constant-product pools (CPAMM, currently in three generations G1/G2/G3) and pegged-asset pairs use stable curves. The on-chain validators are kept lean; off-chain agents handle order routing and discovery without ever holding custody of user funds.

Splash pools are referenced by Danogo's lending oracle as one of the collateral price sources, illustrating exactly the kind of cross-protocol composition the kernel is designed for: a kernel-compatible AMM publishes its pool state on chain, and a kernel-compatible lending protocol consumes that state directly without any centralized middleware in between.

## Datum Schema

```haskell
-- Splash supports multiple pool curves,
-- each with its own validator and datum:

SplashCpammG1   -- gen 1 constant product
SplashCpammG2   -- gen 2 constant product
SplashCpammG3   -- gen 3 constant product
SplashStable    -- stable-curve pool

-- Each PoolDatum encodes reserves, fee tier,
-- and oracle hooks consumed by external
-- integrations such as danogo-lending.
```

## Integration Guide

1. Discover pools via the `splash-offchain-multiplatform` agent or a direct chain query.
2. Read the `PoolDatum` for reserves and fee tier; pick the matching curve (CPAMM or Stable).
3. Build a swap transaction spending the pool UTxO; emit an updated `PoolOut` with adjusted reserves.
4. Submit directly — Splash validators do not require a centralized batcher.
5. Compose with kernel protocols: feed the swap output directly into `cardano-loans` or `cardano-options` in the same transaction.

## Off-Chain Libraries

| Library                       | Language    | URL                                                          |
| ----------------------------- | ----------- | ------------------------------------------------------------ |
| protocol-sdk                  | TypeScript  | https://github.com/splashprotocol/protocol-sdk               |
| splash-offchain-multiplatform | Rust        | https://github.com/splashprotocol/splash-offchain-multiplatform |
| splash-core                   | Haskell     | https://github.com/splashprotocol/splash-core                |
| aleph-cli                     | Rust        | https://github.com/splashprotocol/aleph-cli                  |
