# Light Protocol Examples

Overview to code examples for building with Light Token and the core primtive ZK Compression on Solana.
- ZK Compression is a Solana framework that reduces the cost of token accounts and PDAs by 99%. 
- For tokens, the Light Token SDK keeps code changes minimal and is a superset of the SPL-token API, i.e. supports every `spl-token` instructions and additional instructions for interoperability.

**Cost Comparison** 
| Account Type | Light | Standard Solana |
|---|---|---|
| Mint account | ~0.000091 SOL | ~0.0015 SOL |
| Token account | 0.000017 SOL | ~0.0029 SOL |
| PDA (100 bytes) | ~0.000012 SOL | ~0.0016 SOL |

Learn more: [Overview](https://www.zkcompression.com/learn/overview) | [FAQ](https://www.zkcompression.com/faq)

Install [agent skills](https://github.com/Lightprotocol/skills):
```bash
npx skills add Lightprotocol/skills
```

## Contents

- [Token Examples](#light-token)
  - [Toolkits](#toolkits)
    - [Payments](#toolkits)
    - [Streaming Tokens](#toolkits)
    - [Sign with Privy](#toolkits)
    - [Sign with Wallet Adapter](#toolkits)
    - [Gasless Transactions](#toolkits)
    - [Token 2022 Extensions](#toolkits)
  - [TypeScript Client](#typescript-client)
  - [Rust Client](#rust-client)
  - [Program Examples](#program-examples)
    - [escrow](#program-examples)
    - [fundraiser](#program-examples)
    - [light-token-minter](#program-examples)
    - [token-swap](#program-examples)
    - [cp-swap-reference](#program-examples)
    - [pinocchio-swap](#program-examples)
    - [create-and-transfer](#program-examples)
  - [Anchor Macros](#anchor-macros)
  - [Basic Instructions](#basic-instructions)
  - [Token Distribution](#token-distribution)
    - [simple-claim](#token-distribution)
    - [merkle-distributor](#token-distribution)
    - [token-distribution](#token-distribution)
- [PDA Accounts](#pda-accounts)
  - [Basic Operation Programs](#basic-operation-programs)
    - [create](#basic-operation-programs)
    - [update](#basic-operation-programs)
    - [close](#basic-operation-programs)
    - [reinit](#basic-operation-programs)
    - [burn](#basic-operation-programs)
  - [Nullifier Program](#nullifier-program), e.g. for payments
  - [Counter Program](#counter-program)
    - [anchor](#counter-program)
    - [native](#counter-program)
    - [pinocchio](#counter-program)
  - [Read-only](#read-only)
  - [Create and Update](#create-and-update)
  - [Compare Solana vs Compressed Account](#compare-solana-vs-compressed-account)
  - [ZK Programs](#zk-programs)
    - [zk-id](#zk-programs)
    - [nullifier](#zk-programs)
- [Documentation & AI](#documentation--ai)

## Light Token

### Toolkits

|  | Description |
|---------|-------------|
| [Payments](https://github.com/Lightprotocol/examples-light-token/tree/main/toolkits/payments/) | All you need for wallet integrations and payment flows. Minimal API differences to SPL. |
| [Streaming Tokens](https://github.com/Lightprotocol/examples-light-token/tree/main/toolkits/streaming-tokens/) | Stream mint events using Laserstream |
| [Sign with Privy](https://github.com/Lightprotocol/examples-light-token/tree/main/toolkits/sign-with-privy/) | Light-token operations signed with Privy wallets (Node.js + React) |
| [Sign with Wallet Adapter](https://github.com/Lightprotocol/examples-light-token/tree/main/toolkits/sign-with-wallet-adapter/) | Sign light-token transactions with Wallet Adapter (React) |
| [Gasless Transactions](https://github.com/Lightprotocol/examples-light-token/tree/main/toolkits/gasless-transactions/) | Abstract SOL fees so users never hold SOL. Set your application as the fee payer. Sponsor rent top-ups and transaction fees. |
| [Token 2022 Extensions](https://github.com/Lightprotocol/examples-light-token/tree/main/extensions/) | Create mints with Token 2022 extensions and register them for Light Token |

### TypeScript Client

|  |  |  | Description |
|---------|--------|-------------|-------------|
| create-mint | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/actions/create-mint.ts) | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/instructions/create-mint.ts) | Create a light-token mint with metadata |
| create-spl-mint | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/actions/create-spl-mint.ts) | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/instructions/create-spl-mint.ts) | Create an SPL mint with SPL interface PDA |
| create-t22-mint | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/actions/create-t22-mint.ts) | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/instructions/create-t22-mint.ts) | Create a Token 2022 mint with SPL interface PDA |
| create-spl-interface | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/actions/create-spl-interface.ts) | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/instructions/create-spl-interface.ts) | Register SPL interface PDA for an existing mint |
| create-ata | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/actions/create-ata.ts) | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/instructions/create-ata.ts) | Create an associated light-token account |
| create-ata-explicit-rent-sponsor | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/actions/create-ata-explicit-rent-sponsor.ts) | | Create an ATA with explicit rent sponsor |
| load-ata | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/actions/load-ata.ts) | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/instructions/load-ata.ts) | Load token accounts from light-token, compressed tokens, SPL/T22 to one unified balance |
| mint-to | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/actions/mint-to.ts) | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/instructions/mint-to.ts) | Mint tokens to a light-account |
| transfer-interface | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/actions/transfer-interface.ts) | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/instructions/transfer-interface.ts) | Transfer between light-token, T22, and SPL accounts |
| wrap | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/actions/wrap.ts) | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/instructions/wrap.ts) | Wrap SPL/T22 to light-token |
| unwrap | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/actions/unwrap.ts) | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/instructions/unwrap.ts) | Unwrap light-token to SPL/T22 |
| approve | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/actions/delegate-approve.ts) | | Approve delegate |
| revoke | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/actions/delegate-revoke.ts) | | Revoke delegate |
| delegate-transfer | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/typescript-client/actions/delegate-transfer.ts) | | Delegate transfers tokens on behalf of owner |

### Rust Client

|  |  |  | Description |
|---------|--------|-------------|-------------|
| create-mint | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/actions/create_mint.rs) | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/instructions/create_mint.rs) | Create a light-token mint with metadata |
| create-ata | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/actions/create_associated_token_account.rs) | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/instructions/create_associated_token_account.rs) | Create an associated light-token account |
| create-token-account | | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/instructions/create_token_account.rs) | Create a light-token account with custom owner |
| mint-to | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/actions/mint_to.rs) | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/instructions/mint_to.rs) | Mint tokens to a light-account |
| mint-to-checked | | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/instructions/mint_to_checked.rs) | Mint tokens with decimal validation |
| transfer-interface | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/actions/transfer_interface.rs) | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/instructions/transfer_interface.rs) | Transfer between light-token, T22, and SPL accounts |
| spl-to-light-transfer | | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/instructions/spl_to_light_transfer.rs) | Transfer from SPL to Light via TransferInterface |
| wrap | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/actions/wrap.rs) | | Wrap SPL/T22 to light-token |
| unwrap | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/actions/unwrap.rs) | | Unwrap light-token to SPL/T22 |
| burn | | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/instructions/burn.rs) | Burn tokens |
| burn-checked | | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/instructions/burn_checked.rs) | Burn tokens with decimal validation |
| approve | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/actions/approve.rs) | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/instructions/approve.rs) | Approve delegate |
| revoke | [Action](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/actions/revoke.rs) | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/instructions/revoke.rs) | Revoke delegate |
| freeze | | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/instructions/freeze.rs) | Freeze a token account |
| thaw | | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/instructions/thaw.rs) | Thaw a frozen token account |
| close | | [Instruction](https://github.com/Lightprotocol/examples-light-token/tree/main/rust-client/instructions/close.rs) | Close a token account |

### Program Examples

|  | Description |
|---------|-------------|
| [escrow](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/escrow) | Peer-to-peer light-token swap with offer/accept flow |
| [fundraiser](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/fundraiser) | Token fundraiser with target, deadline, and refunds |
| [light-token-minter](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/light-token-minter) | Create light-mints with metadata, mint tokens |
| [token-swap](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/token-swap) | AMM with liquidity pools and swaps (Anchor) |
| [cp-swap-reference](https://github.com/Lightprotocol/cp-swap-reference/tree/954f679699dbdfe9711308c8ae9fbd21e69db8aa) | Fork of Raydium AMM that creates markets without paying rent-exemption |
| [pinocchio-swap](https://github.com/Lightprotocol/examples-light-token/tree/main/pinocchio/swap) | AMM with liquidity pools and swaps (Pinocchio) |
| [create-and-transfer](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/create-and-transfer) | Create account via macro and transfer via CPI |

### Anchor Macros

|  | Description |
|---------|-------------|
| [counter](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/basic-macros/counter) | Create, increment and close counter PDA with sponsored rent-exemption |
| [create-associated-token-account](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/basic-macros/create-associated-token-account) | Create associated light-token account |
| [create-mint](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/basic-macros/create-mint) | Create light-token mint |
| [create-token-account](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/basic-macros/create-token-account) | Create light-token account |

### Basic Instructions

The instructions use pure CPI calls which you can combine with existing and / or light macros. For existing programs, you can replace spl_token with light_token instructions as you need. The API is a superset of SPL-token so switching is straightforward.

|  | Description |
|---------|-------------|
| [approve](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/basic-instructions/approve/src/lib.rs) | Approve delegate via CPI |
| [burn](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/basic-instructions/burn/src/lib.rs) | Burn tokens via CPI |
| [close](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/basic-instructions/close/src/lib.rs) | Close token account via CPI |
| [create-associated-token-account](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/basic-instructions/create-associated-token-account/src/lib.rs) | Create associated light-token account via CPI |
| [create-mint](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/basic-instructions/create-mint/src/lib.rs) | Create light-token mint via CPI |
| [create-token-account](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/basic-instructions/create-token-account/src/lib.rs) | Create light-token account via CPI |
| [freeze](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/basic-instructions/freeze/src/lib.rs) | Freeze token account via CPI |
| [mint-to](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/basic-instructions/mint-to/src/lib.rs) | Mint tokens via CPI |
| [revoke](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/basic-instructions/revoke/src/lib.rs) | Revoke delegate via CPI |
| [thaw](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/basic-instructions/thaw/src/lib.rs) | Thaw token account via CPI |
| [transfer-checked](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/basic-instructions/transfer-checked/src/lib.rs) | Transfer with mint validation via CPI |
| [transfer-interface](https://github.com/Lightprotocol/examples-light-token/tree/main/programs/anchor/basic-instructions/transfer-interface/src/lib.rs) | Transfer between light-token, T22, and SPL accounts via CPI |

### Token Distribution

|  | Description |
|---------|-------------|
| [simple-claim](https://github.com/Lightprotocol/program-examples/tree/main/airdrop-implementations/simple-claim) | Distributes compressed tokens that get decompressed to SPL on claim with cliff |
| [merkle-distributor](https://github.com/Lightprotocol/distributor) | Distributes SPL tokens, uses compressed PDAs to track claims with linear vesting, partial claims and clawback. Based on Jito Merkle distributor and optimized with rent-free PDAs. |
| [token-distribution](https://github.com/Lightprotocol/example-token-distribution) | Client reference implementations for common token distribution flows with ZK Compression (airdrops and rewards) |

## PDA Accounts

### Basic Operation Programs

- **create** - Initialize a new compressed account
  - [Anchor](https://github.com/Lightprotocol/program-examples/tree/main/basic-operations/anchor/create) | [Native](https://github.com/Lightprotocol/program-examples/tree/main/basic-operations/native/programs/create)
- **update** - Modify data in an existing compressed account
  - [Anchor](https://github.com/Lightprotocol/program-examples/tree/main/basic-operations/anchor/update) | [Native](https://github.com/Lightprotocol/program-examples/tree/main/basic-operations/native/programs/update)
- **close** - Clear account data and preserve its address
  - [Anchor](https://github.com/Lightprotocol/program-examples/tree/main/basic-operations/anchor/close) | [Native](https://github.com/Lightprotocol/program-examples/tree/main/basic-operations/native/programs/close)
- **reinit** - Reinitialize a closed account with the same address
  - [Anchor](https://github.com/Lightprotocol/program-examples/tree/main/basic-operations/anchor/reinit) | [Native](https://github.com/Lightprotocol/program-examples/tree/main/basic-operations/native/programs/reinit)
- **burn** - Permanently delete a compressed account
  - [Anchor](https://github.com/Lightprotocol/program-examples/tree/main/basic-operations/anchor/burn) | [Native](https://github.com/Lightprotocol/program-examples/tree/main/basic-operations/native/programs/burn)

### Nullifier Program

|  | Description |
|---------|-------------|
| [nullifier-program](https://github.com/Lightprotocol/nullifier-program) | Simple program to prevent your onchain instruction from being executed more than once, e.g. for payments. |
| [nullifier-program/examples](https://github.com/Lightprotocol/nullifier-program/tree/main/examples) | Example client usage for creating nullifiers |

> View a nullifier example for [ZK Solana programs here](#zk-programs).

### Counter Program

Full compressed account lifecycle (create, increment, decrement, reset, close):

- [counter/anchor](https://github.com/Lightprotocol/program-examples/tree/main/counter/anchor/) - Anchor program with Rust and TypeScript tests
- [counter/native](https://github.com/Lightprotocol/program-examples/tree/main/counter/native/) - Native Solana program with light-sdk and Rust tests.
- [counter/pinocchio](https://github.com/Lightprotocol/program-examples/tree/main/counter/pinocchio/) - Pinocchio program with light-sdk-pinocchio and Rust tests.

### Read-only

- [read-only](https://github.com/Lightprotocol/program-examples/tree/main/read-only) - Create a new compressed account and read it onchain.

### Create and Update
- [create-and-update](https://github.com/Lightprotocol/program-examples/tree/main/create-and-update/) - Create a new compressed account and update an existing compressed account with a single validity proof in one instruction.

### Compare Solana vs Compressed Account
- [account-comparison](https://github.com/Lightprotocol/program-examples/tree/main/account-comparison/) - Compare compressed vs regular Solana accounts.

### ZK Programs

- [zk-id](https://github.com/Lightprotocol/program-examples/tree/main/zk/zk-id) - Identity verification using Groth16 proofs. Issuers create credentials; users prove ownership without revealing the credential.
- [nullifier](https://github.com/Lightprotocol/program-examples/tree/main/zk/nullifier) - Simple Program to prevent double-spending for ZK Solana programs.

> View a nullifier example e.g. for [payments here](#nullifier-program).

## Documentation & AI

- [Light Token docs](https://www.zkcompression.com/light-token/welcome)
- [ZK Compression docs](https://www.zkcompression.com/learn/overview)
- [llms.txt](https://www.zkcompression.com/llms.txt)
- AI agent skills: [github.com/Lightprotocol/skills](https://github.com/Lightprotocol/skills) | Install: `npx skills add https://zkcompression.com`
