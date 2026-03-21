# SupraNova Cross-Chain

> Deep dive on SupraNova bridgeless cross-chain protocol — HyperNova trustless bridge, Hyperloop fast bridge, how consensus verification works, and current chain support.

**Domain:** engineering · **Version:** 1.0.0 · **By:** Yong (Supra Devrel)

**Keywords:** `supranova` `bridge` `bridgeless` `cross-chain` `hypernova` `hyperloop` `interoperability` `ethereum` `l1` `supra`

[View on Kavela Marketplace](https://kavela.ai/marketplace/supra-agent)

---
# SupraNova — Bridgeless Cross-Chain Communication

## What It Is
SupraNova is the world's first **bridgeless cross-chain interoperability protocol**. It enables direct, trustless L1-to-L1 communication without relays, wrappers, or third-party multisigs.

Traditional bridges have lost over $2 billion in exploits. SupraNova eliminates the bridge entirely by recomputing external chain consensus **natively on Supra L1**.

## How It Works
1. A transaction occurs on a supported chain (e.g., Ethereum).
2. A cryptographic proof of that event is generated and submitted to Supra.
3. SupraNova **recomputes and validates the source chain's native consensus** directly on Supra L1.
4. No external validators, relays, or multisigs involved — the source chain's own security is preserved.

For Ethereum specifically: Supra verifies the Ethereum Beacon State (signed by Ethereum's Sync-Committee) and event proof in a single step.

## Two Bridging Models Under One Architecture

### HyperNova
- Trustless bridge protocol using the **source chain's consensus layer** for event verification via validator signatures.
- Maximum security — preserves the full security guarantees of the source chain.
- Best for: L1-to-L1 transfers where trustlessness is paramount.

### Hyperloop
- Fast, multi-signature based **game-theoretically secure** bridge.
- Designed for source chains where HyperNova-style trustless bridging is not feasible or would result in high latency (e.g., Layer 2 chains).
- Best for: L2-to-Supra transfers where speed matters.

## Current Status
- **Mainnet**: Live for bridging native ETH, wETH, and USDC from Ethereum to Supra.
- Bridged ETH and wETH are minted on Supra as **supETH** (wrapped token using the FA token standard).
- Verification runs in sub-minute time with gas fees paid in $SUPRA.
- **Roadmap**: Expanding to other EVM PoS chains, MoveVM networks (Aptos, Sui), and additional L1s/L2s.

## Use Cases
- **Cross-chain DeFi**: Transfer assets and interact with DeFi across chains
- **Cross-chain gaming**: Move in-game assets or NFTs between blockchain-based games
- **Cross-chain data**: Secure, verified data feeds from other chains
- **Liquidity unification**: Tap into liquidity from Ethereum and other L1s without fragmentation

## Integration
- **SupraNova Docs**: https://docs.supra.com/supranova
- **SupraNova dApp**: Live on mainnet for bridging
- Developers can build cross-chain messaging into their smart contracts using SupraNova's framework
