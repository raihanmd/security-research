# Vault Guardians

> Vault Guardians is a protocol that allows users to deposit ERC20 tokens into ERC4626 vaults managed by vault guardians. The goal of a vault guardian is to manage the vault to maximize value for depositors by allocating funds across Aave V3, Uniswap V2, or simply holding the assets.

Vault guardians must stake a deposit to become guardians and can move funds between the investable universe protocols (Aave, Uniswap, or hold). The protocol includes a DAO that handles pricing parameters and receives a cut of guardian performance fees.

- **Source**: [GitHub Repository](https://github.com/Cyfrin/8-vault-guardians-audit)
- **Audit Date**: May 16, 2026
- **Commit Hash**: 0600272180b6b6103f523b0ef65b070064158303
- **nSLOC**: ~800

---

## Protocol Overview

Vault Guardians is an ERC4626 vault system where fund managers ("vault guardians") manage user deposits across multiple DeFi protocols.

### Key Features:

- ERC4626 compliant vault architecture
- Integration with Aave V3 for lending
- Integration with Uniswap V2 for liquidity provision
- Guardian-managed allocation strategies (Hold / Uniswap / Aave)
- Performance fee model for guardians and DAO
- DAO governance for protocol parameters

### The protocol works as follows:

1. Users deposit ERC20 tokens into guardian-managed vaults
2. Guardians allocate funds across Aave, Uniswap, or hold positions
3. Guardians earn performance fees based on vault performance
4. Users can withdraw at any time (subject to vault liquidity)
5. DAO governs protocol parameters and receives a share of fees

---

## Actors

**Vault Guardian:**

- Powers: Can allocate funds across Aave, Uniswap, or hold positions, can update holding allocation at any time, earns performance fees.

- Limitations: Must stake tokens to become guardian, cannot move funds outside approved protocols.

**User / Depositor:**

- Powers: Can deposit tokens into guardian vaults, can withdraw at any time, can switch between different guardian vaults.

- Limitations: Subject to guardian allocation strategy, withdrawals depend on vault liquidity.

**DAO:**

- Powers: Can update guardian stake price, can update guardian/DAO fee cuts, receives share of guardian performance fees.

- Limitations: Governance decisions are subject to voting by VG token holders.

---

## Audit Scope

### In Scope

```
src/
#-- abstract/
    #-- AStaticTokenData.sol
    #-- AStaticUSDCData.sol
    #-- AStaticWethData.sol
#-- dao/
    #-- VaultGuardianGovernor.sol
    #-- VaultGuardianToken.sol
#-- interfaces/
    #-- InvestableUniverseAdapter.sol
    #-- IVaultData.sol
    #-- IVaultGuardians.sol
    #-- IVaultShares.sol
#-- protocol/
    #-- VaultGuardians.sol
    #-- VaultGuardiansBase.sol
    #-- VaultShares.sol
    #-- investableUniverseAdapters/
        #-- AaveAdapter.sol
        #-- UniswapAdapter.sol
#-- vendor/
    #-- DataTypes.sol
    #-- IUniswapV2Factory.sol
    #-- IUniswapV2Router01.sol
    #-- IPool.sol
```

- **Solidity Version**: 0.8.20
- **Chain**: Ethereum Mainnet

### Test Framework

- **Framework**: Foundry
- **Test Files**: Unit tests and PoC exploits for all findings
- **Time**: Significant time with 1 security researcher

---

## Executive Summary

Security audit of Vault Guardians protocol revealed **27 findings** across multiple severity levels:

| Severity  | Count  |
| --------- | ------ |
| HIGH      | 8      |
| MEDIUM    | 6      |
| LOW       | 9      |
| INFO      | 4      |
| **TOTAL** | **27** |

---

## Known Issues

During the audit, several design decisions and known limitations were identified:

- Hard dependency on Aave/Uniswap in withdrawal and deposit paths (inherent to ERC4626 yield-bearing vaults)
- USDC is behind a proxy and susceptible to being paused and upgraded (assumed not the case for this audit)
- All issues in the `audit-data` folder are considered known

---

## Related Links

- [Original Repository](https://github.com/Cyfrin/8-vault-guardians-audit)
- [Audit Report](./audit/report.pdf)
