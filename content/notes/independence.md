---
title: Independence
description: ""
date: 2024-11-24
preview: ""
draft: false
tags: [statistics]
categories: [math, notes]
math: true
---

#### Events

Depending on the number of events we recognize the following forms of independence:

- **One event:** one event \(A\) is self independent if its likelyhood is exactly 0 or 1.
  $$P(A) = P(A\cap A)\quad\iff\quad P(A) = 0\;\vee\;P(A) = 1$$

- **Two events:** two events \(A\) and \(B\) are independent (\(A \perp\!\!\!\perp B\)) if the occurrence of one does not alter the probability of occurrence of the other. Then the conditional probability of \(A\) given \(B\) does not depend on \(B\)
  $$A \perp\!\!\!\perp B\quad\iff\quad P(A\cap B) = P(A)P(B)$$
  It follows that the conditional probability of \(A\) given \(B\) does not depend on \(B\)
  $$P(A\vert B) = P(A)$$
  and viceversa.

- **More than two events:** for a finite set \(\left\{A_i\right\}_{i=1}^n\) of \(n\) events, two different types of independence can be defined:
  - _Pairwise independence_: the events are pairwise independent if and only if any pair of events is independent.
    $$P(A_j, A_k) = P(A_j)P(A_k)\quad\text{with}\quad j\neq k$$
  - _Mutual independence_: the events are mutually independent if every event is independent of any intersection of the other events.
    $$P\left(\bigcap_{i=1}^k A_{i}\right)=\prod_{i=1}^k P(A_{i})\quad\forall\;k\leq n$$
    This case is a _superset_ of the Pairwise independence and is therefore a _stronger_ condition.

#### Continuous variables

In the case of continuous variables, two random variables \(X\) and \(Y\) are independent if their joint CDF \(F_{X,Y}(x,y)\) is given by the product of their CDFs:
$$\left\{X\leq x\right\}\perp\!\!\!\perp\left\{Y\leq y\right\}\quad\text{if}\quad F_{X,Y}(x,y)=F_{X}(x)F_{Y}(y)\quad\forall\;x,y$$

### Conditional independence

#### Events

Given distinct events \(A,\:B,\:C\), the conditional independence of \(A,\:B\) from \(C\) is defined as
$$(A \perp\!\!\!\perp B\:\vert\:C)\quad\iff\quad P(C) > 0\quad\land\quad\begin{cases}P(A\:\vert\:B, C)=P(A\:\vert\:C)\\P(A,B\:\vert\:C)=P(A\:\vert\:C)\:P(B\:\vert\:C)\end{cases}$$
where the two writings on the right hand side are equivalent in meaning.

> A simple proof is
> $$\begin{align*}P(A,B\:\vert\:C)&=P(A\:\vert\:C)\:P(B\:\vert\:C)\\\frac{P(A,B,C)}{P(C)}&=P(A\:\vert\:C)\:\frac{P(B,C)}{P(C)}\\\frac{P(A,B,C)}{P(B,C)}&=P(A\:\vert\:C)\\P(A\:\vert\:B,C)&=P(A\:\vert\:C)\end{align*}$$

#### Continuous variables

Given the random variables \(X,\:Y,\:Z\), the conditional independence of \(X,\:Y\) from \(Z\) is defined as
$$(X \perp\!\!\!\perp Y\:\vert\:Z)\quad\iff\quad F_{X,Y\vert Z=z}(x,y)=F_{X\vert Z=z}(x)\:F_{Y\vert Z=z}(y)\quad\forall\;x,y,z$$

### Properties

Independence has the following properties:

1. **Symmetry:**
  $$X \perp\!\!\!\perp Y\quad\Rightarrow\quad Y \perp\!\!\!\perp X$$

2. **Decomposition:**
  $$X \perp\!\!\!\perp A,\:B\quad\Rightarrow\quad X \perp\!\!\!\perp A\:\:\land\:\:X \perp\!\!\!\perp B$$

3. **Weak union:**
  $$X \perp\!\!\!\perp A,\:B\quad\Rightarrow\quad X \perp\!\!\!\perp A\:\vert\:B\:\:\land\:\:X \perp\!\!\!\perp B\:\vert\:A$$

4. **Contraction:**
  $$X \perp\!\!\!\perp A\:\vert\:B\quad\land\quad X \perp\!\!\!\perp B\quad\Rightarrow\quad X \perp\!\!\!\perp A,\:B$$

5. **Intersection:**
  $$X \perp\!\!\!\perp Y\:\vert\:Z,\:W\quad\land\quad X \perp\!\!\!\perp W\:\vert\:Z,\:Y\quad\Rightarrow\quad X \perp\!\!\!\perp W,\:Y\:\vert\:Z$$
