# NFT Dealers

> NFT marketplace with progressive fee structure for secondary sales

- **Source**: [CodeHawks Contest](https://codehawks.cyfrin.io/c/2026-03-nft-dealers)
- **Contest Period**: March 12, 2026 - March 19, 2026 (Noon UTC)
- **nSLOC**: 253

---

## Protocol Overview

NFT Dealers is an NFT marketplace protocol with the following features:

- **Pre-set supply** with reveal mechanism
- **Progressive fee structure**: 1%, 3%, or 5% based on resell price
- **Collateral-based minting**: Collects base price on minting
- **Secondary market**: NFTs can be resold at any price with fee growing alongside the resell price
- **Flexible use cases**: Suitable for in-game events, ticketing systems, and more

---

## Protocol Phases

### Phase 1: Preparation Phase

The protocol is deployed but not yet "revealed". During this phase:

- Owner can whitelist wallets that are eligible to mint NFTs

### Phase 2: Revealed Phase

Once the protocol is revealed:

- Whitelisted users can mint NFTs
- Whitelisted users can list NFTs for secondary sales
- Anyone can buy from listings
- Owner can withdraw accumulated fees
- Owner can remove wallets from whitelist at any time

---

## Actors

| Role | Description |
|------|-------------|
| **Owner** | Deploys the smart contract, sets parameters (collateral, collection name, image, symbol), manages whitelist, reveals the protocol, and withdraws fees |
| **Whitelisted User** | Can mint NFTs, buy, update price, cancel listings, list NFTs, and collect USDC after selling |
| **Non-Whitelisted User** | Cannot mint, but can buy, update price, cancel listings, list NFTs, and collect USDC after selling |

---

## Contract Scope

```
src/
├── MockUSDC.sol      # Mock USDC token for testing
└── NFTDealers.sol    # Main NFT marketplace contract
```

---

## Compatibilities

| Category | Details |
|----------|---------|
| **Blockchain** | Ethereum / Any EVM |
| **Token Standards** | ERC-20 (USDC only), ERC-721 |

---

## Setup

```bash
# Clone the repository
git clone https://github.com/CodeHawks-Contests/2026-03-NFT-dealers.git

# Install dependencies
forge install

# Build contracts
forge build

# Run tests
forge test
```

---

## Audit Report

The detailed security audit findings are available in:

- **[audit/report.md](./audit/report.md)** - Full findings in Markdown format
- **[audit/report.pdf](./audit/report.pdf)** - Formal PDF report

---

## Known Issues

None reported at the time of audit.

---

## Related Links

- [CodeHawks Contest Page](https://codehawks.cyfrin.io/c/2026-03-nft-dealers)
- [Original Repository](https://github.com/CodeHawks-Contests/2026-03-NFT-dealers)
