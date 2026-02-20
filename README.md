# .github

Central repository for StyLend's GitHub organization configuration, development briefs, and community templates.

## What's in this repo

```
.github/
├── profile/
│   └── README.md              # Organization profile (shown on github.com/StyLend)
├── sc-brief/
│   ├── CONTRIBUTING.md        # Contribution guide for smart contracts
│   ├── SECURITY.md            # Security vulnerability reporting policy
│   ├── pull_request_template.md
│   └── ISSUE_TEMPLATE/
│       ├── bug_report.md
│       └── feature_request.md
├── indexer-brief/
│   └── BRIEF.md               # Indexer project brief & requirements
├── frontend-brief/
│   └── FE_BRIEF.md            # Frontend project brief & org profile spec
└── README.md                  # This file
```

## Related Repositories

| Repository | Description | Links |
|:-----------|:------------|:------|
| [stylend-sc](https://github.com/StyLend/stylend-sc) | 23 Rust/Stylus smart contracts — LendingPool, Router, Factory, IRM, Position, cross-chain bridges | [README](https://github.com/StyLend/stylend-sc/blob/main/README.md) |
| [stylend-indexer](https://github.com/StyLend/stylend-indexer) | Real-time on-chain indexer — Ponder, PostgreSQL, GraphQL API, APY/TVL calculations | [README](https://github.com/StyLend/stylend-indexer/blob/main/README.md) |
| [stylend-fe](https://github.com/StyLend/stylend-fe) | Frontend DeFi interface — Next.js 16, React 19, wagmi, Three.js WebGL, cross-chain UI | [README](https://github.com/StyLend/stylend-fe/blob/main/README.md) |

## Community Templates

The templates in `sc-brief/` are used across the organization:

- **CONTRIBUTING.md** — Setup guide, WASM size limits, call depth rules, and submission process
- **SECURITY.md** — Responsible disclosure policy (email: security@stylend.xyz)
- **Pull Request Template** — Checklist for builds, contract size, and security
- **Issue Templates** — Bug reports and feature requests with structured fields

## Documentation

- [StyLend Docs (GitBook)](https://stylend.gitbook.io/stylend-docs)
- [Cross-Chain Borrowing](https://stylend.gitbook.io/stylend-docs/our-products/cross-chain-borrowing)
- [API](https://api.stylend.xyz)
