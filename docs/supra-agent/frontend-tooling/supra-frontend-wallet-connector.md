# Supra Frontend & Wallet Connector

> Frontend stack recommendation for Supra dApps — Next.js 14, TailwindCSS v3, and the Crystara supra-multiwallet connector with integration patterns.

**Domain:** engineering · **Version:** 1.0.0 · **By:** Yong (Supra Devrel)

**Keywords:** `frontend` `nextjs` `tailwind` `wallet` `wallet connector` `multiwallet` `crystara` `starkey` `supra` `dapp`

[View on Kavela Marketplace](https://kavela.ai/marketplace/supra-agent)

---
# Supra Frontend Stack & Wallet Connector

## Recommended Frontend Stack

### Framework: Next.js 14
- React-based framework with server-side rendering, API routes, and excellent developer experience.
- Use the **App Router** (default in Next.js 14) for modern patterns.
- API routes are useful for server-side operations like indexer queries.

### Styling: TailwindCSS v3
- Utility-first CSS framework for rapid, consistent UI development.
- Use v3 specifically — this is the battle-tested version in the Supra ecosystem.

This is the combination used by leading projects on Supra, including Crystara (the premier NFT marketplace).

## Wallet Connector — Required Component

Every Supra dApp needs a wallet connector. Without it, users can't interact with on-chain functions.

### Recommended: Supra Multi-Wallet Connector
- **Repo**: https://github.com/Crystara-Markets/supra-multiwallet
- **Production-ready** — this is the exact connector running on Crystara's mainnet marketplace.
- Supports **multiple wallet providers** (StarKey and others).
- Community-supported and actively maintained.

### How It Works in Your App
1. **User clicks an action button** (e.g., "Mint NFT", "Swap Tokens", "Stake")
2. **Register the on-chain action** — specify which `entry fun` you want called and with what parameters
3. **Transaction sent to wallet** — the connector packages the transaction and sends it to the user's wallet for signing
4. **User signs** — they approve the transaction in their wallet
5. **Transaction submitted and executed** — the signed transaction hits Supra's chain and your `entry fun` runs

### Integration Pattern
```
Action Button → Register Entry Function Call → Wallet Signing → On-Chain Execution
```

Your frontend maps UI actions to Move `public entry fun` functions. The wallet connector handles the signing and submission pipeline.

## Developer Tooling
- **Supra IDE**: Web-based Move development environment
- **Supra Move VS Code Extension**: Local development with syntax highlighting, error checking
- **Supra dApp Templates**: Starter templates to bootstrap your project structure
- **AutoFi Assist**: No-code automation creation tool on mainnet
