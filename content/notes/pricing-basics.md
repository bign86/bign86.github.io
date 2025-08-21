---
title: Pricing Basics
description: ""
date: 2020-12-08
preview: ""
draft: false
tags:
  - basic finance
categories:
  - finance
  - notes
math: true
slug: pricing-basics
---

## Few simplified initial concepts

Assume the existence of a market with \(n\) risky assets plus one risk-free asset with prices

$$S = \left(S^0,\ldots,S^n\right)\quad with\quad S^i \geq 0$$

We introduce the **interest rate** \(r\) such that the price evolution of an asset can be described by

$$S_t = S_0\prod_t (1+r_t)\quad\forall t\in[0,\ldots,T]$$

If \(S_0=1\), we call the corresponding asset a **bank account**. This asset is the **reference assets**, or **numeraire**. It will be the first asset \(S^0\) going forward. It is generally assumed the interest rate is known in advance, making the numeraire a **predictable** asset. Other assets \(S^i\) will be regular assets.

This naturally introduces the concept of **discounting** using the numeraire

$$\tilde{S}_t^0=\frac{S_t^0}{S_t^0}=1\quad\text{and}\quad\tilde{S}_t^i=\frac{S_t^i}{S_t^0}$$

This allows to work on discounted asset prices, meaning the price of the numearaire is going to be constant and there are no interest rates.

Define a **portfolio** as a vector \(\varphi=\left(\varphi^0,\ldots,\varphi^n\right)\) evolving in time, representing the amounts held for each asset.

A **trading strategy** is a process \(\Gamma(\varphi)\) that describes the dynamical evolution of the portfolio \(\varphi\)

$$\Gamma_t(\varphi) := \varphi_t \cdot S_t = \sum_i^n \varphi_t^i S_t^i$$

Usually, it is required that at \(t_0=0\), \(\varphi_0 = \left(\varphi_0^0,0,\ldots,0\right)\), so that only the bank account is founded. This is also the **initial cost of the strategy** \(\mathcal{C}_0\)

$$\mathcal{C}_0(\varphi) = \varphi_0^0$$

The **cumulative cost** of the strategy is calculated from the **incremental cost** over time.
$$\mathcal{C}_t(\varphi) = \sum_t^T \sum_i^n \left(\varphi_t^i - \varphi_{t-1}^i\right) S_{t-1}^i = \sum_t^T \Delta\mathcal{C}_t(\varphi)$$

The strategy is called **self-financing** if the cost process \(\mathcal{C}_t(\varphi)\) is _constant_ over time and hence equal to the initial cost.
Therefore, a strategy is self-financing if and only if

$$\Delta\mathcal{C}_t(\varphi) = 0\quad\forall\:t$$


## Risk neutral probabilities

We assume a one-step model with \(t_0=0\) and \(t_1=1\) and define the future price at \(t_1\) for the asset \(i\) as \(S^i(\omega)\) in the scenario \(\omega\in\Omega\)
$$S_1 := \left(S_1^0,\ldots,S_1^n\right)\:\:\in\:\: p\left(\Omega,\mathcal{F},\mathbb{R}\right)$$

Assume the risk-free rate is \(r\) so the future price of the risk-free asset is given by
$$S_1^0(\omega) = S_0^0(1 + r)\quad\forall\omega$$

The probability \(P^\star\) is called **risk-neutral probability** or **martingale measure** if the expected value of the discounted future price under \(P^\star\) equals the current price:
$$S_0^i = E_{P^*}\left[\frac{S_1^i}{1+r}\right]$$

The risk-neutral measure exists if and only if two conditions are verified:

- The market is _complete_. If this is true it can be argued the _Law of one price_ holds, meaning there is a single efficient price for assets that can be found using the a single measure, the risk-neutral one.
- The market is _arbitrage-free_.

### Why risk-neutral probabilities?

Investors price assets depending on the probability of the future payoffs, i.e. on the risk of the asset. Generally, investors require a premium to bear risk, meaning prices differ from the expected discounted payoff by the amount of perceived risk of the investor. Therefore, to price an instrument one could:

- Take the discounted future payoffs and adjust for the investor risk appetite. The adjustment is in general different for each instrument and investor.
- Take the discounted future payoffs that already incorporate all investors' risk premia, and then take the expectation under this new probability distribution, the risk-neutral measure. The advantage is that the risk-neutral probabilities can be used for every instrument.

Risk-neutral probabilitites are a calculation tools useful to compare the price of assets bearing a different amount of risk.

## Fundamental theorem of asset pricing

The theorem states that a market is arbitrage-free if and only if there exists a risk-neutral measure \(P^\star\) equivalent to the original measure \(P\), i.e.
$$\text{if}\:\:\exists P^\star\in\left(\Omega,\mathcal{F}\right)\quad\text{such that}\quad P^\star\approx P$$

Two probability measures are equivalent if the measures agree which events have measure 0. In other words, given a set \(A\)
$$P^\star\approx P\quad\forall A\quad\text{if and only if}\quad P[A]=0\text{ and }P^\star[A]=0$$

