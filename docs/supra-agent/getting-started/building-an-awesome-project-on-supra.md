# Building an Awesome Project on Supra

> High-level onboarding guide for new builders on Supra — social presence, smart contracts, vertical stack overview, performance, frontend, and ship checklist.

**Domain:** engineering · **Version:** 1.0.0 · **By:** Yong (Supra Devrel)

**Keywords:** `supra` `getting started` `build` `project` `dapp` `onboarding` `checklist` `move` `smart contract`

[View on Kavela Marketplace](https://kavela.ai/marketplace/supra-agent)

---
# Building an Awesome Project on Supra — Getting Started
## 1. Establish Your Social Presence
Your project needs visibility in the Supra ecosystem from day one.
- Create and maintain an X (Twitter) account for your project.
- Post regularly about your development progress, milestones, and learnings.
- Tag @Supra_Labs on Twitter in your posts. This signals you're building in the ecosystem and opens doors to ecosystem support, funding, marketing help, and developer relations engagement.
- Community building starts on day one — don't wait until your product is ready.
## 2. Build Provably Working Smart Contracts
A project is never valid until you have provably working smart contracts deployed on the Supra chain. Ideas and mockups are not enough — you must demonstrate functional on-chain code.
Supra uses the Move programming language (based on the Aptos standard). The core framework is maintained here:
- Framework repo: https://github.com/Entropy-Foundation/aptos-core/tree/dev/aptos-move/framework
This repo contains all the modules created and maintained by Supra, including .move files with functions and their usage. Focus on:
- public fun — Public functions callable by other modules
- public entry fun — Entry functions callable directly from transactions (these are what your frontend connects to)
### Developer Tooling
- Supra IDE — Web-based development environment
- Supra Move VS Code Extension — For local development
- Supra dApp Templates — Starter templates to bootstrap your project
- AutoFi Assist — Tooling for automation-powered contracts
## 3. Adopt the Supra Vertical Stack
What makes a Supra dApp strong is adopting the vertically integrated stack. Supra is the world's first fully vertically integrated L1 — all services are native protocol primitives, not bolted-on third-party add-ons.
The stack includes:
- Supra dVRF — Tamper-proof, decentralized verifiable randomness for GameFi, lotteries, fair minting
- Supra Oracles — Native real-time price feeds with sub-second freshness, 600+ feeds across crypto/RWA/equities
- AutoFi — System-level automation engine: smart contracts execute autonomously on conditions, zero-block delay, no keepers needed
- SupraNova — Bridgeless cross-chain L1-to-L1 communication without relays or multisigs
> See dedicated skills for deep dives on each component.
## 4. Performance Advantage — 500k TPS
Supra achieves 500,000 TPS via Moonshot consensus (formally verified with Microsoft's IVy Verifier). This changes what's possible on chain:
- Automatic finance at scale (AutoFi scanning block-by-block)
- AI agent architectures setting parameters and running on-chain scripts in real-time
- Complex on-chain logic that would hit execution limits elsewhere
- Real-time gaming, trading, social with sub-second finality
## 5. Build Your Frontend
- Recommended stack: Next.js 14 + TailwindCSS v3
- Wallet connector: Supra Multi-Wallet Connector — https://github.com/Crystara-Markets/supra-multiwallet (production-ready, used on Crystara marketplace)
- Indexing: Required for production. On-chain you can't iterate lists > ~200 items. Use event emission → off-chain indexer pattern.
> See dedicated skills for frontend setup, wallet integration, and indexer architecture.
## 6. Ship Checklist
| Step | What | Status ||------|------|--------|| 1 | X/Twitter account, actively posting, tagging @Supra_Labs | Required || 2 | Working smart contracts deployed on Supra (Move) | Required || 3 | Adopt vertical stack: dVRF, Oracles, AutoFi, SupraNova | Strongly recommended || 4 | Frontend: Next.js 14 + TailwindCSS v3 | Recommended stack || 5 | Wallet connector: supra-multiwallet | Required || 6 | Indexer: Custom or Crystara's NFT indexer | Required for production |
## Key Links
- Supra Move Framework: https://github.com/Entropy-Foundation/aptos-core/tree/dev/aptos-move/framework
- Supra Multi-Wallet: https://github.com/Crystara-Markets/supra-multiwallet
- Crystara NFT Indexer API: https://docs.crystara.trade/products/marketplace-api/api-overview
- Supra Developer Docs: https://docs.supra.com
