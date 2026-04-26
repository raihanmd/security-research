# Boss Bridge

> Boss Bridge is a bridging mechanism to move an ERC20 token (the "Boss Bridge Token" or "BBT") from L1 to an L2. The L2 part of the bridge is still under construction, so only the L1 components are included in this audit.

The bridge allows users to deposit tokens, which are held in a secure vault on L1. Successful deposits trigger an event that an off-chain mechanism picks up to mint corresponding tokens on L2. Withdrawals must be approved by a bridge operator (signer).

Security mechanisms in place include owner-controlled pause/unpause, strict deposit limits, and operator-signed withdrawals.

- **Source**: [GitHub Repository](https://github.com/Cyfrin/7-boss-bridge-audit)
- **Audit Date**: April 26, 2026
- **Commit Hash**: 07af21653ab3e8a8362bf5f63eb058047f562375
- **nSLOC**: ~450

---

## Protocol Overview

Boss Bridge is a simple bridge mechanism to move an ERC20 token from L1 to an L2. The L2 part of the bridge is still under construction, so only the L1 components were reviewed.

### Key Features:

- Deposit tokens to L2 via the bridge contract
- Withdrawals require operator (signer) approval
- Owner-controlled pause/unpause for emergency situations
- Strict token deposit limit to mitigate risk
- Vault-based token custody on L1

### The protocol works as follows:

1. Users who want to move tokens from L1 to L2 deposit their tokens using `depositTokensToL2`.
2. The bridge transfers tokens from the user to the L1Vault and emits a `Deposit` event.
3. An off-chain service monitors for deposit events and mints corresponding tokens on L2.
4. Users who want to withdraw from L2 to L1 submit a withdrawal request.
5. The bridge operator signs the withdrawal request after validating it off-chain.
6. The user calls `withdrawTokensToL1` with the operator's signature to receive their tokens.

---

## Actors

**User / Depositor:**

- Powers: Can deposit tokens to L2 via the bridge, can withdraw tokens from L1 with a valid operator signature.

- Limitations: Cannot withdraw without valid operator signature, cannot bypass deposit limits.

**Operator / Signer:**

- Powers: Can sign withdrawal requests enabling users to withdraw their tokens from L1.

- Limitations: Must validate that withdrawal requests correspond to legitimate L2 deposits.

**Bridge Owner:**

- Powers: Can pause/unpause the bridge, can add/remove operators (signers), can emergency-withdraw funds. Owner is trusted.

- Limitations: Single point of trust - compromised owner key puts all funds at risk.

---

## Audit Scope

### In Scope

```
src/
#-- L1BossBridge.sol
#-- L1Token.sol
#-- L1Vault.sol
#-- TokenFactory.sol
```

- **Solidity Version**: 0.8.20
- **Chain**: Ethereum Mainnet, ZKSync Era

### Test Framework

- **Framework**: Foundry
- **Test Files**: Unit tests and PoC exploits for all findings
- **Time**: 4 hours with 1 security researcher

---

## Executive Summary

Security audit of Boss Bridge protocol revealed **19 findings** across multiple severity levels:

| Severity  | Count |
| --------- | ----- |
| HIGH      | 8     |
| MEDIUM    | 1     |
| LOW       | 7     |
| INFO      | 3     |
| **TOTAL** | **19** |

---

## Known Issues

During the audit, several design decisions and known limitations were identified:

- The bridge is centralized and owned by a single user.
- Missing zero-address checks are intentionally omitted to save gas.
- Magic numbers are defined as literals rather than named constants.
- Only L1Token.sol (or copies) is assumed to be used with the bridge.

---

## Related Links

- [Original Repository](https://github.com/Cyfrin/7-boss-bridge-audit)
- [Audit Report](./audit/report.md)