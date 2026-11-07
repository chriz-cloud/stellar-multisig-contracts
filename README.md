# Stellar Multisig Contracts

On-chain Soroban smart contracts for M-of-N multisig accounts on Stellar. A custom account contract that requires M signatures from N registered signers to authorize any action (token transfers, contract calls, etc.).

## Overview

This repository contains:

1. **Multisig Account Contract** - A Soroban custom account implementing `__check_auth` for M-of-N signature verification
2. **Account Factory Contract** - Deploys new multisig accounts with configurable signers and thresholds

## Features

- ✅ M-of-N signature threshold enforcement
- ✅ Self-custodial signer management (add/remove signers requires multisig approval)
- ✅ Dynamic threshold adjustment (with multisig approval)
- ✅ Protection against signature replay and duplicate counting
- ✅ Factory pattern for easy deployment
- ✅ Event emission for all state changes

## Repository Structure

```
stellar-multisig-contracts/
├── contracts/
│   ├── multisig-account/     # Custom account with __check_auth
│   └── account-factory/      # Factory for deploying accounts
├── docs/
│   ├── multisig-account.md   # Deep dive into the account contract
│   └── account-factory.md    # Factory contract documentation
├── scripts/
│   └── deploy.sh             # Deployment automation
├── DEPLOYMENTS.md            # Testnet/mainnet addresses
├── CONTRIBUTING.md           # Development guide
└── README.md
```

## Quick Start

### Prerequisites

- Rust 1.74+
- Soroban CLI (`cargo install soroban-cli`)
- Stellar account with testnet XLM

### Build

```bash
cargo build --release --target wasm32-unknown-unknown
```

### Test

```bash
cargo test
```

### Deploy

See [DEPLOYMENTS.md](./DEPLOYMENTS.md) for deployed contract addresses.

```bash
./scripts/deploy.sh
```

## How It Works

Soroban custom accounts implement the `__check_auth` function, which is called by the host whenever `require_auth()` is invoked on the contract's address. The multisig account contract:

1. Stores a list of authorized signer public keys
2. Stores a threshold M (minimum signatures required)
3. In `__check_auth`, verifies that at least M valid signatures are present
4. Ensures signatures are sorted and deduplicated (prevents double-counting)

Any action requiring authorization (transfers, contract invocations, signer changes) goes through this same verification flow.

## Sister Repositories

- **Web Interface**: [stellar-multisig-web](https://github.com/<your-org>/stellar-multisig-web)
- **API & Documentation**: [stellar-multisig-api-docs](https://github.com/<your-org>/stellar-multisig-api-docs)

## Security

- All admin functions require multisig authorization
- Signature ordering enforced to prevent replay attacks
- Threshold constraints validated at all times
- No panics - all errors handled via error enums

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for development guidelines, testing procedures, and architecture details.

## License

MIT
