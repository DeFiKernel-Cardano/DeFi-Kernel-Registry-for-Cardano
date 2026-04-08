---
name: my-contract-name
developer: Your Name or Team
category: dex
script_hashes:
  - label: Spending Script
    hash: replace-with-real-script-hash
links:
  homepage: https://example.com
  source: https://github.com/your/repo
  docs: https://docs.example.com
---

## Description

Two or three short paragraphs explaining what the contract is, what problem it solves, and what makes it kernel-compatible. This is the long-form text shown when a user expands the row on the website. Combine your "Description" and "What It Does" content into this single section.

Markdown formatting (**bold**, _italic_, links, lists) is fine here.

## Datum Schema

```haskell
-- Replace with your real on-chain datum
data MyDatum = MyDatum
  { fieldOne   :: PolicyId
  , fieldTwo   :: TokenName
  , fieldThree :: Integer
  }
```

## Integration Guide

1. First step a developer follows to build a transaction against this contract.
2. Second step.
3. Third step.
4. Fourth step.
5. Fifth step.

## Off-Chain Libraries

| Library         | Language    | URL                                  |
| --------------- | ----------- | ------------------------------------ |
| my-contract-cli | Haskell     | https://github.com/your/cli          |
| Lucid Evolution | TypeScript  | https://github.com/Anastasia-Labs/lucid-evolution |
| MeshJS          | TypeScript  | https://meshjs.dev                   |
