# SNARKeling

> SNARKeling Treasure Hunt is a real-world snorkeling treasure hunt with on-chain reward claiming on an EVM blockchain. Participants physically find hidden treasures and then submit a zero-knowledge proof showing they know the correct treasure secret, without revealing the secret itself. The protocol verifies the proof on-chain and pays out an ETH reward to the designated recipient. This mechanism is built around a Noir circuit and a generated Barretenberg Honk verifier contract (more theory here https://updraft.cyfrin.io/courses/noir-programming-and-zk-circuits).

- **Source**: [GitHub Repository](https://github.com/CodeHawks-Contests/2026-04-snarkeling)
- **Audit Date**: April 23, 2026
- **Commit Hash**: aed232ce6c1ce3f909b68f1ae67978b3bfb82408
- **nSLOC**: ~220

---

## Protocol Overview

SNARKeling Treasure Hunt is a real-world snorkeling treasure hunt with on-chain reward claiming on an EVM blockchain. Participants physically find hidden treasures and then submit a zero-knowledge proof showing they know the correct treasure secret, without revealing the secret itself. The protocol verifies the proof on-chain and pays out an ETH reward to the designated recipient. This mechanism is built around a Noir circuit and a generated Barretenberg Honk verifier contract (more theory here https://updraft.cyfrin.io/courses/noir-programming-and-zk-circuits).

### Key Features:

- Real-world treasure hunt with blockchain-based reward settlement

- ZK-SNARK based proof verification for treasure discovery

- Noir circuit that proves knowledge of a valid treasure secret without revealing it

- On-chain ETH reward distribution to a recipient bound into the proof

- Replay-resistance through recipient binding as a public input

- Owner-controlled pause/unpause, verifier update, emergency withdrawal, and post-hunt fund withdrawal flows

### The protocol works as follows:

1.  The organizer deploys the verifier and `TreasureHunt` contract and funds the hunt with ETH.

2.  A participant finds a physical treasure associated with a unique secret string.

3.  Off-chain, the participant generates a ZK proof that:
    - they know a valid treasure secret,
    - its Pedersen hash matches one of the allowed treasure hashes baked into the circuit,
    - and the proof is bound to a specific recipient address.

4.  The participant submits the proof, treasure hash, and recipient to the `TreasureHunt` contract.

5.  If the proof is valid and the treasure has not already been claimed, the contract transfers the fixed ETH reward to the recipient and marks the treasure as claimed.

---

## Actors

**Participant / Treasure Finder:**

- Powers: Can submit a ZK proof to claim a treasure reward for a valid recipient address.

- Limitations: Cannot claim with an invalid proof, cannot claim an already-claimed treasure, cannot claim if the contract lacks sufficient funds, and cannot use invalid recipients such as the zero address, the contract address, the owner, or the caller itself.

**Owner / Hunt Organizer:**

- Powers: Deploys and funds the hunt, pauses/unpauses the contract, updates the verifier while paused, emergency-withdraws ETH while paused, and withdraws leftover funds after all treasures have been claimed. Owner is trusted.

- Limitations: Cannot claim treasure rewards as a participant and cannot set certain invalid recipients in emergency flows.

---

## Audit Scope

### In Scope

```
contracts/
#-- scripts/
    #-- Deploy.s.sol
#-- src/
    #-- TreasureHunt.sol

circuits/
#-- src/
    #-- main.nr
```

- **Solidity Version**: ^0.8.27
- **Chain**: Ethereum

### Test Framework

- **Framework**: Foundry
- **Test Files**: Unit tests and PoC exploits for all findings
- **Time**: 4 hours with 1 security researcher

---

## Executive Summary

Security audit of ThunderLoan flash loan protocol revealed **12 findings** across multiple severity levels:

| Severity  | Count |
| --------- | ----- |
| HIGH      | 1     |
| MEDIUM    | 1     |
| LOW       | 2     |
| INFO      | 1     |
| **TOTAL** | **5** |

## Related Links

- [Original Repository](https://github.com/CodeHawks-Contests/2026-04-snarkeling)
- [Audit Report](./audit/report.md)
