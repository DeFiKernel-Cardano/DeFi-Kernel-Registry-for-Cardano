---
name: cardano-options
developer: fallen_icarus
category: options
script_hashes:
  - label: Spending Script (placeholder — update from compiled source)
    hash: 2c5a5cae0f1d3e4a6b7c8d9e0f1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c
links:
  homepage: https://github.com/fallen-icarus/cardano-options
  source: https://github.com/fallen-icarus/cardano-options
  docs: https://github.com/fallen-icarus/cardano-options#readme
---

## Description

`cardano-options` is a p2p protocol for options contracts on Cardano. Writers each create a personal "options" address where all contracts and locked assets are kept until execution or expiry. Every dimension of the contract — offered asset, asked asset, premium asset, strike price, expiry — is configured by the writer themselves, and every Cardano native asset is supported out of the box.

Buyers query proposal beacons by offer, ask, or premium asset to find matching contracts. Once a buyer pays the premium, an Active contract UTxO is minted as a transferable NFT, which the buyer can either exercise or resell on the kernel aftermarket. The protocol is composable with itself: an options writer can close an expired contract and immediately create a new contract for sale in a single transaction.

## Datum Schema

```haskell
-- ProposalDatum
data ProposalDatum = ProposalDatum
  { proposalBeacon :: TokenName
  , offerAsset     :: Asset
  , offerQuantity  :: Integer
  , askAsset       :: Asset
  , premiumAsset   :: Asset
  , premiumPrice   :: Rational
  , strikePrice    :: Rational
  , expiration     :: POSIXTime
  , payToAddress   :: Address
  }
```

## Integration Guide

1. Writer locks the offer asset in their personal options address with a Proposal datum.
2. Buyer queries proposal beacons by offer, ask, or premium asset to find matching contracts.
3. Buyer pays the premium → an Active contract UTxO is minted, transferable as an NFT.
4. Buyer either exercises (pays the strike to the writer's address) or lets the contract expire.
5. Compose with the aftermarket to resell active contracts before expiry.

## Off-Chain Libraries

| Library             | Language    | URL                                                  |
| ------------------- | ----------- | ---------------------------------------------------- |
| cardano-options CLI | Haskell     | https://github.com/fallen-icarus/cardano-options     |
| Lucid Evolution     | TypeScript  | https://github.com/Anastasia-Labs/lucid-evolution    |
| Aiken (on-chain)    | Aiken       | https://aiken-lang.org                               |
