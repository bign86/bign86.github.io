---
title: Probability and distributions
description: ""
date: 2024-11-24
preview: ""
draft: false
tags: [statistics]
categories: [math, notes]
math: true
---

What follows are the basic definitions of the fundational elements in statistics.

An **outcome** \(\omega\) is a possible result of an experiment or trial.

An **event** \(A=\{\omega_i\}\) is a set of outcomes realizable from a trial and is a subset of the sample space \(A\subseteq\Omega\).

A **sample space** \(\Omega\) is the set of all possible outcomes \(\omega\) of an experiment. To be a sample space, a given set must have the following properties:
- The outcomes must be _mutually exclusive_ so that the realization of one outcome excludes the realization of all the others.
  $$A,\:B\:\:\text{mutually exclusive}\:\iff\:A\neq B\;\rightarrow\;A\cap B=\varnothing\quad\forall\:A, B\in\Omega$$
  $$\Rightarrow P(A\cap B) = 0$$
- The outcomes must be _exhaustive_ so that any potential outcome of a trial is contained in the set

The **probability** \(P(A)\) of an event \(A\) is the likelyhood the event will occur and \(P(A)\in[0,1]\) in \(\mathbb{R}\).

Moreover, for two events \(A\) and \(B\) the following definitions can be given:

- **Joint probability:** the joint probability \(P(A\cap B)\) or \(P(A\land B)\) or \(P(A,B)\) is the likelyhood both events will happen at the same time.
  $$\begin{align*}P(A\cap B)&=P(A\:\vert\:B)\:P(B)\\&=P(B\:\vert\:A)\:P(A)\end{align*}$$
  This can also be seen as the definition of the Bayes' theorem
  $$P(A\:\vert\:B)=\frac{P(B\:\vert\:A)\:P(A)}{P(B)}$$

- **Conditional probability:** the conditional probability \(P(A\:\vert\:B)\) is the likelihood an event \(A\) will occur given that another event \(B\) has already occurred.
  $$P(A\:\vert\:B) = \frac{P(A\cap B)}{P(B)}$$

## Probability distributions

The **probability distribution** \(P\) is the mathematical function that calculates the likelyhood of an event from an experiment.
$$P(A\subseteq\Omega)\;\rightarrow\;[0, 1]\in\mathbb{R}$$
Formally, to be a probability distribution, a function must satisfy the 3 _Kolmogorov axioms_:

1. The probability is a non-negative real number
$$P(A)\in\mathbb{R},\;P(A)\geq 0\qquad\forall\:A\subseteq\Omega$$
1. The probability that an outcome (any in the sample space) occur is 1
$$P(\Omega)=1$$
1. The probability of mutually exclusive events is the sum of the probabilities of each individual event
$$P\left(\bigcup_{i=1}^\infty A_{i}\right)=\sum_{i=1}^\infty P(A_{i})$$

Distributions may be defined for discrete or continuous random variables.\
For discrete random variables the likelyhood of each possible outcome is given by the _Probability Mass Function (PMF)_. For continuous random variables the relative likelihood of each value is given by the _Probability Density Function (PDF)_, while the _Cumulative Distribution Function (CDF)_ evaluates the probability the random variable will take a value up to \(x\).

#### Properties of distributions

From the Kolmogorov axioms several properties can be derived:

- **Monotonicity**: given an event \(B\) and a subset of it \(A\), the probability of the superset is never smaller than the probability of the subset
$$\text{if}\quad A\subseteq B\quad\Rightarrow\quad P(A)\leq P(B)$$
From this rule is easy to prove that \(0\leq P(A)\leq 1\quad\forall A\).

- **Empty set**: the empty has probability zero
$$P(\varnothing)=0$$

- **Complement**: the probability of the complement event \(A^c\) is
$$P(A^c)=P(\Omega-A)=P(\Omega)-P(A)=1-P(A)$$
The complement can also be interpreted as the probability an event will _not_ happen \(P(\lnot A)\).

- **Sum rule**: The probability of either one of two events happening is given by
$$P(A\cup B)=P(A)+P(B)-P(A\cap B)$$
where \(P(A\cap B)\) is the intersection of the two events.

### Definitions related to distributions

A **random variable** is a function that takes values from a sample space with likelyhood given by the probability distribution.

The **support** of the distribution is the subset of the function domain \(X\) having non-zero probability
$$\text{with}\:f:X\rightarrow\mathbb{R},\quad\text{supp}(f)=\{x\in X:f(x)\neq 0\}$$

The **q-quantile** \(x\) is the value such that
$$P(X < x)=q$$

The **median** is the value such that the probability of finding values greater or smaller than the median is 50%
$$m=\text{med}(X)\::\:P(X < m)=P(X > m)=0.5$$

The **mode** is the most likely value from the sample space if discrete, the value at which the probability density function peaks if continuous.

### Recap

The following table summarizes the most basic properties with the appropriate notation for one event and the combination of two events

| events    | notation                     |  formula                              |
|:---------:| ---------------------------- | ------------------------------------- |
| A         | \(P(A)\)                     | \(\in [0, 1]\)                        |
| not A     | \(P(\lnot A)\)               | \(1 - P(A)\)                          |
| A or B    | \(P(A \cup B)\)              | \(P(A) + P(B) - P(A \cap B)\)         |
| A and B   | \(P(A \cap B)\), \(P(A, B)\) | \(P(A\vert B)P(B) = P(B\vert A)P(A)\) |
| A given B | \(P(A\vert B)\)              | \(\frac{P(A, B)}{P(B)}\)              |

