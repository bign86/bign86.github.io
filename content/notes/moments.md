---
title: Moments and cumulants
description: ""
date: 2024-11-24
preview: ""
draft: false
tags: [statistics]
categories: [math, notes]
math: true
---

## Moments

Moments are quantitative measures that can be related to the shape of a probability distribution.

**Raw moments \(\mu'_n\)** are the simplest definition of moments as the weighted average of the \(n\)-th power of a random variable.

$$\mu'_n = E\left[X^n\right] = \begin{cases}\sum_i x_i^n p_i & \text{Discrete variables}\\\int_{\mathbb{R}} x^n f(x)\:dx & \text{Continuous variables}\end{cases}$$

The raw moment can be generalized by defining it up to a constant shift

$$\int\left(x - c\right)^n f(x)\:dx$$

It is simple to see that the raw moment is the special case \(c=0\).

**Central moments \(\mu_n\)** are the special case with \(c=E[X]=\mu\). Then

$$\mu_n = E\left[\left(X - E[X]\right)^n\right] = \begin{cases}\sum_i p_i \left(x_i - \mu\right)^n & \text{Discrete variables}\\\int_{\mathbb{R}} \left(x - \mu\right)^n f(x)\:dx & \text{Continuous variables}\end{cases}$$

Further generalizing, moments can be defined up to a scaling factor

$$\int\left(\frac{x - c}{s}\right)^n f(x)\:dx$$

**Standardized moments \(\tilde{\mu}_n\)** are the special case where \(s=\text{Std}[X]=\sigma\)

$$\tilde{\mu}_n = \frac{E\left[\left(X - E[X]\right)^n\right]}{\left(E\left[\left(X - E[X]\right)^2\right]\right)^{n/2}} = \frac{\mu_n}{\sigma^n} = \frac{\mu_n}{\mu_2^{n/2}}$$

### Moment generating function

A moment generating function (MGF) defines the moments of a distribution and can be used to uniquely determine a probability distribution. However, there are real-valued distributions that do not have a moment generating function or moments. So, a moment generating function cannot be always used.

With \(X\) random variable, the MGF \(M_X(t)\) is given by the expectation of the exponential of the random variable

$$M_X(t) := E\left[e^{tX}\right] = \begin{cases}\sum_{i=0}^\infty e^{tx_i}p_i &\text{discrete variable}\\\int_{-\infty}^{\infty}e^{tx}f(x)\:dx &\text{continuous variable}\end{cases}$$

if the expectation \(E[\cdot]\) exists in a neighborhood of \(t=0\). If the function exists, the n-th moment \(m_n\) can be calculated as

$$m_n = M_X^n(0) = \left.\frac{d^n\:M_X}{dt^n}\right|_{t=0}\qquad\text{with }n\in\mathbb{N}>0$$

#### Properties of MGF

- **Log-convexity**: MGF are log-convex.
- **Distribution specification**: with \(X\) and \(Y\) random variables
  $$\text{if}\quad M_X(t)=M_Y(t)\quad\Longrightarrow\quad F_X(x)=F_Y(x)$$

### Common distribution moments

- **Expected value \(E[X],\:\mu\)**: first _raw_ moment and a generalization of the weighted average. Is the mean of the possible values of the random variable \(X\).
  $$E[X] = \begin{cases}\sum_i x_i p_i & \text{Discrete variables}\\\int_{-\infty}^{\infty} x f(x)\:dx & \text{Continuous variables}\end{cases}$$

- **Variance \(\text{Var}[X],\:\sigma^2\)**: second _central_ moment. Is defined as the expected value of the squared deviation from the mean.
  $$\text{Var}[X] = E\left[\left(X-\mu\right)^2\right] = \begin{cases}\sum_i p_i \left(x_i - \mu\right)^2 & \text{Discrete variables}\\\int_{\mathbb{R}} \left(x - \mu\right)^2 f(x)\:dx & \text{Continuous variables}\end{cases}$$

- **Skewness \(\text{skew[X]}\)**: third _standardized_ moment. Measure of distribution asymmetry.
  $$\text{skew[X]} = E\left[\left(\frac{X - \mu}{\sigma}\right)^3\right]$$

- **Kurtosis \(\text{kurt[X]}\)**: fourth _standardized_ moment. Measure of distribution "tailedness".
  $$\text{kurt[X]} = E\left[\left(\frac{X - \mu}{\sigma}\right)^4\right] = \frac{E\left[\left(X - \mu\right)^4\right]}{\left(E\left[\left(X - \mu\right)^2\right]\right)^2}$$

## Cumulants

Cumulants are quantities alternative to moments that, like moments, can be used to define a probability distribution.

Cumulants of \(X\) are defined using a _cumulants generating function_ \(K(t)\) which is the logarithm of the _moment generating function_

$$K(t)=\log\:E\left[e^{tX}\right]$$

following the same requirements as for moments. If the cumulant function exists around \(t=0\), the k-th cumulant \(k_n\) can be calculated as

$$k_n = K_X^n(0) = \left.\frac{d^n\:\log M_X}{dt^n}\right|_{t=0}\qquad\text{with }n\in\mathbb{N}>0$$

### Internal links

[Expected value](/notes/expected-value)
