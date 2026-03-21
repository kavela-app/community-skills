# Indexing on Supra

> Guide to building and using indexers on Supra — the event emission pattern, idempotency requirements, Crystara's pre-built NFT indexer, and custom indexer architecture.

**Domain:** engineering · **Version:** 1.0.0 · **By:** Yong (Supra Devrel)

**Keywords:** `indexer` `indexing` `events` `custom event` `idempotency` `state` `database` `crystara` `nft indexer` `supra` `infrastructure`

[View on Kavela Marketplace](https://kavela.ai/marketplace/supra-agent)

---
# Indexing on Supra — Mission-Critical Infrastructure

## Why You Need an Indexer

On-chain storage has hard limitations on Supra (and all Move-based chains):
- You **cannot store large lists or dictionaries** the way you would in a traditional database.
- You **cannot iterate over a list with more than ~200 items** on average without hitting the execution limit.
- For any dApp with meaningful usage, you'll quickly exceed what's queryable directly on chain.

**The solution**: Index data off-chain by capturing on-chain events.

## The Indexing Pattern

```
On-Chain Action → Emit Custom Event → Off-Chain Indexer Captures Event → Store in Database → Frontend Queries Database
```

1. **On-chain**: When an action happens in your smart contract (mint, transfer, trade, etc.), your code emits a **custom event** with all relevant data.
2. **Off-chain indexer**: Your indexer watches the chain for these events and stores/organizes the data in a traditional database (PostgreSQL, MongoDB, etc.).
3. **Frontend**: Your app queries the indexer's database for display, search, filtering, pagination — everything that would be impractical on-chain.

## Building Your Own Indexer — Critical Requirements

### Ingestion Layer
- Reliably captures events from the Supra chain in real-time.
- Must handle chain reorgs gracefully (if applicable).
- Should support backfilling historical events.

### State Persistency
- Stores indexed data durably in a proper database.
- Track the last processed block/transaction to resume after restarts.

### Idempotency — THE #1 CONCERN
**This is the most mission-critical aspect of your indexer.**
- You must **only index each event once**. Never duplicate.
- A bad, non-stateful indexer that double-counts events or misses events will cause **community distrust**.
- Examples of what goes wrong without idempotency:
  - NFT counts are wrong (shows 1050 minted when only 1000 exist)
  - Token balances don't match on-chain state
  - Marketplace listings appear duplicated or missing
  - Users lose confidence and leave your platform

**Implementation**: Track event hashes or transaction IDs. Before processing any event, check if it's already been indexed. Use database constraints (unique indexes) as a safety net.

## Available Pre-Built Indexers

### Crystara's NFT Indexer
If you're building anything NFT-related, this saves enormous development time:
- **Fully equipped** with all NFT functionalities (minting, transfers, listings, sales, metadata)
- Production-grade, used by Crystara marketplace
- **Docs**: https://docs.crystara.trade/products/marketplace-api/api-overview

### Custom Indexing
If your dApp does something unique (custom DeFi, gaming state, social graphs), you'll likely need a custom indexer. Use Crystara's approach as a reference architecture for event schema design, ingestion pipeline structure, idempotency patterns, and API layer design.

## Summary
Indexing is not optional for production. It's the #1 infrastructure concern after smart contracts. Budget time and engineering resources for it early — don't treat it as an afterthought.
