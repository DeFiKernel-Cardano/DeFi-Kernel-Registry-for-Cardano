---
name: cardano-loans
developer: fallen_icarus
category: lending
script_hashes:
  - label: Spending Script (placeholder — update from compiled source)
    hash: 1b4f4b9f9e0c2d3f5a6b7c8d9e0f1a2b3c4d5e6f7a8b9c0d1e2f3b4c
links:
  homepage: https://github.com/fallen-icarus/cardano-loans
  source: https://github.com/fallen-icarus/cardano-loans
  docs: https://github.com/fallen-icarus/cardano-loans#readme
---

## Description

`cardano-loans` is a peer-to-peer lending and borrowing protocol with trustlessly negotiable loan terms and built-in on-chain credit histories. Every loan is its own term sheet — borrowers and lenders settle directly without a rate-setting committee or oracle dependency for the negotiation itself.

Active loans are tradeable as bond NFTs on the kernel aftermarket, which means lenders can sell their position on the secondary market without disrupting the borrower's repayment schedule. Defaulted collateral is claimable by the bond NFT holder via composition with the aftermarket contract — no liquidation bot or external keeper required.

## Datum Schema

```haskell
-- ActiveDatum
data ActiveDatum = ActiveDatum
  { borrowerId      :: TokenName
  , lenderAddress   :: Address
  , loanAsset       :: Asset
  , collateral      :: [(Asset, Rational)]
  , loanPrincipal   :: Integer
  , loanTerm        :: POSIXTime
  , loanInterest    :: Rational
  , loanOutstanding :: Rational
  }
```

## Integration Guide

1. Borrowers post Ask UTxOs tagged with asset beacons describing their desired terms.
2. Lenders post Offer UTxOs with principal as a counter-offer.
3. Borrower accepts an Offer in a single transaction — an Active loan UTxO is created.
4. Repayments live in the same transaction graph; full payment burns the borrower-ID beacon.
5. Defaulted collateral is claimable via aftermarket composition.

## Off-Chain Libraries

| Library           | Language    | URL                                                  |
| ----------------- | ----------- | ---------------------------------------------------- |
| cardano-loans CLI | Haskell     | https://github.com/fallen-icarus/cardano-loans       |
| Lucid Evolution   | TypeScript  | https://github.com/Anastasia-Labs/lucid-evolution    |
| MeshJS            | TypeScript  | https://meshjs.dev                                   |
| PyCardano         | Python      | https://github.com/Python-Cardano/pycardano          |
