---
title: Cash Flows
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
slug: cash-flows
---

## Interest rates

An interest rate \(r\) is used to gross a present value of money \(PV\) to obtain a future value of money \(FV\).

$$FV = PV (1 + r)$$

If the same is repeated over several periods \(n\), the interest is compounded. There are two basic definitions of compounding:

- **Simple interest rate**: The value grows by the same monetary quantity in every period
  $$PV(1 + nr)$$
- **Compound interest rate**: The value grows by the same percentage quantity in every period
  $$PV(1 + r)^n$$

_Annualization_: often interest rates are quoted based on standard periods (tipically  one year, hence the word annualization) but are compounded at a different frequency. In this case the convention is

$$PV \left(1 + \frac{r}{m}\right)^{nm}$$

For example a rate can be quoted as an annual value and be compounded monthly for 5 years. In this case \(n=5\) and \(m=12\).

- **Continuous interest rate**: This is the limit where the compounding frequency is infinite
  $$\lim_{m\rightarrow\infty} PV\left(1 + \frac{r}{m}\right)^{nm} = PV\:e^{nr}$$

The continuous compounding is not really used in practice but has two important properties:

- It represents an upper bound to compounding. Taking the compounded interest rate, it can be showed that the \(FV\) grows with the compounding frequency. The limit is given by the continuous compounding.
- The exponential form is very well behaved mathematically and helps keeping complex models mathematically tractable. For example, the rate of growth of a sum of money would be
  - in the non-continuous case
    $$\frac{d}{dt}\:\left(1 + \frac{r}{m}\right)^{mt} = m\:\log\left(1 + \frac{r}{m}\right)\:\left(1 + \frac{r}{m}\right)^{mt}$$
  - in the continous case
    $$\frac{d}{dt}\:e^{rt} = \:r\:e^{rt}$$

### Discount factors

If the future value of a present monetary amount is found by grossing, the opposite operation is called **discounting**. Assuming a single period:

$$PV = FV \frac{1}{(1 + r)} = FV\:d$$

where \(d\) is called _discount factor_.\
As usually interest rates are positive, the discount factor is usually lower than 1.

### Term structure

So far it has been assumed a single interest rate \(r\). However, the interest rate value can be defined as depending on a tenor, the amount of time the interest rate is applied to, using the formulas above. For example, assuming a constant yearly rate \(r_{1y}\), the 2-year interest rate can be defined as:

$$r_{2y} = \left(1 + r_{1y}\right)^2 - 1$$

In practice, the real interest rate at varying tenors follow a more complex curve, called the **term structure** or **yield curve**. Each point of the curve is called a **spot rate**, the interest rate that can be used to discount a cash flow received at the corresponding tenor to today.

The term structure of spot rates univocally determines a **forward curve**. The points in the forward curve are the interest rates that can be used to discount a cash flow between tenors in the yield curve.\
Called \(s_1\) the spot rate applicable to discount a cash flow received 1 year from now to the present, and \(s_2\) the spot rate applicable to discount a cash flow received 2 years from now to the present, it must be that

$$\left(1 + s_2\right) = \left(1 + s_1\right) \left(1 + f_{1,2}\right)$$

with \(f_{1,2}\) the **forward rate**.

Analogously, the yield curve also univocally determines a discount curve as well as forward discount factors obtained as:

$$\frac{1}{1 + s_2} = d_2 = d_1\:d_{1,2} = \frac{1}{1 + s_1} \frac{1}{1 + f_{1,2}}$$




