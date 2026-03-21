# Supra AutoFi

> Deep dive on Supra's AutoFi system-level automation — zero-delay execution, revenue model, AI agents, and the paradigm shift enabled by 500k TPS.

**Domain:** engineering · **Version:** 1.0.0 · **By:** Yong (Supra Devrel)

**Keywords:** `autofi` `automation` `automatic finance` `keepers` `liquidation` `arbitrage` `rebalancing` `agents` `ai agents` `supra` `defi`

[View on Kavela Marketplace](https://kavela.ai/marketplace/supra-agent)

---
# Supra AutoFi — Automatic Finance via System-Level Automation
## What It Is
AutoFi is Supra's system-level automation engine, embedded natively into the L1. It enables smart contracts to execute autonomously the moment specific on-chain conditions are met — with zero-block delay and no third-party keepers required.
This is the first system-level automation in blockchain. Other chains rely on external keeper networks (e.g., Chainlink Keepers, Gelato) that introduce latency, extra trust assumptions, and MEV extraction. AutoFi eliminates all of that.
## How It Works
- Smart contracts register automation tasks with conditions (price thresholds, time triggers, on-chain state changes).
- The Supra protocol evaluates these conditions every block.
- When conditions are met, the automation executes immediately — same block, zero delay.
- No external bots, no off-chain infrastructure, no keeper networks.
## Revenue Model
AutoFi generates sustainable revenue redistributed across the ecosystem:
- 25% to DeFi dApps participating in automation
- 25% to node operators
- 50% to the Supra treasury
- Revenue comes from automation task fees and priority execution auctions
This creates a self-sustaining, anti-inflationary economic loop. Supra aims to phase out block rewards within two years as AutoFi revenue scales.
## Use Cases
- Automatic liquidations: DeFi lending protocols can liquidate undercollateralized positions instantly, block-by-block
- On-chain arbitrage: Capture price discrepancies automatically without external bots
- Automated rebalancing: Portfolio and LP rebalancing triggered by market conditions
- Scheduled DeFi operations: Recurring payments, vesting schedules, yield harvesting
- AI Agent architectures: Agents setting parameters and running on-chain scripts — at 500k TPS, agents can operate in real-time
- Entry scanning: AutoFi scanning for market entry signals and executing strategies autonomously
## Why 500k TPS Changes Everything
With Supra's extreme throughput, AutoFi isn't just "automation" — it's a new paradigm. You can imagine:
- Agents that monitor hundreds of markets simultaneously and execute block-by-block
- On-chain scripts that scan for entries, set parameters, and trade — all without human intervention
- Complex multi-step strategies that would be impossible on slower chains
## Integration
AutoFi is accessed through Supra's Move framework. Register automation conditions in your smart contract, and the protocol handles execution.
- AutoFi Assist: No-code automation creation dApp on Supra mainnet
- AutoFi Whitepaper: Available at supra.com/autofi
- Developer Docs: https://docs.supra.com
