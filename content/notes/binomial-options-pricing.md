---
title: The binomial options pricing model
description: ""
date: 2025-08-16
preview: ""
draft: false
tags:
  - basic finance
  - options
categories:
  - finance
  - notes
math: true
slug: binomial-options-pricing-model
---

The binomial model is a simple numerical method for the calculation of the price of European and American options, formalized for the first time by Cox, Ross and Rubinstein in 1979[^1] and sometimes called the CRR model. It as been then expanded by other authors to address some shortcomings of the original formulation.

The binomial model discretizes time into equal length steps and creates a tree of possible scenarios. At each time step, the each available scenario generates two new scenarios in the following time step, from which the term binomial. When the full binomial tree is calculated, the fair value of the option is found by back-calculation, starting from the latest time step and moving backward to the current time.

Here we give a simple introduction to the model. See also [^2].

## One-step formulation

In the one-step binomial model there are only two time points, \(t_0=0\) and \(t_1=1\). The present price (at \(t_0=0\)) of the option underlying is \(S_0\) and is known which certainty. At time \(t_1\), it is assumed the price has moved in one of two ways: the price has moved up by a quantity \(u\), which happens in the scenario \(\omega_1\) with probability \(p\), or down by a quantity \(d\), in the scenario \(\omega_2\) with probability \(p-1\).

Thus, the price \(S_1\) is a random variable that may assume values

$$S_1 = \begin{cases}u\:S_0\quad\text{in }\omega_1\text{ with prob. }p\\d\:S_0\quad\text{in }\omega_2\text{ with prob. }1-p\end{cases}$$

At \(t_1\), the value \(C\) of a call option or \(P\) of a put option is known exactly. However, to find the price of the option at \(t_0\) the claim must be discounted in some way.

### Risk-neutral probabilities

To find the present value of the future claim, the option values at \(t_1\) must be discounted to \(t_0\), and it is desiderable to do so in a way that is not affected by the risk preferences of any investor. The risk-neutral probabilities are the best tool to achieve a risk-neutral pricing based on the principle of no-arbitrage.

First, consider a portfolio \(\xi_0\) at \(t_0\) composed of \(S_0\) of stock and 1 in cash. Note that here we assuming cash is the numeraire. Given \(r\) risk-free rate, the value of the portfolio at \(t_0\) and \(t_1\) is

$$\xi_0 = S_0 + 1\qquad\qquad\xi_1 = \begin{cases}u\:S_0 + (1+r)\quad\text{in }\omega_1\\d\:S_0 + (1+r)\quad\text{in }\omega_2\end{cases}$$

To find the risk-neutral probabilities \(\pi_1\) under \(\omega_1\) and \(\pi_2\) under \(\omega_2\), we solve the following system

$$\frac{1}{1+r}\begin{pmatrix}1+r & 1+r\\ u\:S_0 & d\:S_0\end{pmatrix}\cdot
\begin{pmatrix}\pi_1\\\pi_2\end{pmatrix} = \begin{pmatrix}1\\ S_0\end{pmatrix}$$

where we have equated the discounted value of the portfolio at \(t_1\) weighted for the risk-neutral probabilities to the initial value of the portfolio. This is equivalent to

$$\begin{cases}
\pi_1 + \pi_2 = 1\\
\frac{u\:S_0}{1+r}\pi_1 + \frac{d\:S_0}{1+r}\pi_2 = S_0
\end{cases}$$

The solution of the system yields the risk-neutral probabilities

$$\pi_1 = \frac{(1+r)-d}{u-d}\qquad\pi_2 = 1-\pi_1 = \frac{u-(1+r)}{u-d}\tag{1}$$

To guarantee the condition of no-arbitrage it must be verified that \(\pi_1>0\) and \(\pi_2>0\), implying that (substituting from the above equation)

$$d < 1+r < u$$

### Replicating portfolio

