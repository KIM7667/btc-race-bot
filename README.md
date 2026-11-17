[![bounty](https://make.fun/badge/btc-race-24h-20261117-0603.svg)](https://make.fun/bounty/btc-race-24h-20261117-0603)
[![rank](https://make.fun/badge/btc-race-24h-20261117-0603/KIM7667/btc-race-bot/rank.svg)](https://make.fun/bounty/btc-race-24h-20261117-0603)
[![reward](https://make.fun/badge/btc-race-24h-20261117-0603/KIM7667/btc-race-bot/reward.svg)](https://make.fun/bounty/btc-race-24h-20261117-0603)

# btc-race-bot

A BTC trading bot for the make.fun competition
[btc-race-24h-20261117-0603](https://make.fun/bounty/btc-race-24h-20261117-0603):
race a BTC trading bot live for 24 hours, paper money, simulation only.

## Strategy

Range trading on 1-minute cbBTC closes, long only. The bot only trades
the edges of a range it has actually seen, never the noise in the middle:

- Range: highest and lowest closes of the last 360 bars (6 hours).
- Buy: close within 40 bps above the rolling low, at least 25 bps above
  it (the low held and price is bouncing), and the low was set at least
  60 bars ago (an established level, not a fresh dip still falling).
  Full equity, one position at a time.
- Sell: close within 40 bps below the rolling high, that high set at
  least 60 bars ago, or a 1.50% trailing stop off the peak position
  value, or a 1.50% hard stop under cost.

Pure Python standard library, deterministic (integer math on closes only).

## Dev window result (local replay, exact harness economics)

- Net PnL: **+$11.49** on $1,000 paper (gate needs > $0)
- Trades: 6
- Max drawdown: 1.41% (guardrail: <= 25%)
- Determinism: two runs produce byte-identical action logs

## Grade locally

```sh
node <kit>/cli/makefun.mjs grade-local --kit <kit> --bot . --driver docker
```

`main.py` is the entrypoint; `vendor/makefun_bars.py` is the kit SDK
shim copied byte for byte.
