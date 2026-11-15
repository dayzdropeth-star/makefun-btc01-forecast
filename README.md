# makefun-btc01-forecast

[![make.fun · btc-cent-forecast-01](https://make.fun/badge/btc-cent-forecast-01.svg)](https://make.fun/bounty/btc-cent-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=btc-cent-forecast-01)
[![rank](https://make.fun/badge/btc-cent-forecast-01/dayzdropeth-star/makefun-btc01-forecast/rank.svg)](https://make.fun/bounty/btc-cent-forecast-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=btc-cent-forecast-01)
[![reward](https://make.fun/badge/btc-cent-forecast-01/dayzdropeth-star/makefun-btc01-forecast/reward.svg)](https://make.fun/bounty/btc-cent-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=btc-cent-forecast-01)

Bitcoin price forecaster for make.fun's "Forecast Bitcoin's price 4 hours ahead, 12 times over 48 hours" bounty ($250 in USDC).

## Method

The program forecasts the close 240 minutes after each checkpoint. Under absolute-error scoring the best single guess is the conditional median of the future price, and Bitcoin's 4-hour moves are close to a random walk — so the anchor is the checkpoint's own close (the last price in the case). On top of that, a small drift term: the slope of a least-squares fit of log price on time over the last 12 hours, extrapolated 240 minutes and shrunk by 0.25 toward zero, because a 4-hour-ahead trend signal is noisy. A longer level (median of the last 30–60 closes) lags in a trending window and scored worse on the practice cases, so it is not used.

Practice (p00–p04, answers taken from the next case's last close): mean absolute error **45,678 cents**, against the persistence baseline's **47,055 cents**.

The program never crashes a case: any anomaly falls back to the last finite close, and if none exists, to a valid in-range constant — an invalid case takes no prize, so a valid output is always written.

## Run

Node 22, standard library only, no network.

```sh
mkdir -p out && MAKEFUN_INPUT_DIR=input MAKEFUN_OUTPUT_DIR=out node main.ts
```

Reads every `<id>.in` in the input directory and writes `<id>.out` to the output directory: `{"forecasts":[{"case":"<id>","price":<number>}]}`.
