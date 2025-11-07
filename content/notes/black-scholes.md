---
title: Black-Scholes equation
description: ""
date: 2025-08-16
preview: ""
draft: false
tags:
  - basic finance
  - options
  - Black-Scholes
categories:
  - finance
  - notes
math: true
slug: black-scholes
---

The Black-Scholes model[^1] is a famous method to obtain an estimate of the price of an European option that brought to the popularization of options and the creation of the first option exchanges. The formula is largerly employed by pratictioners around the globe.

### Fundamental hypothesis

The following assumptions are made about the asset:

- It exists a riskless asset whose return is the risk-free rate. The risk-free rate is unique and constant.
- The price of the asset cannot be negative.
- The asset does not pay dividends.

Some assumptions are also made about the market. In particular, it is assumed the weak form of the efficient market hypothesis, so that

- The market responds instantaneously to external influences.
- Asset prices reflect all past information.
- Future prices are independent of the past, therefore knowledge of the price history does not grant any advantage.
- There are no arbitrage opportunities.
- There are no transaction costs (frictionless market).
- Buying and selling can happen at any time and for any quantity of the asset.
- Short-selling is allowed.

Under these conditions the model calculates the fair price at the present time of a derivative security having a certain payoff at a future date depending on the price of the underlying asset at that date.

## Itô's lemma

When a function \(f\) is applied to a stochastic process \(X_t\), the result is another stochastic process \(f(X_t)\) that is generally very complex to handle mathematically, in particular when it needs differentiating. The Itô's lemma[^2] solves the problem by employing a Taylor-expansion approach to expressing the chain-rule of \(f(X_t)\) in a tractable form. This is relevant to the option pricing problem as the option claim \(V\) depends on the stochastic price of the underlying, as in \(f(X_t) = V(S_t)\).

**Itô's process**: a stochastic process expressed as the integral of a drift and the integral of a Wiener process such as a Brownian motion

$$X_t = X_0 + \int_0^t a_s ds + \int_0^t b_s dW_s\tag{1}$$

with \(W_t\) a m-dimensional Wiener process. If \(a_t\) and \(b_t\) do not depend on the process \(X_t\) but only on time, it is possible to determine the expected value and variance of the problem very easily

$$\begin{align*}
E\left[X_t\right] &= \int_0^t a_s ds\qquad\text{as}\:E\left[dW_t\right] = 0 \\
\text{Var}\left[X_t\right] &= \int_0^t b_s^2 ds\qquad\text{as}\:\text{Var}\left[dW_t\right] = 1
\end{align*}$$

and considering that \(dW_t\) are uncorrelated from one another.\
But if \(a_t\) and \(b_t\) DO depend on the process itself and can be written as a function applied to the stochastic process, like in the case below, this integral approach is not feasible.

$$f(X_t) = x + \int_0^t a(X_s,s) ds + \int_0^t b(X_s,s) dW_s\tag{2}$$

All these processes can be expressed in the differential form. Equations (1) and (2) are often found as

$$\begin{align*}
dX_t &= a_t\:dt + b_t\:dW_t \\
df(X_t) &= a(X_t,t)\:dt + b(X_t,t)\:dW_t,\qquad X_0=x
\end{align*}$$

**Itô's lemma**: Given \(X_t\) Itô's process satisfying (1) and \(f(X_t)\) twice-differentiable scalar function, the Itô's lemma states that the process \(f(X_t)\) in (2) is itself also an Itô's process, and gives a recipe through a Taylor-series expansion to calculate its derivative as in

$$df(X_t) = f'(X_t)\:dt + \frac{1}{2}f''(X_t)\:dt + f'(X_t)\:dW_t$$

The result of the Itô's lemma applied to (2) is therefore

$$\begin{align*}
df(X_t) &= \frac{\partial f}{\partial t}\:dt + \frac{\partial f}{\partial x}\:dW_t + \frac{1}{2}\frac{\partial^2 f}{\partial x^2}\:dW_t^2\\
&= \left(\frac{\partial f}{\partial t} + a_t\frac{\partial f}{\partial x} + \frac{1}{2}b_t^2\frac{\partial^2 f}{\partial x^2}\right)\:dt + b_t\frac{\partial f}{\partial x}dW_t\tag{3}
\end{align*}$$

## Derivation of Black-Scholes

We apply the Itô's lemma to the stochastic process that generates the underlying's price allowing to come to a solvable differential equation. The solution of the differential equation express the value of the option.

The price of the underlying follows a geometric Brownian motion as in

$$dS_t = \mu S_t\:dt + \sigma S_t\:dW_t\tag{4}$$

Assume a European option with claim \(V(S_t)\) and apply the Itô's lemma. We obtain

$$dV(S_t, t) = \left(\frac{\partial V_t}{\partial t} + \mu S_t\frac{\partial V_t}{\partial S_t} + \frac{1}{2}\sigma^2 S_t^2\frac{\partial^2 V_t}{\partial S_t^2}\right)\:dt + \sigma S_t\frac{\partial V_t}{\partial S_t}\:dW_t\tag{5}$$

where \(a(S_t) = \mu S_t\) and \(b(S_t) = \sigma S_t\). This is the standard Black-Scholes formulation where \(\mu\) and \(\sigma\) are taken as constants over the life of the option.

Consider now a self-financing trading strategy where the portfolio holds \(x_t\) of the cash account and \(y_t\) of the underlying. The value \(P_t\) of the portfolio is

$$P_t = x_t B_t + y_t S_t\tag{6}$$

