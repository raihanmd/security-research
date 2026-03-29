# T-Swap# T-Swap

> Decentralized Automated Market Maker (AMM) enabling permissionless token swaps with liquidity provider rewards

- **Source**: [GitHub Repository](https://github.com/Cyfrin/5-t-swap-audit)
- **Audit Date**: March 29, 2026
- **Commit Hash**: 7bd52c6a75f1115b23e0cff0fa16f7c522703fcd
- **nSLOC**: 447

---

## Protocol Overview

T-Swap is a decentralized asset exchange protocol built on the Automated Market Maker (AMM) model, similar to Uniswap v1. The protocol enables permissionless token swaps between any ERC20 token and WETH through liquidity pools. It's designed to be a fair-price exchange mechanism where users can swap assets with transparent pricing.

### Key Features

- **Liquidity Pools**: Each pool consists of an ERC20 token pair with WETH
- **Automated Market Maker (AMM)**: Uses the x\*y=k constant product invariant for pricing
- **Liquidity Provider (LP) Rewards**: LPs earn 0.3% fee revenue from swaps
- **Two Swap Methods**: `swapExactInput()` and `swapExactOutput()` for flexible trading
- **LP Tokens**: Represent share of pool for liquidity providers

### Fee Model

- **Fee**: 0.3% (represented as 997/1000 multiplier)
- **Fee Distribution**: Stays in the pool, rewarding liquidity providers

---

## Protocol Phases

### Phase 1: Pool Creation

The `PoolFactory` contract deploys new `TSwapPool` instances for ERC20/WETH pairs. Each pool operates independently and manages its own liquidity.

### Phase 2: Liquidity Provision

Users (Liquidity Providers) deposit tokens into pools and receive LP tokens representing their share of the pool. They earn fee revenue from all swaps in their pool.

### Phase 3: Token Swaps

Users (Traders/Swappers) execute permissionless token swaps using either:

- `swapExactInput()`: Specify exact input amount, receive variable output
- `swapExactOutput()`: Specify exact output amount, pay variable input

### Phase 4: Liquidity Withdrawal

LPs can withdraw their liquidity at any time by burning their LP tokens.

---

## Core Invariant

The protocol maintains the constant product invariant: $x \times y = k$

Where:

- $x$ = Token Balance X (ERC20)
- $y$ = Token Balance Y (WETH)
- $k$ = The constant product

With the 0.3% fee, the invariant increases with each swap to reward liquidity providers.

---

## Actors

| Role                          | Description                                                                                                                                 |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **Protocol Owner**            | Deploys the PoolFactory contract, sets parameters (WETH token address, fee structure), and creates new trading pools                        |
| **Liquidity Providers (LPs)** | Deposit ERC20 tokens and WETH into pools, receive LP tokens, earn 0.3% fee revenue from swaps, can withdraw liquidity at any time           |
| **Traders/Swappers**          | Execute permissionless token swaps via `swapExactInput()` or `swapExactOutput()`, pay 0.3% fee on all swaps, require no special permissions |

---

## Audit Scope

### In Scope

```
./src/
  ├── PoolFactory.sol
  └── TSwapPool.sol
```

- **Solidity Version**: 0.8.20
- **Focus**: Smart contract security audit with Foundry test suite validation

### Test Framework

- **Framework**: Foundry
- **Test Files**: Unit and invariant tests for all findings
- **Time**: 4 hours with 1 security researcher

---

## Executive Summary

Security audit of T-Swap AMM protocol revealed **11 findings** across multiple severity levels:

| Severity  | Count  |
| --------- | ------ |
| HIGH      | 3      |
| MEDIUM    | 1      |
| LOW       | 2      |
| INFO      | 4      |
| **TOTAL** | **11** |

### Key Findings

**HIGH Severity Issues:**

1. Missing deadline validation on deposit function - MEV/sandwich attack risk
2. Incorrect fee calculation (10_000 vs 1_000) - Users charged 10x fees
3. Protocol invariant violation from bonus tokens - Pool insolvency risk

**MEDIUM Severity Issues:**

1. Missing maximum input amount in swapExactOutput - No slippage protection

**LOW Severity Issues:**

1. Loose deadline check implementation
2. Wrong event parameter ordering

**INFORMATIONAL Issues:**

1. Unused error definitions
2. Missing indexed event parameters
3. Magic numbers without constants
4. Missing zero address validation

---

## Recommendations

1. **Immediate Action**: Address all HIGH severity findings before deployment
2. **Before Launch**: Implement MEDIUM severity mitigations
3. **Follow-up**: Consider LOW and INFORMATIONAL improvements for code quality

All findings include:

- Detailed descriptions and risk assessments
- Proof of concept code examples
- Recommended mitigations with code snippets
- Foundry test validation

---

## Related Links

- [Original Repository](https://github.com/Cyfrin/5-t-swap-audit)
- [Audit Report](./audit/report.pdf)
