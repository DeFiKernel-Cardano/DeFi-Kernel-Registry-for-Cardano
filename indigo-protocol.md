---
name: indigo-protocol
developer: Indigo Protocol
category: lending
script_hashes:
  - label: CDP Validator (placeholder — update from compiled source)
    hash: indigo-cdp-validator-script-hash-update-from-source
links:
  homepage: https://indigoprotocol.io
  source: https://github.com/IndigoProtocol
  docs: https://docs.indigoprotocol.io
---

## Description

Indigo Protocol is an autonomous synthetics protocol for on-chain price exposure to real-world assets, built on Cardano. Users lock ADA as collateral in a Collateralized Debt Position (CDP) to mint **iAssets** — synthetic tokens soft-pegged to the price of a target asset such as iUSD, iBTC, iETH, or iCPI. Every iAsset is over-collateralized (minimum ratio set per asset by the Indigo DAO, typically 150–200%), and each CDP must remain above its Minimum Collateralization Ratio or become eligible for liquidation.

A distinctive Cardano-native feature: **CDP Liquid Staking**. Because Cardano addresses separate payment and staking credentials, Indigo sends CDP collateral to a contract address that carries the *user's* own staking key. The contract controls how the ADA can be spent, but the ADA keeps earning staking rewards as if it had never left the user's wallet. Combined with Stability Pools (a liquidity backstop that absorbs liquidated debt) and a redemption mechanism that enforces a hard price floor for iAssets, this makes Indigo a kernel-compatible synthetics layer that any other kernel protocol can compose with — for example, opening a CDP and immediately swapping the minted iAsset through a kernel DEX in a single atomic transaction.

## Datum Schema

```haskell
-- CDPDatum
data CDPDatum = CDPDatum
  { cdpOwner      :: Maybe PubKeyHash  -- Nothing once frozen / in liquidation
  , iAssetName    :: TokenName         -- e.g. "iUSD", "iBTC"
  , mintedAmount  :: Integer           -- debt, in iAsset units
  , collateral    :: Integer           -- collateral amount in lovelace
  }
```

## Integration Guide

1. Fetch current iAsset and collateral prices from Indigo's on-chain oracle feed (see the `latest-price-feed` repo).
2. Compute the target collateral ratio and the resulting `mintedAmount` for the user's deposit.
3. Build a transaction that locks the collateral at the CDP validator address, attaching the user's own staking credential so the ADA remains delegated.
4. Mint the iAsset via the Indigo mint policy; attach a `CDPDatum` with owner, asset name, minted amount, and collateral.
5. Compose freely with other kernel protocols — for example, route the newly minted iAsset through a kernel DEX swap or an options contract in the same transaction.

## Off-Chain Libraries

| Library            | Language    | URL                                                     |
| ------------------ | ----------- | ------------------------------------------------------- |
| indigo-sdk         | TypeScript  | https://github.com/IndigoProtocol/indigo-sdk            |
| iris               | TypeScript  | https://github.com/IndigoProtocol/iris                  |
| iris-sdk           | TypeScript  | https://github.com/IndigoProtocol/iris-sdk              |
| latest-price-feed  | JavaScript  | https://github.com/IndigoProtocol/latest-price-feed     |
| dexter             | TypeScript  | https://github.com/IndigoProtocol/dexter                |
