[README.md](https://github.com/user-attachments/files/34939341/README.md)

# Vasicek interest rate simulator

An interactive page that simulates short-term interest rate paths using the Vasicek model, so you can see how the model's parameters change the range of possible outcomes.

**Live demo:**  https://euandcosta07.github.io/Vasicek-Interest-Rate-Model-Simulation/

## Why I built it

I first modelled the Vasicek framework in Excel, calibrating it to Bank of England data to simulate interest rate pathways. This page is a companion to that project. It lets anyone adjust the parameters and see the effect immediately, instead of reading a table of numbers.

## The model

The Vasicek model describes the short rate `r` as:

```
dr = a(b − r)dt + σ dW
```

- `a` is the speed of mean reversion: how quickly the rate is pulled back to its long-run level.
- `b` is the long-run level the rate drifts towards.
- `σ` is the volatility of the random shocks.
- `dW` is a random shock from Brownian motion.

## What the page shows

- Simulated rate paths over a chosen horizon, with the number of paths adjustable.
- The average (expected) path and a 95% range, calculated from the model's closed-form results.
- The long-run level as a dashed line.
- Four live figures: the half-life of the gap to the long-run level, the expected rate at the horizon, the 95% range at the horizon, and the probability the rate is below zero at the horizon.

## The maths

Paths are simulated weekly with the model's exact update rather than a rough approximation:

```
r(t+Δt) = r(t)·e^(−aΔt) + b·(1 − e^(−aΔt)) + σ·√((1 − e^(−2aΔt)) / (2a)) · Z
```

where `Z` is a standard normal random number.

The average path and the spread come from the closed-form results:

```
E[r(t)]   = b + (r₀ − b)·e^(−at)
Var[r(t)] = σ² / (2a) · (1 − e^(−2at))
```

The half-life is `ln(2) / a`, and the long-run standard deviation is `σ / √(2a)`.

## Limitations

- The model can produce negative rates. This was once seen as a flaw, and it matches some real market episodes. Models such as Cox–Ingersoll–Ross avoid it.
- It has a single factor and constant parameters, so it does not capture the full yield curve or changing market conditions.
- Parameters are set by the user. The page does not calibrate them to real data.
- This is an educational illustration, not a forecast and not financial advice.

## Run it locally

It is a single self-contained file with no build step. Download `index.html` and open it in a browser. It loads two Google Fonts and falls back to system fonts if they are unavailable.

## Built with

HTML, CSS and JavaScript, using the browser's canvas for the chart and no external libraries. I built it with help from Claude (Anthropic) and worked through the model and the maths myself.

## Author

Euan D'Costa, BSc Mathematics and Statistics, UCL.
