<p align="center">
  <img src="https://github.com/StyLend/.github/blob/main/stylend-logo.webp" width="200" alt="StyLend Logo" />
</p>

<h1 align="center">StyLend</h1>

<p align="center">
  <strong>Lending & borrowing protocol built entirely on Arbitrum Stylus with Rust and WebAssembly.</strong>
</p>

StyLend is a decentralized lending & borrowing protocol built natively on [Arbitrum Stylus](https://docs.arbitrum.io/stylus/stylus-gentle-introduction) using Rust + WASM. The protocol supports multi-asset lending, isolated collateral vaults, dynamic interest rates, and cross-chain borrowing via [LayerZero](https://layerzero.network/). Currently deployed on Arbitrum Sepolia testnet.

---

## Highlights

- **23 smart contracts** written in Rust/Stylus (not Solidity)
- **EIP-1167 proxy pattern** — solved Stylus' 24 KiB WASM size constraint
- **Two-slope interest rate model** — dynamic pricing based on utilization
- **Per-user isolated Position contracts** — collateral vaults with DEX swap integration
- **Cross-chain borrowing via LayerZero** OFT bridges (Arbitrum <> Base)
- **Real-time indexer** — event-driven with historical snapshots & GraphQL API
- **Modern frontend** — Next.js 16, React 19, Three.js WebGL, GSAP animations

---

## Architecture

```
User ──► Frontend (Next.js) ──► Arbitrum Sepolia (Stylus Contracts)
              │                         │
              │ GraphQL                 │ Events
              ▼                         ▼
         Indexer (Ponder) ◄──── On-chain Events
              │
              ▼
         PostgreSQL (22 tables)
```

**How it works:**

1. **Smart Contracts** (Rust/Stylus) handle all on-chain logic — lending, borrowing, collateral management, liquidation, and cross-chain operations
2. **Indexer** (Ponder) listens to on-chain events, computes metrics (APY, utilization, TVL), stores data in PostgreSQL, and serves it via GraphQL
3. **Frontend** (Next.js) consumes data from two sources — direct RPC reads for real-time state, GraphQL for historical data & events

---

## Cross-Chain Borrowing

StyLend uses Arbitrum as the coordination and accounting layer while distributing liquidity across multiple chains. Users deposit collateral on Arbitrum and receive borrowed funds on destination chains — no manual bridging required.

```
Arbitrum (Collateral + Risk Engine)
    │
    │  LayerZero OFT
    ▼
Base / Other Chains (Liquidity Access)
```

All risk management — health factor calculations, liquidation logic, interest accrual — remains anchored on Arbitrum as a single source of truth.

---

## Repositories

| Repository | Description | Tech |
|:-----------|:------------|:-----|
| [**stylend-sc**](https://github.com/StyLend/stylend-sc) | Smart contracts — 23 Rust/Stylus contracts (LendingPool, Router, Factory, IRM, Position, IsHealthy, OFT bridges) | Rust, Stylus SDK, WASM |
| [**stylend-indexer**](https://github.com/StyLend/stylend-indexer) | Blockchain indexer — real-time event processing, APY calculations, historical snapshots, GraphQL API | TypeScript, Ponder, PostgreSQL |
| [**stylend-fe**](https://github.com/StyLend/stylend-fe) | Frontend — full DeFi interface with real-time data, charts, cross-chain UI, WebGL background | Next.js 16, React 19, wagmi, Three.js |

---

## Tech Stack

| Layer | Technology |
|:------|:-----------|
| Smart Contracts | Rust, Arbitrum Stylus SDK 0.9.0, WASM, LayerZero |
| Indexer | TypeScript, Ponder v0.16, PostgreSQL, GraphQL |
| Frontend | Next.js 16, React 19, TypeScript, wagmi/viem, Tailwind CSS 4, Three.js, GSAP |
| Network | Arbitrum Sepolia (testnet) |

## Supported Assets

USDC, USDT, WETH, WBTC (testnet)

---

## Key Contract Addresses (Arbitrum Sepolia)

| Contract | Address |
|:---------|:--------|
| LendingPoolFactory | `0x6ce797460987931a88d061d0cb2729af36e6e754` |
| TokenDataStream | `0x1145895ef3d3eb9f624acb8dc65ec40bd8e4cd39` |
| InterestRateModel | `0x1c1c290df8fe859778fe21eeada7a30c6d91587f` |
| IsHealthy | `0x04cf7500937c675c3b7e3fdff007523ada184fcf` |
| Protocol | `0x1017e1509e6ac180e4864bc46407a8bec70363d3` |
| StyLendEmitter | `0xf39ce77de228b30effb947fdab1ec2ac961212e7` |
| MockDex | `0xe91047615f53a62a5be5058fac7d890f3cc169ba` |
| Multicall | `0xe9a1adc452cd26cae2062d997a97a3800eaaeaa3` |

---

## Technical Innovations

### EIP-1167 Minimal Proxy Clones
Stylus contracts require `cargo stylus deploy` with brotli-compressed WASM — making traditional factory patterns impossible. StyLend deploys each implementation once, then creates 45-byte EVM clone proxies that `DELEGATECALL` into the WASM implementation. Each clone has independent storage but shares execution logic.

### WASM Size Optimization
Every contract must fit within 24 KiB compressed. Techniques: raw ABI calls instead of macros, 4-byte error selectors, manual Ownable/ReentrancyGuard, aggressive compiler tuning (`opt-level = "z"`, `lto = true`, `strip = true`).

### Call Depth Optimization
Stylus enforces a max nested WASM call depth of 3. StyLend flattens call chains by having callers look up all needed data first and pass it as parameters, reducing call depth from 4-5 to 2-3 levels.

---

## Links

- [Documentation](https://stylend.gitbook.io/stylend-docs)
- [API](https://api.stylend.xyz)

---

## License

MIT / Apache-2.0
