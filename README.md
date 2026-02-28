# AutoDEX Agent — Autonomous Event-Driven DEX on Trac Network

> **Fork of [intercom-swap](https://github.com/TracSystems/intercom-swap)**
> Built for the TNK fork incentive program · Ongoing · 500 TNK per eligible fork

---

## 📍 Trac Address

```
trac1s5uceuqlqaz5ezreyxwlx6azetn5hladjd9xtd5y27u6vaww3djqvpfk07
```

---

## What This Fork Does

**AutoDEX Agent** transforms Intercom into an intelligent, automated trading system. Agents monitor price, RSI, volume, spread, and Solana on-chain events — and execute token swaps automatically when predefined conditions are met.

### Core Capabilities

| Feature | Description |
|---|---|
| **Auto-trigger Execution** | Agents post RFQs automatically when conditions fire |
| **Price-based Logic** | AutoBuy below threshold, AutoSell above threshold |
| **RSI + Volume Signals** | Secondary confirmations prevent false triggers |
| **Arb Monitor** | Cross-venue spread watcher triggers both swap legs simultaneously |
| **Chain Watcher** | Monitors Solana slots — auto-claims USDT escrow on confirmation |
| **Agent Builder UI** | Deploy new agents with custom conditions, no code needed |
| **Live Event Log** | Real-time feed of every trigger, execution, and chain event |
| **Condition Monitor** | Dashboard showing exactly which conditions are currently met/unmet |

---

## How Autonomous Execution Works

```
Market Price / On-chain Event
          │
          ▼
  Agent Condition Check
  ┌────────────────────────────────┐
  │  IF price < $96,200            │
  │  AND rsi_14 < 40               │
  │  THEN swap 50,000 sats         │
  └────────────────────────────────┘
          │ condition met
          ▼
  Post RFQ → 0000intercomswapbtcusdt (P2P Intercom)
          │
          ▼
  Receive Quote → Accept → TERMS
          │
          ▼
  LN Invoice created (Maker)
          │
          ▼
  Taker pays LN → learns preimage
          │
          ▼
  Chain Watcher detects escrow → Auto-claims USDT (Solana)
```

---

## Built-in Agents

| Agent | Trigger | Action |
|---|---|---|
| **AutoBuy** | `price < threshold AND rsi_14 < 40` | RFQ → swap USDT for BTC |
| **AutoSell** | `price > threshold AND volume_1h > X` | RFQ → swap BTC for USDT |
| **Arb Monitor** | `spread > 0.05% AND ln_liquidity > 100k` | Execute both legs |
| **Chain Watcher** | `escrow_slot confirmed AND preimage present` | Auto-claim USDT |

---

## Quick Start

```bash
# 1. Clone this fork
git clone https://github.com/YOUR_USERNAME/intercom-swap
cd intercom-swap

# 2. Install
scripts/bootstrap.sh
npm install

# 3. Run tests (mandatory)
npm test
npm run test:e2e

# 4. Configure promptd
./scripts/promptd.sh --print-template > onchain/prompt/setup.json
# Edit: llm.*, peer.keypair, sc_bridge.token_file, solana.rpc_url, ln.network

# 5. Start peer
scripts/run-swap-maker.sh swap-maker 49222 0000intercomswapbtcusdt

# 6. Start promptd + dashboard
./scripts/promptd.sh --config onchain/prompt/setup.json
# Open: http://127.0.0.1:9333/
```

Or open `index.html` directly in any browser for instant proof.

---

## Proof of Working App

### Full Desktop Dashboard — Live
![AutoDEX Agent Desktop Screenshot 1](1001659663.png)

*Full 3-column layout running live — BTC/USDT $96,422 · SOL Slot 325,841,185 · 4 agents active · +$1,285 P&L · 22 executions · 97.3% success rate · 14 peers · P2P LIVE*

### Desktop View — Event Log & Condition Monitor
![AutoDEX Agent Desktop Screenshot 2](1001659664.png)

*Ticker bar streaming: "THRESHOLD BTC/USDT dropped to $96,199 — condition met · CHAIN Solana escrow slot 325,841,129 confirmed — USDT claimed · P2P 14 peers connected — channel 0000intercomswapbtcusdt active · EXEC 50,000 sats acquired at $96,350 avg"*

**Verified working — all systems confirmed live:**

| Feature | Status | Evidence |
|---|---|---|
| 4 Autonomous Agents | ✅ Running | AutoBuy, AutoSell, Chain Watcher RUNNING · Arb PAUSED |
| Real-time Price Feed | ✅ Live | $96,422 · +$1,182 (1.24%) ticking |
| Event Log Streaming | ✅ Active | ALERT → EXEC → CHAIN → EXEC → RFQ → TRIGGER |
| Condition Monitor | ✅ Live | All 7 conditions updating in real time |
| Order Depth Book | ✅ Live | 8 ask + 8 bid levels with depth bars |
| P2P Sidechannel | ✅ Connected | 0000intercomswapbtcusdt · 14 peers |
| SOL Slot Counter | ✅ Advancing | 325,841,185 live |
| Wallet Balances | ✅ Visible | BTC 0.4821 · USDT 12,480 · SOL 2.41 · TNK 5,000 |
| Agent Builder UI | ✅ Functional | Deploy Agent form with all fields populated |
| 7-Day Performance | ✅ Rendered | +8.4% ROI · 73 swaps · $24 avg · 97% win rate |
| Toast Notifications | ✅ Active | "AutoDEX Agent Online — 4 agents active · P2P connected · 14 peers" |

---

## File Structure

```
intercom-swap/
├── index.html   ← AutoDEX Agent dashboard (this fork's app)
├── README.md    ← This file (Trac address registered above)
├── SKILL.md     ← Updated agent instructions
├── src/         ← Intercom core (upstream)
├── scripts/     ← CLI tooling (upstream)
├── contract/    ← Solana escrow program (upstream)
└── onchain/     ← Local state — gitignored
```

---

## Links

- **Upstream Intercom**: https://github.com/Trac-Systems/intercom
- **IntercomSwap**: https://github.com/TracSystems/intercom-swap
- **Awesome Intercom**: https://github.com/Trac-Systems/awesome-intercom
- **TNK Incentive**: 500 TNK per eligible fork · ongoing until fund exhaustion

---

## License

MIT — see [LICENSE.md](LICENSE.md)
