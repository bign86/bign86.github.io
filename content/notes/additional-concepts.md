---
title: Additional basic concepts
description: ""
date: 2024-11-24
preview: ""
draft: false
tags: [statistics]
categories: [math, notes]
math: true
---

Basic definitions and notations used here are found [in this page](/notes/probability-definitions).

### Law of total probability

The law of total probability connects marginal probabilities to conditional probabilities, expressinng the total probability of an outcome as the sum of the conditional probabilities of distinct events.

- **Events:** with \(\left\{B_n\::\:n=1,2,\ldots\right\}\) is a finite or countably infinite set of mutually exclusive and collectively exhaustive events then for any event \(A\)
  $$P(A)=\sum_nP(A\cap B_n)=\sum_nP(A\:\vert\:B_n)\:P(B_n)$$
  The same rule can be written for _conditional probabilities_
  $$P(A\:\vert\:C)=\sum_nP(A\:\vert\:B_n\:C)\:P(B_n\:\vert\:C)$$

- **Continuous variables:** for continuous variables the rule is defined as
  $$P(A)=\int_{\mathbb{R}}P(A\:\vert\:X=x)\:dF_X(x)\stackrel{*}{=}\int_{\mathbb{R}}P(A\:\vert\:X=x)\:f_X(x)\:dx$$
  where the step marked with \(\stackrel{*}{=}\) is only valid if \(F_X\) admits a density function \(f_X\).
  
### Chain rule

The probability of the intersection of \(n\) events can be written as

$$\begin{align*}P(A_1\:\cap\:\ldots\:\cap\:A_n)&=P(A_1\:\vert\:A_2\:\cap\:\ldots\:\cap\:A_n)\:P(A_2\:\cap\:\ldots\:\cap\:A_n)\\&=\prod_{k=1}^{n}\:P\left(A_k\:\vert\:\bigcap_{j=k+1}^{n}\:A_j\right)\end{align*}$$

Following the same principle, the chain rule can be written for \(n\) random variables \(X_1,\ldots,X_n\) and \(x_1,\ldots,x_n\in\mathbb{R}\): as

$$\begin{align*}P(X_1=x_1,\ldots,X_n=x_n)&=P(X_1=x_1\:\vert\:X_2=x_2,\ldots,X_n=x_n)\:P(X_2=x_2,\ldots,X_n=x_n)\\&=\prod_{k=1}^{n}\:P\left(X_k=x_k\:\vert\:\bigcap_{j=k+1}^{n}\:X_j=x_j\right)\end{align*}$$
