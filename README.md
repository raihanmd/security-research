# Smart Contract Audit Collection

> A curated collection of smart contract security audits from various sources including CodeHawks contests, public audits, and private audits.

## Overview

This repository contains security audit reports for various blockchain protocols. Each audit folder represents a separate project that has been reviewed for security vulnerabilities, gas optimization opportunities, and code quality improvements.

## Repository Structure

```
.
├── README.md
└── <project-name>/
    ├── README.md          # Project details and protocol description
    └── audit/
        ├── report.md       # Full audit report (markdown)
        └── report.pdf      # Full audit report (PDF)
```

## Audited Projects

| Project                                 | Source    | Date       | Status  |
| --------------------------------------- | --------- | ---------- | ------- |
| [NFT Dealers](./2026-03-13_NFT-Dealer/) | CodeHawks | March 2026 | Audited |

## Audit Standards

Each audit entry includes:

- **README.md** - Project overview containing:
  - External link to original contest/source
  - Protocol description
  - Actors and their roles
  - Contract scope
  - Setup instructions
  - Known issues (if any)

- **audit/report.md** - Detailed findings documented in markdown
- **audit/report.pdf** - Formal PDF report

## Sources

Audits are collected from:

- **CodeHawks** - Competitive audit contests
- **Public Audits** - Publicly disclosed security reviews
- **Private Audits** - Confidential engagement audits (with permission)

## Contributing

To add a new audit:

1. Create a new folder with project name: `<project-name>/`
2. Add `README.md` with project details
3. Create `audit/` subfolder with `report.md` and `report.pdf`
4. Update this README with the new entry

## Disclaimer

These audits are performed by various security researchers and may represent different timeframes, scopes, and methodologies. Findings should be reviewed in context of their respective audit dates and scope definitions.

The pdf is generated with pandoc

```cmd
pandoc <file>.md -o report.pdf --from markdown --template=eisvogel --listings
```
