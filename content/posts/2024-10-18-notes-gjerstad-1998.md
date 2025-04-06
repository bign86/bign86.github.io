---
title: "Notes on: *Price formation in double auctions*"
description: ""
date: 2024-10-18
preview: ""
tags:
  - finance
  - literature review
categories:
  - finance
author_profile: true
draft: false
comments: false
math: true
slug: notes-price-formation-double-auctions
---

_Paper_: "Price formation in double auctions", Gjerstad and Dickhaut, Games and Economic Behavior, 22, 1, 1998.

The paper develops a model of information processing and strategy choice for agents in a double auction market.

Marketing experiments have established the transaction prices converge quickly to a competitive equilibrium price in the double auction for a wide variety of market environment, In particular with stationery supply and demand.
In particular with stationery supply and demand.
We demonstrate that the persistent puzzle of convergence can be resolved with traders whose information, processing and strategy choices are simple and intuitive.
Each trader independently determines an offer believed to maximize expected surplus only observing market activity.

Economic system \(S=(e,I)\) with environement \(e=\prod_{i\in A}e^i\) with \(A=\{1,\ldots,n\}\) agents w/ preferences \(e^i\). \(I\) is the institution intended as the message space for all agents.

To determine the behavioral actions \(\{\beta^i(H_t\vert e^i,I)\}_{i\in A}\) with \(H_t\) history of activity at time \(t\).

The buyer monetary gain is \(\sum_{k=1}^{\nu_i}\left(v_i^k-p_k\right)\), and therefore the
buyer utility \(U^b_i \left(\sum_{k=1}^{\nu_i}\left(v_i^k-p_k\right)\right)\)

The seller monetary gain is \(\sum_{k=1}^{\nu_i}\left(p_k-c_i^k\right)\), and therefore the
seller utility \(U^s_i \left(\sum_{k=1}^{\mu_i}\left(p_k-c_i^k\right)\right)\)

where \(U^b, U^s\) are the utility function for buyers and sellers, both monotonically increasing. \(p_k\) is the transaction price of unit \(k\), \(c_k^i\) cost for the seller \(i\), \(v_k^i\) redemption value for the buyer, with both summations running to a total quantity per agent lower than the maximum inventory of the agent.

The simulation runs in discrete trading periods in which each trader tries to maximize its own monetary gain. The traders have memory of the last \(L\) transactions, and use it to form the belief \(p(a)\) that an ask amount \(a\) will be accepted by a buyer or the belief \(p(b)\) that a bid amount \(b\) will be accepted by a seller.\
The belief \(p(a)\) is a monotonically non-increasing function, while the belief \(p(b)\) is monotonically non-decreasing. They are given by:

$$\begin{align}
p(a) &= \frac{\sum_{d\geq a}TA(d) + \sum_{d\geq a}B(d)}{\sum_{d\geq a}TA(d) + \sum_{d\geq a}B(d) + \sum_{d\leq a}RA(d)}
= \frac{TAG(d) + BG(d)}{TAG(d) + BG(d) + RAL(d)}\\
p(b) &= \frac{\sum_{d\leq a}TB(d) + \sum_{d\leq a}A(d)}{\sum_{d\leq a}TB(d) + \sum_{d\leq a}A(d) + \sum_{d\geq a}RB(d)}
= \frac{TBL(d) + AL(d)}{TBL(d) + AL(d) + RBG(d)}
\end{align}$$

where \(A(d)\) is the total number of asks at \(d\), \(TA(d)\) and \(RA(d)\) the accepted and rejected asks respectivelly, under the constrain \(A(d)=TA(d)+RA(d)\). The same applied to bids \(B(d)\).

The authors explore through simulation the effect that belief formation and strategy choices have on the model convergence to competitive equilibrium measured by the mean absolute deviation of transaction prices from the equilibrium price.

Results:
- The market allocations are efficient (close to 100% efficiency)
- Transaction prices converge to an equilibrium and allocation are Pareto optimal, idicating that competitive equilibrium is reached

The authors conclude that agents adopting the strategy of surplus maximization using the belief function described above, converge to market equilibrium prices and reach competitive equilibrium.
