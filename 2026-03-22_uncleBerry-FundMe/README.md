# FundMe v1.0

> Decentralized crowdfunding protocol with Chainlink oracles and NFT tier rewards

- **Source**: [GitHub Repository](https://github.com/uncleBerry/FundMe-v1.0)
- **Commit Hash**: cc06172ede346cf94f44780a267e7a6c95a7e64c
- **Audit Date**: March 22, 2026
- **nSLOC**: 351

---

## Protocol Overview

FundMe v1.0 is a fully decentralized, autonomous crowdfunding protocol that leverages smart contracts for transparency, Chainlink Oracles for accurate USD-to-ETH price conversions, and Chainlink Automation to handle campaign finalization without human intervention. Donors receive unique NFT Tier Rewards stored on IPFS as immutable proof of support.

### Key Features

- **Minimum $5 USD** contribution requirement (via Chainlink ETH/USD price feed)
- **Tiered NFT rewards**: Bronze ($50+), Silver ($200+), Gold ($1000+)
- **Chainlink Automation** for automatic campaign closure
- **IPFS storage** for NFT metadata

---

## Protocol Phases

### Phase 1: NOT_OPEN_YET

Initial state after deployment. Owner has not opened the campaign yet.

### Phase 2: OPEN

Active fundraising phase. Users can fund ETH to the campaign.

### Phase 3: CLOSED

Campaign ended (target reached or deadline passed). No more funding accepted.

### Phase 4: FINALIZED

Owner has withdrawn all funds. Contract is ready for a new round.

---

## Actors

| Role | Description |
|------|-------------|
| **Owner** | Deploys the smart contract, opens campaign, closes campaign, withdraws funds, claims NFT rewards on behalf of funders, modifies NFT tier URIs |
| **Funder** | Funds ETH to active campaigns, claims NFT rewards based on contribution tier |
| **Chainlink Automation / Anyone** | Can call `performUpkeep` to trigger automatic campaign closure, can call `closeCampaign` directly |

---

## Contract Scope

```
src/
├── FundMe.sol              # Main crowdfunding contract with Chainlink integration
├── FundMeRewardNft.sol     # ERC-721 NFT rewards contract
├── PriceConverter.sol      # ETH to USD price conversion utility
├── CampaignLifeCycle.sol   # Campaign state management enum
├── CampaignClosedReason.sol # Campaign closure reasons enum
└── NftTier.sol            # NFT tier definitions enum
```

---

## Compatibilities

| Category | Details |
|----------|---------|
| **Blockchain** | Ethereum / Any EVM |
| **External Protocols** | Chainlink (Price Feeds + Automation), OpenZeppelin |
| **Token Standards** | ERC-20 (ETH), ERC-721 (NFT Rewards) |

---

## Setup

```bash
# Clone the repository
git clone https://github.com/uncleBerry/FundMe-v1.0

# Install dependencies
forge install

# Build contracts
forge build

# Run tests
forge test
```

---

## Deployment Details

The FundMe v1.0 smart contract is deployed and verified on the **Sepolia Testnet**:

| Contract | Network | Address |
| :--- | :--- | :--- |
| **FundMe (Core)** | Sepolia | [`0xb449e5a68680c3e2688a7fd87918117AD777908d`](https://sepolia.etherscan.io/address/0xb449e5a68680c3e2688a7fd87918117ad777908d) |
| **FundMeNFT (Rewards)** | Sepolia | [`0x284e2a4792F6a1b2Cfc26DdC4340946481E49FDA`](https://sepolia.etherscan.io/token/0x284e2a4792f6a1b2cfc26ddc4340946481e49fda) |

---

## Audit Report

The detailed security audit findings are available in:

- **[audit/FundMeV1.0-audit-report.pdf](./audit/FundMeV1.0-audit-report.pdf)** - Full audit report

### Findings Summary

| Severity | Number of issues found |
| -------- | ---------------------- |
| CRITICAL | 1 |
| MEDIUM   | 1 |
| LOW      | 6 |
| INFO     | 3 |
| TOTAL    | 11 |

---

## Known Issues

The following issues were identified during the audit and are being tracked:

- **CR-1**: NFT Tier Eligibility Calculated at Claim Time, Not Fund Time — Users may receive NFTs they weren't entitled to based on ETH price volatility
- **M-1**: No Minimum Campaign Duration Enforced — Campaign can be opened with 1 second duration

---

## Related Links

- [Original Repository](https://github.com/uncleBerry/FundMe-v1.0)
- [Audit Report](./audit/FundMeV1.0-audit-report.pdf)
