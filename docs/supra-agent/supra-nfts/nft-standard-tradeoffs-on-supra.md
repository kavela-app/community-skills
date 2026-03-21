# NFT Standard Tradeoffs on Supra

> Comparison of Supra's two NFT standards — 0x3 legacy (Aptos Token) vs 0x4 object standard — with tradeoffs, use cases, and a decision matrix for choosing the right one.

**Domain:** engineering · **Version:** 1.0.0 · **By:** Yong (Supra Devrel)

**Keywords:** `nft` `nft standard` `0x3` `0x4` `aptos token` `object standard` `soulbound` `sft` `property version` `tradeoffs` `supra`

[View on Kavela Marketplace](https://kavela.ai/marketplace/supra-agent)

---
# Choosing an NFT Standard on Supra — 0x3 vs 0x4 Tradeoffs
Supra inherits from the Aptos token framework and supports two NFT standards. Choosing the right one depends on your project's needs.
## 0x3 — Aptos Token (Legacy Standard)
Address: 0x3
### What It Does
- Supports NFTs, SFTs (Semi-Fungible Tokens), and FTs (Fungible-like tokens with supply > 1).
- A single token definition (e.g., "Character A") can have a max supply (e.g., 1000 copies).
- Each individual mint is differentiated by a property version — this is also the mint order.
- So 1000 people can mint "Character A" — same attributes, same URI — but each mint is unique via its property version number.
### Best For
- Collections where you want multiple copies of the same asset with sequential differentiation
- Projects that need the SFT/FT pattern (think: game items with limited editions)
- Simpler use cases where the property version model fits
### Limitations
- Less efficient than 0x4
- No soulbound support
- NFTs can't sign for themselves
- Less flexible for advanced programmable NFT patterns
---
## 0x4 — Object Standard (Modern Standard)
Address: 0x4
### What It Does
- Each NFT is a full on-chain Object — a first-class programmable entity.
- More efficient storage and execution than 0x3.
- Supports soulbound NFTs (non-transferable, identity-bound tokens).
- Objects can sign for themselves — this is a major differentiator.
### Best For
- Projects needing full customizability and programmable NFTs
- Soulbound tokens: Achievements, credentials, identity, memberships
- Agents-as-NFTs: NFTs that self-sign and connect to AutoFi — autonomous on-chain entities
- Self-executing NFTs: NFTs that can trigger actions, hold assets, or interact with DeFi
- Any project wanting maximum flexibility
### Tradeoffs
- More barebones — you build what you need from raw primitives
- Steeper learning curve if you want custom behavior
- The generic wrapper (aptos_token.move) is restrictive — mainly for basic NFTs without custom logic
---
## Decision Matrix
| Factor | 0x3 (Legacy) | 0x4 (Object) ||--------|--------------|---------------|| Efficiency | Lower | Higher || Multi-supply / SFT | Native support via property versions | Build yourself || Soulbound NFTs | Not supported | Supported || Self-signing | Not possible | Yes — objects sign for themselves || AutoFi / Agent integration | Not possible | Yes — NFTs as autonomous agents || Customizability | Moderate (structured) | Maximum (barebones, build anything) || Learning curve | Lower | Higher || Recommended for new projects | Only if you need the property version model | Yes — default choice |
## Recommendation
Use 0x4 for new projects unless you specifically need the 0x3 property-version/multi-supply model. The self-signing capability combined with AutoFi opens up entirely new design spaces that are unique to Supra.
