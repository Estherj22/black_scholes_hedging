# Black-Scholes, Monte Carlo and Delta Hedging

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Estherj22/black-scholes-delta-hedging/blob/main/black_scholes_hedging.ipynb)

A Python project that prices options with the Black-Scholes formula, checks the price with a Monte Carlo simulation, and then simulates selling an option and delta hedging it. I built it to understand when an options seller makes or loses money.

## The question

If you sell an option and hedge it with shares, when do you make money? How does the answer depend on how often you rebalance, and on whether the volatility you priced with matches what the stock actually does?

## How it works

- **Formula:** Black-Scholes prices for a European call and put, plus delta, gamma and vega.
- **Checks:** put-call parity, and a Monte Carlo price averaged over 200,000 simulated paths.
- **Hedging simulation:** sell one call priced at a chosen volatility, hold delta shares, rebalance at fixed intervals, and record profit and loss at expiry across 5,000 simulated paths.
- **Settings used:** stock price 100, strike 100, three months to expiry, interest rate 3%, volatility 20%.

All data is simulated.

## Results

**Does the code agree with the maths?**

- ## Results

**Does the code agree with the maths?**

- Formula call price: 4.3576. Monte Carlo call price: 4.3627, with a standard error of 0.0144, so the two differ by about a third of a standard error.
- Put-call parity: both sides came to 0.7472.
- Greeks at the start: delta 0.5497, gamma 0.0396, vega 0.1979 (per percentage point of volatility).

**Experiment A: rebalancing more often reduces risk (true volatility equals priced volatility)**

| Rebalances | Mean profit | Standard deviation |
|---|---|---|
| 4 | -0.010 | 1.626 |
| 16 | -0.002 | 0.863 |
| 63 | -0.010 | 0.436 |
| 252 | -0.003 | 0.219 |

![Hedging profit and loss with few and many rebalances](hedging_pnl.png)

**Experiment B: what if the true volatility differs from the one I priced with? (63 rebalances)**

| True volatility | Mean profit | Standard deviation |
|---|---|---|
| 15% | 0.993 | 0.458 |
| 20% | -0.000 | 0.432 |
| 25% | -0.997 | 0.693 |
| 30% | -1.996 | 1.129 |

**Finding 1:** Rebalancing more often cut risk without changing the average outcome. Mean profit stayed close to zero at every frequency, but the standard deviation fell from 1.626 with 4 rebalances to 0.219 with 252. Each fourfold increase in rebalancing roughly halved the risk, consistent with risk falling with the square root of the number of rebalances. With 4 rebalances the standard deviation was about 37% of the 4.36 option price, and with 252 it was about 5%.

**Finding 2:** The seller made money when the stock was calmer than the volatility they priced with, and lost money when it was wilder. Priced at 20%, mean profit was +0.993 at 15% real volatility, about zero at 20%, -0.997 at 25% and -1.996 at 30%. Every 5 percentage points moved profit by roughly 1.0, which matches the option's vega of 0.198 per point (0.198 x 5 is about 0.99). A hedged option seller is effectively short volatility. Risk also grew with real volatility: the standard deviation was 1.129 at 30%, against 0.432 at 20%.

**Experiment A: rebalancing more often reduces risk (true volatility equals priced volatility)**


![Hedging profit and loss with few and many rebalances](hedging_pnl.png)

**Experiment B: what if the true volatility differs from the one I priced with?**


## Limitations

- Volatility is constant, whereas real volatility changes over time.
- There are no trading costs, so more rebalancing is always better here but is not in real markets.
- Price paths have no jumps, so sudden moves are underestimated.
- Rebalancing happens at fixed intervals and is not continuous.

## Run it yourself

1. Click the Open In Colab badge at the top, or download black_scholes_hedging.ipynb and open it in Jupyter.
2. Run all cells. The settings at the top let you change the strike, expiry, interest rate and volatility.

## Possible next steps

- Solve for implied volatility from a market price using bisection.
- Add a trading cost per share and find the best rebalancing frequency.
- Let volatility change over time and test how the hedge performs.