With the concept of risk-neutral probability, we price the present value of the future option claim through _replication_. The idea of replication for pricing is to build a portfolio of simple assets that mimicks the payoff of the option. The cost of such portfolio must be the cost of the option under the no-arbitrage condition.

The first portfolio is constituted by an European call option \(C\) on the stock. The solutions for a put only require substituting the formula for the put payoff. The value of this portfolio at \(t_0\) and at \(t_1\) in the two scenarios is

$$C_0\qquad\qquad C_1 = \begin{cases}C_u = \max\left[uS_0-K, 0\right]\quad\text{in }\omega_1\\C_d = \max\left[dS_0-K, 0\right]\quad\text{in }\omega_2\end{cases}$$

where \(K\) is the option strike. \(C_0\) is the present value of the future claim we are interested in.\
To build the replicating portfolio, we slightly adapt the portfolio from the previous section

$$\xi_0 = \delta S_0 + B\qquad\qquad\xi_1 = \begin{cases}\delta uS_0 + B(1+r)\quad\text{in }\omega_1\\\delta dS_0 + B(1+r)\quad\text{in }\omega_2\end{cases}$$

with \(\delta\) amount of stock and \(B\) amount of cash, instead of using the cash as numeraire as above. In practice, at \(t_0\) we borrow \(B\) cash at the risk free rate and we invest in \(\delta\) of the stock. Note that we are not explicitly accounting for the signs of the cash flows.

At \(t_1\) we impose the value of the two portfolios in the two scenarios is the same.

$$\begin{cases}C_u = \delta uS_0 + B(1+r)\\ C_d = \delta dS_0 + B(1+r)\end{cases}$$

We solve the system to find that

$$\delta = \frac{C_u-C_d}{(u-d)S_0}\qquad B = \frac{uC_d - dC_u}{(u-d)(1+r)}$$

The replication must work in the same way also at \(t_0\). This allows to write the present value of future claim \(C_0\) as

$$\begin{align*}
C_0 &= \delta S_0 + B\\
&= \frac{1}{1+r}\left[\left(\frac{(1+r)-d}{u-d}\right)C_u + \left(\frac{u-(1+r)}{u-d}\right)C_d\right]\\
&= \frac{1}{1+r}\left[\pi_1\:C_u + \pi_2\:C_d\right]\\
&= \frac{1}{1+r}\left[\pi_1\:C_u + (1-\pi_1)\:C_d\right]
\end{align*}\tag{2}$$

where we recognize that the present value of the option is the discounted value of the future claim weighted by the risk-neutral probabilitites. It is important to note that the real probabilities \(p\) and \(1-p\) have no bearing over the pricing of the option, meaning that a change in \(p\) does not alter the price of the option. This is a result that may seem counter-intuitive.

## Multi-step formulation

### Dual-step

Starting from the previous one-step model, the two scenarios at \(t_1\) become the starting point for the generation of scenarios at \(t_2\). The claims in the new scenarios thus generated are \(C_{uu}\), \(C_{ud}\), and \(C_{dd}\), where, for example, \(C_{uu} = \max[\delta uuS_0 - K, 0]\). Note that, since the model is _multiplicative_ a move up followed by a move down is equal to the viceversa, i.e. \(C_{ud} = C_{du}\), therefore we have a **recombining tree**.

Using equations (1) and (2) and simplifying the notation for the risk-neutral probability \(\pi=\pi_1\), we express the option claim at \(t_1\) as

$$\begin{align*}
C_u &= \frac{1}{1+r}\left[\pi\:C_{uu} + (1-\pi)\:C_{ud}\right]\\
C_d &= \frac{1}{1+r}\left[\pi\:C_{du} + (1-\pi)\:C_{dd}\right]
\end{align*}$$

and thus substituting into the expression for the value at \(t_0\)

$$\begin{align*}
C_0 &= \frac{1}{1+r}\left[\pi\:C_u + (1-\pi)\:C_d\right]\\
&= \frac{1}{(1+r)^2}\left[\pi^2\:C_{uu} + 2\pi(1-\pi)\:C_{ud} + (1-\pi)^2\:C_{dd}\right]
\end{align*}$$

