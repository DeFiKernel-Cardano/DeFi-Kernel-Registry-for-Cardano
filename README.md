# DeFi Kernel Registry

The public registry of smart contracts that conform to the **DeFi Kernel** standard on Cardano.

This repository is the single source of truth for the [DeFi Kernel website](https://defikernel.org). Each contract is described by one markdown file. The website fetches these files directly — to add or update a contract, open a pull request.

---

## What is DeFi Kernel?

DeFi Kernel is an open standard for publishing financial intent on Cardano. Any smart contract that satisfies the three rules below can declare itself kernel-compatible and inherit the entire kernel ecosystem — its liquidity, its users, and its tooling.

1. **Permissionless** — No batcher required. Users sign and submit their own transactions directly to a Cardano node.
2. **Composable** — The datum schema is publicly documented so any other contract or wallet can read, reference, and atomically chain transactions across protocols.
3. **Discoverable** — Orders are findable by anyone running a Cardano node, without trusting a centralized indexer.

---

## Contributing a contract

1. Fork this repository.
2. Copy [`_template.md`](_template.md) to `<your-contract-slug>.md`.
3. Fill in every field. Do not delete sections — leave them empty if not yet applicable, or write `_n/a_`.
4. Open a pull request. The website rebuilds automatically once the PR is merged.

There is no review committee. The only checks are:

- The file parses (valid YAML frontmatter, all required sections present).
- The script hashes resolve on Cardano mainnet.
- The contract implements the three kernel rules.

---

## File schema

Every contract README has two parts:

### 1. YAML frontmatter (machine-readable)

```yaml
---
name: cardano-swaps
developer: fallen_icarus
category: dex                    # dex | lending | options | marketplace | amm | other
script_hashes:
  - label: Spending Script
    hash: 0a3e3a8f8d9b1c2e4f5a6b7c8d9e0f1a2b3c4d5e6f7a8b9c0d1e2f3a
  - label: Beacon Policy
    hash: 1b4f4b9f9e0c2d3f5a6b7c8d9e0f1a2b3c4d5e6f7a8b9c0d1e2f3b4c
links:
  homepage: https://...
  source: https://github.com/...
  docs: https://...
---
```

The website fetches TVL separately from a Cardano indexer using the script hashes listed above, so contributors do not need to maintain a TVL field.

### 2. Markdown body (human-readable)

The site looks for these section headings, in this order. Headings are case-sensitive.

- `## Description` — A few paragraphs combining what the contract is and what it does. This is the long-form text shown when a user expands the row on the website. Combine the "Description" and "What It Does" content here.
- `## Datum Schema` — A code block (any language hint is fine; `haskell`, `aiken`, or `rust` render best) showing the on-chain datum or redeemer structure.
- `## Integration Guide` — A numbered list (`1.`, `2.`, `3.` …) of steps a developer follows to build a transaction against this contract.
- `## Off-Chain Libraries` — A markdown table with three columns: `Library`, `Language`, and `URL`.

That's it. The site renders the markdown of each section into the corresponding slot in the detail panel.

---

## Repo layout

```
defi-kernel-registry/
├── README.md              # this file
├── _template.md       # copy this to start a new contract
├── cardano-swaps.md
├── cardano-loans.md
├── cardano-options.md
├── cardano-aftermarket.md
├── danogo-clmm.md
├── danogo-lending.md
└── splash-dex.md
```

---

## License

CC0 — public domain. The registry, like the kernel, belongs to no one.
