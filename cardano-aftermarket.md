---
name: cardano-aftermarket
developer: fallen_icarus
category: marketplace
script_hashes:
  - label: Spending Script (placeholder — update from compiled source)
    hash: 3d6a6dba1a2e4f5a7b8d9e0f1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d
links:
  homepage: https://github.com/fallen-icarus/cardano-aftermarket
  source: https://github.com/fallen-icarus/cardano-aftermarket
  docs: https://github.com/fallen-icarus/cardano-aftermarket#readme
---

## Description

`cardano-aftermarket` is a general-purpose marketplace for reselling any tradeable financial NFT — bonds from `cardano-loans`, active option contracts from `cardano-options`, futures, and instruments not yet invented. Because aftermarkets are discovered by minting policy alone, this single contract supports every financial instrument that has been or will ever be issued on Cardano as an NFT, with no upgrade path needed and no central registry.

New markets need no governance action: mint a policy beacon and you're live. The protocol is also composable with the rest of the kernel — a buyer can purchase a bond NFT and immediately spend it against `cardano-loans` in the same transaction, removing the settlement risk that plagues most secondary markets.

## Datum Schema

```haskell
-- SaleDatum
data SaleDatum = SaleDatum
  { saleBeacon     :: TokenName
  , policyBeacon   :: TokenName
  , nftPolicyId    :: PolicyId
  , nftNames       :: [TokenName]
  , salePrice      :: [(Asset, Integer)]
  , paymentAddress :: Address
  }
```

## Integration Guide

1. Mint a policy beacon for any new instrument — no governance approval required.
2. Sellers list NFTs (bonds, option contracts, etc.) at their own dApp address.
3. Bidders post non-custodial bids.
4. Seller accepts → atomic transfer of NFT and payment in one transaction.
5. Compose with `cardano-loans` or `cardano-options` to spend the NFT in the same transaction.

## Off-Chain Libraries

| Library                 | Language    | URL                                                  |
| ----------------------- | ----------- | ---------------------------------------------------- |
| cardano-aftermarket CLI | Haskell     | https://github.com/fallen-icarus/cardano-aftermarket |
| Lucid Evolution         | TypeScript  | https://github.com/Anastasia-Labs/lucid-evolution    |
| MeshJS                  | TypeScript  | https://meshjs.dev                                   |
| PyCardano               | Python      | https://github.com/Python-Cardano/pycardano          |
