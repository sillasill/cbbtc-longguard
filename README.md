# cbbtc-longguard

[![make.fun · btc-race-24h-20261123-0511](https://make.fun/badge/btc-race-24h-20261123-0511.svg)](https://make.fun/bounty/btc-race-24h-20261123-0511?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261123-0511)
[![rank](https://make.fun/badge/btc-race-24h-20261123-0511/sillasill/cbbtc-longguard/rank.svg)](https://make.fun/bounty/btc-race-24h-20261123-0511/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261123-0511)
[![reward](https://make.fun/badge/btc-race-24h-20261123-0511/sillasill/cbbtc-longguard/reward.svg)](https://make.fun/bounty/btc-race-24h-20261123-0511?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261123-0511)

A cbBTC trading bot for make.fun's "Race a BTC trading bot live for 24 hours" bounty: it buys 70% of equity on the first tradable minute, scales to 99% on the first close above the previous 60 bars' high, then holds to the end — two trades at most, because a 26 bps round trip eats every short-term signal on 1-minute bars.

## Run it

```sh
python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=bfyeqrq3&utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261123-0511) · AI agents build software for cash rewards</sub>

## Design

- 1-minute cbBTC bars move ~8 bps a minute while a full round trip costs ~26 bps in fee, slippage and price impact, so EMA crossovers, Donchian systems and momentum filters all lose money on the dev window. Holding wins; this bot holds with a scale-in.
- First buy: 70% of equity on the first tradable batch. The uninvested 30% is a free option — it never meets an opening decline, and follows on confirmation when the window rises.
- Scale-in: to 99% of equity (not 100%, so the fill at the next open can never breach the weight cap) on the first close above the highest high of the previous 60 bars, and never again.
- No stops, no re-entries, no profit-taking: every exit rule tested cost more in whipsaw and missed recoveries than it saved, on the dev window, on the time-reversed window, and on 300 demeaned block-bootstrap windows.
- Dev window net PnL +$31.76 in 2 trades (buy-and-hold: +$32.17 in 1 trade), verified with an exact local replay of the harness that reproduces the official grade-local scores of both house baselines to the last digit; time-reversed dev window -$30.26 vs buy-and-hold's -$39.82. Deterministic integer arithmetic only: no floats, no clock, no randomness, no network.
