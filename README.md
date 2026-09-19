# PumpSim API

Simulate pump.fun bonding-curve and PumpSwap buys, sells, and trader PNL, all off-chain with real curve math and zero SOL risk. Build and backtest trading bots or strategies without burning real SOL.

[Docs](https://docs.pumpsim.dev) · [Dashboard](https://pumpsim.dev/dashboard)

## Try it right now

No signup, no key needed.

```bash
curl -X POST https://api.pumpsim.dev/v1/demo/buy/quote -H "Content-Type: application/json" -d "{\"quote\": \"1\"}"
```

Real bonding-curve math, running live against a fresh coin.

## Use it in a bot

Get a free key at [pumpsim.dev/dashboard](https://pumpsim.dev/dashboard) (100 credits, no card required), then:

```js
const key = "YOUR_API_KEY";

const buy = await fetch("https://api.pumpsim.dev/v1/sim/buy/quote", {
  method: "POST",
  headers: { Authorization: `Bearer ${key}`, "Content-Type": "application/json" },
  body: JSON.stringify({ quote: "1" }), // 1 SOL
}).then(r => r.json());

console.log(`bought ${buy.trade.base_out} tokens, mc now ${buy.stats.market_cap_quote} SOL`);

// chain a sell on the same coin by passing its state back in
const sell = await fetch("https://api.pumpsim.dev/v1/sim/sell/base", {
  method: "POST",
  headers: { Authorization: `Bearer ${key}`, "Content-Type": "application/json" },
  body: JSON.stringify({ base_atoms: buy.trade.base_out, state: buy.state }),
}).then(r => r.json());

console.log(`sold back for ${sell.trade.quote_out} lamports`);
```

Every response shares the same envelope: `state` (feed it into your next call to chain trades), `stats`, `fees`, `trade`. Full reference: https://docs.pumpsim.dev/api-reference

## Why simulate instead of testing live

- No SOL spent, no wallet, no devnet faucet queue
- Real bonding-curve/AMM math, not an approximation
- Supports Custom Pairs (coins quoted against tokenized stocks, crypto, or other pump.fun coins) and Holder Rewards, not just SOL/USDC
- Pull a real coin's live reserves by mint and backtest against actual on-chain state
- Chain buys/sells to backtest a full strategy in milliseconds instead of minutes on-chain

## Links

- Docs: https://docs.pumpsim.dev
- Dashboard / API key: https://pumpsim.dev/dashboard
- Updates: [Telegram](https://t.me/PumpSimAPI)
- Questions: [Discord](https://discord.gg/GEaNhnWUGD)
