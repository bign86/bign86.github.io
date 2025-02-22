---
title: Median
description: ""
date: 2024-12-28
preview: ""
draft: false
tags: [statistics]
categories: [math, notes]
math: true
---

The median is defined as the value that separates the sample dataset in two parts of equal size, corresponds to the 50th percentile, and it is therefore considered a good representation of the "center" of the dataset.
The median is well-defined for any ordered 1-D dataset, although for discrete datasets the median may not be unique if it falls in between two discrete values. It cannot be defined however for nulti-dimensional datasets where a unique ordering is not available.
It is commonly used in applications where a low sensitivity to outliers is desirable as it is a robust statistic.

It can be defined as

$$P[X\leq m)\geq\frac{1}{2} \text{ and } P[X\geq m)\geq\frac{1}{2}$$

In this definition \(X\) is a random variable that does not need to have a continuous distribution and therefore an associated probability density function \(f\).
In general, any distribution has a median, but pathological cases exists. One such case is a distribution that has cumulative probability distribution \(F=1/2\) for a finite interval.
In this case any value in the interval satisfies the definition of median. This is the continuous version of the discrete case of a median falling in between two values.

### Properties

**Optimality**: the value \(m\) is the median if and only if it minimizes the mean absolute error \(E\left[\vert X - c\vert\right]\). Hence
$$m=\min\left\{E\left[\vert X - c\vert - \vert X\vert\right]\right\}$$

**Mean - median distance**: if the distribution has finite variance the maximum distance between mean and median is one standard deviation \(\sigma\).

> Proof:\
> $$\begin{align*}\vert\mu - m\vert = \vert E\left[X-m\right]\vert &\leq E\left[\vert X-m\vert\right]\\ &\leq E\left[\vert X-\mu\vert\right]\\ &\leq \sqrt{E\left[\left( X-\mu\right)^{2}\right]} = \sigma\end{align*}$$
> with mean \(\mu\). This proof makes use of the Jensen's inequality in the absolute value of the first step and in the square in the last step, both of which are convex functions. The second inequality comes from the fact that the median minimizes the absolute deviation.

**Jensen's inequality**: the Jensen's inequality can be generalized to median
$$f\left(\text{med}\left[X\right]\right)\leq\text{med}\left[f(X)\right]$$

**Distribution of the sample median**: the median of a sample drawn from a population is a random variable. With \(f\) density function of the population, the density function of the sample median is asymptotically normal and given by
$$\text{Sample median}\sim\mathcal{N}\left(\mu=m,\:\sigma^2=\frac{1}{4nf(m)^2}\right)$$
with \(m\) median and \(n\) sample size.

In the special case of the median of a normal sample, the variance of the median is given by
$$\frac{\pi\sigma^2}{2n}$$

**Efficiency**: ratio of the variance of the mean to the variance of the median, hence how quicker the mean converges to the true value with respect to the median as the sample becomes larger. Assuming a sample from the normal distribution, the efficiency is given by
$$\frac{2}{\pi}\frac{n+2}{n}\quad\xrightarrow[n\to\infty]{}\quad\frac{2}{\pi}$$

https://brilliant.org/wiki/median-finding-algorithm/\
https://rcoh.me/posts/linear-time-median-finding/
