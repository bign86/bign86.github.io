---
title: Poisson distribution
description: ""
date: 2025-01-18
preview: ""
draft: false
tags: [statistics]
categories: [math, notes]
math: true
---


The Poisson distribution describes the probability of having \(k\) events occurring in a fixed interval of time, given these events occur with a known constant mean number of events per time interval \(\lambda\), and independently of the time since the last event.

$$X\sim \text{Pois}(k;\lambda)\quad\text{with}\quad\begin{cases}k\in\mathbb{N_0}\\\lambda\in\mathbb{R} > 0\end{cases}$$

Its **probability mass function** is

$$f(k;\lambda)=P(X=k)=\frac{\lambda^k e^{-\lambda}}{k!}$$

with \(k\) the number of events in the time interval, \(\lambda\) is the constant mean number of events. The latter can also be expressed in terms of a mean rate \(r\) as

$$\lambda=rt$$

The **expected value** equals the **variance** and is given by \(\lambda\):

$$E[X]=\text{Var}[X]=\lambda$$

#### Assumptions

- The events are independent of one another.
- The average rate at which events occur is independent of any occurrences.
- Two events cannot occur at exactly the same instant.

#### Notes

- The Poisson distribution is invalid if the rate of occurrence is not constant. This is a common case of failure that is better addressed with _mixed Poisson_ and _compound Poisson_ distributions.
- Cases where the probability of having no occurrence in a given time interval is relatively high, can be modeled with a _zero-inflated Poisson distribution_ or through a _Hurdle model_.
- The Poisson distribution can be approximated with a Binomial distribution \(np\rightarrow\lambda\).
- It can be proved that the time between two successive Poisson events is an exponential time.

## Sum

The sum of two Poisson variables is a Poisson variable.

> $$\begin{gather*}\text{Let}\quad X\sim\text{Pois}\left(\lambda_1\right),\quad Y\sim\text{Pois}\left(\lambda_2\right),\quad Z:=X+Y\\\Longrightarrow\quad Z\sim\text{Pois}\left(\lambda_1+\lambda_2\right)\end{gather*}$$
>
> Proof:
>
> Assuming \(X\perp\!\!\!\perp Y\)
> $$\begin{align*}P(Z=k)&=P(X+Y=k)\\&=\sum_{k_1=0}^kP(X=k_1)P(Y=k-k_1) &\text{by independence}\\&=\sum_{k_1=0}^k\frac{\lambda_1^{k_1} e^{-\lambda_1}}{k_1!}\frac{\lambda_2^{k-k_1} e^{-\lambda_2}}{\left(k-k_1\right)!}\\&=e^{-\left(\lambda_1+\lambda_2\right)}\sum_{k_1=0}^k\frac{\lambda_1^{k_1}}{k_1!}\frac{\lambda_2^{k-k_1}}{\left(k-k_1\right)!}\\&=e^{-\left(\lambda_1+\lambda_2\right)}\frac{1}{k!}\sum_{k_1=0}^k\binom{k}{k_1}\lambda_1^{k_1}\lambda_2^{k-k_1} &\text{by definition of binomial coef.}\\&=e^{-\left(\lambda_1+\lambda_2\right)}\frac{\left(\lambda_1+\lambda_2\right)^k}{k!} &\text{by the Binomial Theorem}\end{align*}$$