where \(B_t\) represents a amount of cash invested at the constant risk-free rate \(r\). We now calculate the partial derivatives of (6) as found in (5) to apply the Itô's lemma to the portfolio, obtaining

$$
\frac{\partial P_t}{\partial t} = x_t r B_t,\qquad
\frac{\partial P_t}{\partial S_t} = y_t,\qquad
\frac{\partial^2 P_t}{\partial S_t^2} = 0
$$

The Itô's lemma (5) applyied to the portfolio yields the change of the portfolio value as time progresses. We can do this as the value of the portfolio depends on the price of the underlying expressed by (4), just like the option claim. This results in

$$\begin{align*}
dP(S_t,t) &= \left(\frac{\partial P_t}{\partial t} + \mu S_t\frac{\partial P_t}{\partial S_t} + \frac{1}{2}\sigma^2 S_t^2\frac{\partial^2 P_t}{\partial S_t^2}\right)\:dt + \sigma S_t\frac{\partial P_t}{\partial S_t}\:dW_t\\
&= \left(x_t r B_t + y_t \mu S_t + \frac{1}{2}\sigma^2 S_t^2\cdot 0\right)\:dt + y_t\sigma S_t\:dW_t\\
&= \left(x_t r B_t + y_t\mu S_t\right)\:dt + y_t\sigma S_t\:dW_t\tag{7}
\end{align*}$$

Using a replication argument and since the strategy is self-financing, we can equate the value of the option claim and the value of the portfolio at \(t_0=0\), and the change in the value of the option to the one of the portfolio, as both depend on the same stochastic process. By the replication argument, it must then be that the value of the option and the one of the portfolio must be equal at all times:

$$V_0 := P_0\quad\text{and}\quad dV_t = dP_t\qquad\Longrightarrow\qquad V_t = P_t\quad\forall t\tag{8}$$

Therefore, using (8), we equate the coefficients of (5) and (7) to find

$$\begin{cases}
x_t r B_t + y_t\mu S_t = \frac{\partial V_t}{\partial t} + \mu S_t\frac{\partial V_t}{\partial S_t} + \frac{1}{2}\sigma^2 S_t^2\frac{\partial^2 V_t}{\partial S_t^2}\\
y_t\sigma S_t = \sigma S_t\frac{\partial V_t}{\partial S_t}
\end{cases}
\qquad\Longrightarrow\qquad
\begin{cases}
x_t r B_t = \frac{\partial V_t}{\partial t} + \frac{1}{2}\sigma^2 S_t^2\frac{\partial^2 V_t}{\partial S_t^2}\\
y_t = \frac{\partial V_t}{\partial S_t}
\end{cases}\tag{9}$$

Lastly, by virtue of (8) and substituting (9) into (6)

$$\begin{align*}
P_t &= x_t B_t + y_t S_t\\
V_t &= x_t B_t + y_t S_t &\text{using (8)}\\
&= \frac{1}{r}\left(\frac{\partial V_t}{\partial t} + \frac{1}{2}\sigma^2 S_t^2\frac{\partial^2 V_t}{\partial S_t^2}\right) + S_t\frac{\partial V_t}{\partial S_t}&\text{using (9)}
\end{align*}$$

This equation is usually rearranged to yield the **Black-Scholes equation**[^1]

$$\frac{\partial V_t}{\partial t} + \frac{1}{2}\sigma^2 S_t^2\frac{\partial^2 V_t}{\partial S_t^2} + r S_t\frac{\partial V_t}{\partial S_t} - r V_t = 0\tag{10}$$

with \(\mu\), \(\sigma\) and \(r\) constants.

## Solutions of the Black-Scholes equation

The Black-Scholes equation (10) is a partial differential equation (PDE) in the price of the option. To solve it, a choice must be made on the type of option (put/call) and border conditions need to be defined.
Here are reported the solutions for call and put options. The derivation of the solution is presented in full [here](/notes/black-scholes-solution).

#### Call option

The following border conditions are true for a call

$$\begin{align*}
C(S_T, T) &= \max\left[S_T - K,0\right]\\
C(0, t) &= 0 &\forall t\\
C(S_t, t) &\to S_t - Ke^{-r\tau} &\text{with }S_t\to\infty\quad\forall t
\end{align*}$$

where \(\tau = T-t\). Solving the equation yields

$$C(S_t,t) = S_t \Phi(d_1) - e^{-r\tau}\:K\Phi(d_2)$$

where \(\Phi(\cdot)\) is the CDF of the normal distribution, and

$$\begin{align*}
d_1 &= \frac{1}{\sigma\sqrt{\tau}}\left(\log\left(\frac{S_t}{K}\right) + \left(r + \frac{\sigma^2}{2}\:\tau\right)\right)\\
d_2 &= d_1 - \sigma\sqrt{\tau}
\end{align*}$$

#### Put option

The following border conditions are true for a put

$$\begin{align*}
P(S_T, T) &= \max\left[K - S_T,0\right]\\
P(0, t) &= Ke^{-r\tau} &\forall t\\
P(S_t, t) &\to 0 &\text{with }S_t\to\infty\quad\forall t
\end{align*}$$

Solving the equation yields

$$P(S_t,t) = e^{-r\tau}\:K\Phi(-d_2) - S_t \Phi(-d_1)$$

where the symbols have the same meaning as for a call option.

[^1]: Fischer Black, Myron Scholes (1973), _"The Pricing of Options and Corporate Liabilities"_, Journal of Political Economy 81 (3), 637–654.
[^2]: Kiyosi Itô (1944), _"Stochastic Integral"_, Proc. Imperial Acad. Tokyo 20, 519–524.
