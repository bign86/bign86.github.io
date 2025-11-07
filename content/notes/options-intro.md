---
title: Options
description: ""
date: 2020-12-08
preview: ""
draft: false
tags:
  - basic finance
  - options
categories:
  - finance
  - notes
math: true
slug: options-intro
---

## Basic Definitions

An option is a contract that gives the buyer the _right_, but **not** the _obligation_, to trade a specific instrument at a specific price called strike and at a specific time. The option buyer is said to be long the option, while the option seller is said to be short the option.

A **call** option gives the right to buy the instrument from the option seller, while the **put** option gives the right to sell the instrument to the option seller.

An **European** option can be exercised at one single point in time, the expiration. An **American** option can be exercised at any time after an initial period of lock. In these notes to keep things simple, is it always assumed an European option unless otherwise stated.

Since the option gives the buyer a right but not an obligation, the buyer can freely decide if (and when in case of an american option) to exercise, while the seller has the obligation to honor his side of the trade should the option buyer decide to exercise. Therefore, the option has a value to the buyer, which is the **price** of the option paid to the seller.

When exercising the option has a positive value to the holder is said to be **in-the-money**, when it is wortless is said to be **out-of-the-money**. At the breakeven point, the option is **at-the-money**.

### Payoff

The payoff \(V\) for the buyer (long position) of a (_costless_) option is given by

$$V^{long}=\begin{cases}\max\left[S_T-K, 0\right] & call\\\max\left[K-S_T, 0\right] & put\end{cases}$$

where \(K\) is the strike of the option decided at the initiation of the contract (\(t=0\)), and \(S_T\) is the price of the underlying at the exercise time \(t=T\). The payoff structure derives from the fact that the option buyer will not exercise the option if the payoff is negative. Therefore, the option buyer can never loose from an option trade, and the seller can never gain since the payoff for the seller (short position) is the opposite of the buyer's one

$$V^{short}=\begin{cases}\min\left[K-S_T, 0\right] & call\\\min\left[S_T-K, 0\right] & put\end{cases}$$

where by construction \(V^{short} \leq 0\).

To compensate the imbalance, the price of the option, \(C_0\) for a call \(P_0\) for a put, is set to make the deal "fair" to both the buyer and the seller. The payoff structure for the buyer considering the price is

$$V^{long}=\begin{cases}\max\left[S_T-K-C_0, -C_0\right] & call\\\max\left[K-S_T-P_0, -P_0\right] & put\end{cases}$$

which is positive only for \(S_T > K+C_0\) for a call and \(S_T < K-P_0\) for a put.

The choice of the option price depends on the valuation at the present day of the expected value of the option's future claim.

### Option price

The price of an option, the present value of the future claim, can be decomposed in two parts:

- The **intrinsic value** can be either positive or negative and represents by how much the option is in- or out-of-the-money.
- The **time value** is always positive and is an increasing function in the residual life of the option and the volatility of the underlying. Both are valuable to the option holder as previously discussed.

The price of the option is therefore **convex** in the payoff, with the time value of the option decreasing over time until at expiration there is no optionality left, and only the intrinsic value is left.
The present value of a European option can be bound between a minimum and a maximum:

- For a call option it cannot be greater than the value of the underlying, and it cannot be smaller than the value of the underlying minus the present value of the strike
  $$S_t - K\:e^{-\tau r}\leq C_t\leq S_t$$
- For a put option the upper bound is the strike, attained in full if the underlying price falls to zero, the lower bound can be calculated from the put-call parity using the corresponding call option
  $$K\:e^{-\tau r} - S_t\leq P_t\leq K$$

#### Determinants of option value

The option price depends on the following quantities:

- Price of the underlying (\(S\)): the payoff of the option   depends directly on the price of the underlying at exercise time.
- Volatility of the underlying (\(\sigma\)): a higher volatility increases the probability of the option to be in-the-money at expiration.
- Dividends paid by the underlying (\(D\)): large dividend yields have a negative impact on the price of the underlying, which can be positive or negative for the option.
- Strike of the option (\(K\)): the choice of strike impacts directly the payoff.
- Residual time to expiration (\(\tau\)): more time to expiration increases the probability the option could end up in-the-money.
- Interest rates (\(r\)): a change in the interest rate changes impact the real value of the strike of the option.
- American/European: American options are worth an additional premium.

The effect of these variables is summarized in the following table. The table assume an increase in each of the variables, the effects for a decrease are reversed.

| Change               | Call | Put |
|:--------------------:|:----:|:---:|
| \(S\) increase       |  +   |  -  |
| \(K\) increase       |  -   |  +  |
| \(\sigma\) increase  |  +   |  +  |
| \(\tau\) increase    |  +   |  +  |
| \(r\) increase       |  +   |  -  |
| \(D\) increase       |  -   |  +  |

### Put-call parity

Under no-arbitrage conditions, given \(C_T\) the payoff at \(T\) of the costless call and \(P_T\) that of the costless put we have that

$$C_T - P_T = S_T - K$$

Discounting to the present and rearranging, we obtain the most common form of the **put-call parity formula**

$$C_t + K\:e^{-\tau r_f} = P_t + S_t$$

with \(C_t\) call value, \(P_t\) put value, \(S_t\) value of the underlying (all at \(t\)), \(\tau=T-t\) time to expiration, and \(r_f\) risk-free rate. Here it has been used that \(P\:e^{-\tau r_f} = \pi_t\) in a risk-neutral setting, as discussed in [Pricing Basics](/notes/pricing-basics).

There are two simple pictures to get convinced the put-call parity works.

#### One-step model

Assume a one-step binomial model with only two possible outcome scenarios \((\omega_1, \omega_2)\). We have that
$$\begin{align*}C_T(\omega)&=\begin{cases}S_T-K & \text{if}\:\omega=\omega_1\\ 0 & \text{if}\:\omega=\omega_2\\\end{cases}\\P_T(\omega)&=\begin{cases} 0 & \text{if}\:\omega=\omega_1\\ K-S_T & \text{if}\:\omega=\omega_2\\\end{cases}\end{align*}$$

It is easily shown that, regardless of the realized scenario, the difference \(C_T - P_T\) satisfies the put-call parity.

#### Replicating portfolios

The relationship can be found through the **replicating portfolio** technique, where a combination of risk-free borrowing/lending and the underlying of the option is used to build a portfolio that replicates the cash flow of the option. The cost of this portfolio must be the value of the option, due to no-arbitrage conditions. Assume two portfolios at \(t\):

- A long call and a short put on the same strike. This combinations replicates a forward on the underlying and the payoff at \(T\) will be \(S_T - K\).
- Borrow \(K\:e^{-\tau r_f}\) at the risk-free and invest in the underlying at \(S_t\). The payoff at \(T\) will be \(S_T - K\) as the underlying is sold and the borrowed money is repaid.

As both portfolio have the same payoff must have the same cost, hence the put-call parity relation.

### American option early exercise

Early exercise of an American option is always sub-optimal. By exercising the option, the holder realises the payoff which is the intrinsic value. By selling the option without exercising, the holder realises the option value which is the intrisic value plus the time value which is always positive. The only situation where early exercise should be considered are:

- For a call option, if the underlying pays high dividends. The option does not give rights to receive the dividends, while a high yield depletes the value of the underlying.
- For a put option, if the option is deep in-the-money and interest rates are high, it may be worth exercising the option.
