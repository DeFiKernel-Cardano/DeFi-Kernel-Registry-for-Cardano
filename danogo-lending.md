---
name: danogo-lending
developer: Danogo (Dano Finance)
category: lending
script_hashes:
  - label: Lending Pool Script (Mainnet)
    hash: 703d581d9a657d1326aea6a754cdcf191efb22133a62a181951156cf
links:
  homepage: https://danogo.io
  source: https://docs.danogo.io
  docs: https://docs.danogo.io/developers/integration/guide-to-create-a-flexible-loan-transaction
---

## Description

`danogo-lending` is a pool-based lending protocol with floating interest rates and oracle-priced collateral. Borrowers create loans against pool reserves; the pool simultaneously withdraws base tokens from a staking contract and posts collateral, all in a single atomic transaction. Multi-oracle price aggregation hardens collateral valuation by combining feeds from Charli3, Orcfax, Splash, Liqwid, Indigo, Djed, and Minswap LP.

Each loan mints a `loanOwnerNft` representing the borrower's position. Because the position is an NFT, borrowers can compose their loan with other kernel protocols atomically — for example, opening a loan, swapping the borrowed asset on `cardano-swaps`, and writing a covered call on `cardano-options`, all in one signed transaction.

## Datum Schema

```rust
// LendingAction.CreateLoan redeemer
type CreateLoan {
    pool_out_idx:         Int,           // required
    loan_out_idx:         Int,           // required
    fee_out_idx:          Option<Int>,   // required if fee > 0
    protocol_cfg_ref_idx: Int,           // required
    market_ref_idx:       Int,           // required
    pool_in_out_ref:      OutputReference // required
}

// OraclePriceCalcRdmr supports a wide range of
// oracle sources: Charli3, Orcfax, Liqwid V1/V2,
// Splash CPAMM G1/G2/G3, Splash Stable, Indigo,
// Djed, Danogo Pool, Danogo Staking, Minswap LP.
```

## Integration Guide

1. Call the Loan API for pool, collateral, and oracle reference inputs.
2. Spend `PoolInUtxo` with the `CreateLoan` redeemer; spend `StakingContract` with `TopupWithdrawStaking`.
3. Emit `PoolOut`, `LoanOut`, `FeeOut`, and `StakingContractOut` exactly as instructed by the API.
4. Add an `OraclePriceCalcRdmr` withdrawal for collateral pricing (multi-oracle aggregation).
5. Mint the `loanOwnerNft` (FLOAT or STAKING redeemer) and attach metadata. TTL must be ≤ 6 minutes.

## Off-Chain Libraries

| Library         | Language    | URL                                                                       |
| --------------- | ----------- | ------------------------------------------------------------------------- |
| Danogo Loan API | REST        | https://docs.danogo.io/developers/integration/lending-apis                |
| Lucid Evolution | TypeScript  | https://github.com/Anastasia-Labs/lucid-evolution                         |