### Generalization

Since the tree in the CRR binomial model recombines, the generalization to an arbitrary number of steps \(n\) is easy. Each additional step generates exactly \(n+1\) new nodes. By induction, it can be proven that the present value of the option may be written as

$$\begin{align*}
C_0 &= \frac{1}{(1+r)^n}\left[\sum_{i=0}^n\binom{n}{i}\pi^i\left(1-\pi\right)^{n-i}\:\max\left[u^id^{n-i}S_0 - K, 0\right]\right]\\
P_0 &= \frac{1}{(1+r)^n}\left[\sum_{i=0}^n\binom{n}{i}\pi^i\left(1-\pi\right)^{n-i}\:\max\left[K - u^id^{n-i}S_0, 0\right]\right]
\end{align*}$$

for a call and a put option respectively.

#### American options

The binomial model can be used to price American options. The difference is that, at each node of the tree, the future value of the sub-tree departing from the node is compared to the intrinsic value of the option in that node. If the intrinsic value is higher than the discounted future value, it is convenient to exercise the option. The intrinsic value is taken as the value of the claim in that node.\
The formula to calculate the value of an American call option at time \(t\) in the node \(i\) is therefore

$$C_t^i = \max\left[S_t^i - K, \frac{1}{(1+r)^{n-t}}E_t\left[C_{t+1}^{\{i\}}\right]\right]\qquad\forall\:t\in[1,n-1]$$

where \(E_t\left[C_{t+1}^{\{i\}}\right]\) represents the expected value of the sub-tree stemming from node \(i\), and \(S_t^i\) is the value of the underlying in node \(i\) realized through any appropriate combination of up and down moves.\
The formula for an American put option is found in a similar way.

Note that with American options, the whole tree needs to be generated and traversed as we need to take the early exercize decision at every intermediate node.

### Practical parametrization

To be able to use the CRR model in practice, we must define the values of \(u\), \(d\), and \(\pi\), assuming the risk-free \(r\) is known. The parametrization proposed in the CRR binomial model[^1] is

$$u=\exp\left(\sigma\sqrt{\tau}\right),\qquad d=\exp\left(-\sigma\sqrt{\tau}\right),\qquad\pi=\frac{1}{2}\left(1+\sigma\right)$$

where \(\tau\) is the time step obtained as \(\tau=T/n\) where \(T\) is the total time of the simulation and \(n\) the number of steps. Usually, the risk-free rate is substituted for a continuously compounded rate \(R\) such that

$$R = \exp\left(r\tau\right)$$

A common assumption in continuous time models is that a price process follows a _Geometric Brownian Motion (GBM)_ so that \(S_t\sim GBM(\mu,\sigma)\). It can be shown that the binomial distribution tends to a normal distribution for the number of trial going to infinite, and therefore that a binomial process tends to a GBM for the number of steps going to infinite. Therefore, it is useful to calibrate the binomial process such that the GBM is recovered. The above choice of parameters ensures this.

### Properties

The CRR binomial models takes several assumptions. Notably:

- The risk-free rate \(r\) is assumed constant.
- Each node generates only two child nodes.
- The up and down moves are fixed.

There are also advantages:

- Simplicity: the model is technically simple to implement and quick to calculate.
- Discretized: time is taken in discrete steps instead of being continuous, contributing to the simplicity of the formulation.
- It can be used for problems where closed form analytical solutions are not available.
- In the limit of continuous time, it converges to an exact formula, the Black-Schole.

[^1]: Cox, J. C., Ross, S. A., Rubinstein, M., _“Option Pricing: A Simplified Approach”_, Journal of Financial Economics (1979)
[^2]: Gregory Gundersen, [_"Binomial Options-Pricing Model"_](https://gregorygundersen.com/blog/2023/06/03/binomial-options-pricing-model/)
