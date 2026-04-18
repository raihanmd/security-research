# ThunderLoan

> Flash loan protocol based on Aave and Compound, enabling permissionless loans with liquidity provider rewards

- **Source**: [GitHub Repository](https://github.com/Cyfrin/6-thunder-loan-audit)
- **Audit Date**: April 18, 2026
- **Commit Hash**: 8803f851f6b37e99eab2e94b4690c8b70e26b3f6
- **nSLOC**: 783

---

## Protocol Overview

ThunderLoan is a flash loan protocol designed to enable users to borrow assets for exactly one transaction. It's modeled after Aave and Compound, providing two main services: flash loans for instant, collateral-free borrowing, and liquidity provision for earning passive income from loan fees.

Liquidity providers can deposit assets into ThunderLoan and receive AssetTokens in return. These AssetTokens gain value over time as the protocol collects fees from flash loans. The protocol uses an on-chain TSwap price oracle to calculate fees based on token values.

### Key Features

- **Flash Loans**: Borrow any amount of assets for one transaction with 0.3% fee
- **AssetTokens**: ERC20 tokens representing LP share that appreciate with fee collection
- **Exchange Rate Mechanism**: Tracks value of AssetTokens relative to underlying assets
- **Upgradeable Proxy**: UUPS proxy pattern for contract upgrades
- **Price Oracle**: TSwap-based oracle for fee calculations in WETH terms

### Fee Model

- **Fee**: 0.3% of borrowed amount (calculated in WETH terms)
- **Fee Distribution**: Stays in protocol, increasing exchange rate for LPs
- **Fee Precision**: 1e18 (18 decimals)

---

## Protocol Phases

### Phase 1: Token Allowlist

The `Owner` adds tokens to the allowed list via `setAllowedToken()`. Each allowed token gets an associated `AssetToken` contract that tracks LP deposits.

### Phase 2: Liquidity Provision

Users (Liquidity Providers) deposit ERC20 tokens into the protocol and receive AssetTokens representing their share. The exchange rate determines how many underlying tokens each AssetToken is worth.

### Phase 3: Flash Loans

Users (Borrowers) execute permissionless flash loans via `flashLoan()`:

1. Borrower specifies token, amount, and receiver contract
2. Protocol transfers tokens to receiver
3. Protocol calls `executeOperation()` callback on receiver
4. Receiver performs operations and repays amount plus fee
5. Protocol verifies repayment and updates exchange rate

### Phase 4: Liquidity Withdrawal

LPs can withdraw their liquidity at any time by redeeming AssetTokens at the current exchange rate.

---

## Core Mechanism

The protocol maintains the exchange rate between AssetTokens and underlying assets:

$$exchangeRate = \frac{totalUnderlying \times EXCHANGE\_RATE\_PRECISION}{totalAssetTokens}$$

Where `EXCHANGE_RATE_PRECISION = 1e18`.

The exchange rate increases when flash loan fees are collected. Each AssetToken becomes worth more underlying tokens as fees accrue.

### Flash Loan Fee Calculation

$$valueOfBorrowedToken = \frac{amount \times getPriceInWeth(token)}{feePrecision}$$

$$fee = \frac{valueOfBorrowedToken \times flashLoanFee}{feePrecision}$$

Where:

- `feePrecision = 1e18`
- `flashLoanFee = 3e15` (0.3%)

---

## Actors

| Role                          | Description                                                                                          |
| ----------------------------- | ---------------------------------------------------------------------------------------------------- |
| **Owner**                     | Deploys contracts, sets allowed tokens, configures fees, upgrades implementation via UUPS proxy      |
| **Liquidity Providers (LPs)** | Deposit assets, receive AssetTokens, earn fees from flash loans proportional to their share          |
| **Flash Loan Users**          | Borrow any allowed token for one transaction, implement `IFlashLoanReceiver` interface, pay 0.3% fee |

---

## Audit Scope

### In Scope

```
#-- interfaces
|   #-- IFlashLoanReceiver.sol
|   #-- IPoolFactory.sol
|   #-- ITSwapPool.sol
|   #-- IThunderLoan.sol
#-- protocol
|   #-- AssetToken.sol
|   #-- OracleUpgradeable.sol
|   #-- ThunderLoan.sol
#-- upgradedProtocol
    #-- ThunderLoanUpgraded.sol
```

- **Solidity Version**: 0.8.20
- **Chain**: Ethereum
- **ERC20s**: USDC, DAI, LINK, WETH
- **Focus**: Smart contract security audit with Foundry test suite validation

### Test Framework

- **Framework**: Foundry
- **Test Files**: Unit tests and PoC exploits for all findings
- **Time**: 4 hours with 1 security researcher

---

## Executive Summary

Security audit of ThunderLoan flash loan protocol revealed **12 findings** across multiple severity levels:

| Severity  | Count  |
| --------- | ------ |
| CRITICAL  | 3      |
| HIGH      | 3      |
| MEDIUM    | 2      |
| INFO      | 4      |
| **TOTAL** | **12** |

### Key Findings

**CRITICAL Severity Issues:**

1. Exchange rate inflation via phantom fee in `deposit()` - LPs lose funds
2. Storage layout corruption on contract upgrade - Protocol breaks after upgrade
3. Flash loan updates exchange rate before fee collection - Rate manipulation window

**HIGH Severity Issues:**

1. Unit mismatch in fee calculation - Fees only correct when price equals 1e18
2. Missing zero address validation in Oracle initialization - Protocol can brick
3. Unchecked return value in flash loan callback - Off-chain confusion risk

**MEDIUM Severity Issues:**

1. Missing reentrancy guard on flash loan - Cross-token reentrancy possible
2. Unauthorized repayment interference - Third parties can interfere with loans

**INFORMATIONAL Issues:**

1. Unused error definition
2. Rounding error in small flash loans (acknowledged by team)
3. First depositor advantage (team plans mitigation)
4. Weird ERC20 token handling (team will vet tokens)

---

## Recommendations

1. **Immediate Action**: Address all CRITICAL severity findings before deployment
2. **Before Launch**: Implement HIGH severity mitigations
3. **Follow-up**: Consider MEDIUM and INFORMATIONAL improvements for code quality

All findings include:

- Detailed descriptions and risk assessments
- Proof of concept code with economic impact verification
- Recommended mitigations with code snippets
- Foundry test validation

---

## Related Links

- [Original Repository](https://github.com/Cyfrin/6-thunder-loan-audit)
- [Audit Report](./audit/report.md)
- [Findings Detail](./audit/findings.md)
