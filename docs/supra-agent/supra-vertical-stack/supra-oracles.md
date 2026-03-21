# Supra Oracles

> Deep dive on Supra's native oracle infrastructure — DORA protocol, 600+ feeds, sub-second freshness, AI oracles, and integration guidance.

**Domain:** engineering · **Version:** 1.0.0 · **By:** Yong (Supra Devrel)

**Keywords:** `oracles` `price feeds` `dora` `data` `rwa` `defi` `supra oracles` `threshold ai` `real-time`

[View on Kavela Marketplace](https://kavela.ai/marketplace/supra-agent)

---
# Supra Oracles — Native Real-Time Data Feeds

## What It Is
Supra Oracles are native, real-time oracle price feeds built directly into Supra's L1 as a first-class protocol primitive. They deliver **sub-second data freshness** — not an external service bolted on, but embedded at the protocol level.

## Capabilities
- **600+ price feeds** across crypto, Real World Assets (RWA), equities, and more
- **Sub-second data freshness** — feeds update in real time, not on heartbeat intervals
- **Serving 90+ external chains** — Supra Oracles are already battle-tested across the broader blockchain ecosystem
- Built on the **Distributed Oracle Agreement (DORA) protocol**, which tolerates up to **49% Byzantine faults** — one of the most resilient oracle designs in production

## Threshold AI Oracles
Supra also provides **Threshold AI Oracles** — putting AI insights on-chain in a trustless, decentralized, and secure manner. These use BLS threshold signatures and decentralized committees of specialized AI agents that deliberate in parallel for bias-resistant insights.

## Use Cases
- **DeFi**: Price feeds for DEXs, lending protocols, derivatives, liquidation triggers
- **Prediction markets**: Sports data, election results, event outcomes
- **Parametric insurance**: Weather data, flight delays, disaster triggers
- **RWA tokenization**: Real-time pricing for tokenized real-world assets
- **Any dApp** that needs off-chain data delivered on-chain reliably

## Why Native Matters
On other chains, you'd integrate Chainlink or another third-party oracle — adding latency, trust assumptions, and extra fees. On Supra, oracle data is a native primitive:
- Zero extra latency (data lives in the protocol)
- No additional trust assumptions beyond Supra's own validator set
- No external fees for oracle access
- Simplified developer experience — just call the oracle module

## Integration
Access oracle feeds through Supra's Move framework modules. Documentation and data dashboard available at:
- **Data Dashboard**: Available on supra.com (live feeds across crypto, RWAs, and more)
- **Developer Docs**: https://docs.supra.com
