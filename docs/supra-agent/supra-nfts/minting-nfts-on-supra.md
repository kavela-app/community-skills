# Minting NFTs on Supra

> Guide to minting NFTs on Supra — source file locations, quick mint via wrapper vs full custom minting, and advanced patterns like self-signing NFTs and Agents-as-NFTs.

**Domain:** engineering · **Version:** 1.0.0 · **By:** Yong (Supra Devrel)

**Keywords:** `nft` `minting` `mint` `0x4` `0x3` `aptos token` `move` `source` `self-signing` `agents` `supra` `custom nft`

[View on Kavela Marketplace](https://kavela.ai/marketplace/supra-agent)

---
# Minting NFTs on Supra — Source Files & Approaches
## The Move Framework for NFTs
All NFT functionality on Supra lives in the Aptos Move framework maintained by Supra:
- Full framework: https://github.com/Entropy-Foundation/aptos-core/tree/dev/aptos-move/framework
You'll find .move files with functions and their usage. Focus on public fun and public entry fun — those are the functions you wire into your dApp.
## 0x4 Object Standard — Two Paths to Minting
### Path 1: Quick/Generic Minting via Aptos Token Wrapper
- File: https://github.com/Entropy-Foundation/aptos-core/blob/dev/aptos-move/framework/aptos-token-objects/sources/aptos_token.move
- This is a wrapper built by Aptos on top of the 0x4 standard.
- Use when: You want a generic NFT minted quickly without custom logic.
- Limitation: Highly restrictive. You get a standard NFT with basic metadata — no custom behavior, no special minting logic.
### Path 2: Full Custom Minting via Raw 0x4 Modules
- Directory: https://github.com/Entropy-Foundation/aptos-core/tree/dev/aptos-move/framework/aptos-token-objects/sources
- This directory contains all the individual .move module files for the 0x4 standard.
- Use when: You want full customizability — custom minting logic, custom traits, programmable behavior, self-signing NFTs, AutoFi integration.
- What's here: The raw building blocks — collection management, token creation, property maps, royalty handling, transfer logic, and the object primitives.
## 0x3 Legacy Standard
- The 0x3 standard modules are also in the framework repo under aptos-token.
- Each NFT collection supports multi-supply with property version differentiation.
- Minting creates a new property version within the collection, sequentially ordered.
## Advanced Patterns (0x4 Only)
### Self-Signing NFTs
Because 0x4 NFTs are Objects, they can sign transactions for themselves. This means:
- An NFT can hold other assets
- An NFT can call other smart contracts autonomously
- An NFT can interact with DeFi protocols on its own behalf
### Agents-as-NFTs
Combine self-signing NFTs with AutoFi to create:
- NFTs that monitor market conditions and execute trades
- NFTs that manage portfolios autonomously
- NFTs as on-chain AI agents with their own wallet and execution capability
This architecture is unique to Supra — no other chain has the combination of self-signing objects + native system-level automation.
## Key Source Files to Study
| File/Directory | Purpose ||---|---|| aptos_token.move | Generic wrapper for quick 0x4 mints (restrictive) || aptos-token-objects/sources/ | Full 0x4 module library (maximum flexibility) || Framework aptos-token/ | 0x3 legacy standard modules |
