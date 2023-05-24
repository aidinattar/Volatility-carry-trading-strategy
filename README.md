# Volatility carry trading strategy
Modelling the implicit volatility, using multi-factor statistical models.

Laboratory of Computational Physics course A.Y. 2021/2022.

Project by Aidin Attar, Bahador Amjadi, Ema Baci and Melika Keshavarzmirzamohammadi

Prof. Marco Zanetti

**XSOR Capital LTD**
- Stefano Soricetti
- Alessio Pieri

## Introduction

### What is Volatility?

Volatility is one of the standard measures of risk in ﬁnancial markets. From a statistical point of
view, volatility is the annualized standard deviation of the yield of an underlying asset. There are
two main types of volatility we can think of:
 - Realized volatility : realized volatility is computed on the historical yields of the underlying.
For instance, it may be computed on the past yields of an index.
 - Implied volatility : implied volatility is estimated from the prices of options on the underlying,
and it represents the expectations of the market on the volatility of the underlying from now
until the expiry.

From a market point of view, the implied volatility is more interesting, as it provides some kind
of information on the future evolution of the actual volatility.

### What is VIX and the structure of futures on VIX

VIX (which stands for Volatility Index) is the most popular and liquid volatility index on the
market. It represents the expectation of the market on the volatility at 30 days of the S&P 500
index, as derived by looking at its options, as outlined above. Since the volatility is at 30 days,
this has so called constant maturity.

Apart from “VIX cash”, there are futures on VIX which have predetermined expiry. Usually,
the curve of futures on VIX is in [contango](https://en.wikipedia.org/wiki/Contango), meaning that the futures with nearer maturity have a
lower price than those at further maturity.

You can see an example of the curve of futures on VIX in Fig. [1.](#br1)

<p align="center">
<img src="figures/VIX.png"  width="600"/> </p>
<p align="center">
Figure 1: A typical example of the curve of futures on VIX.



## The trading idea

If an investor were to replicate the “cash VIX” (that is, with a ﬁxed duration of 30 days) using
the existing futures, she would have to rebalance her futures portfolio on a daily basis. In more
practical terms, consider once again for instance Fig. [1.](#br1)[ ](#br1)The investor would have to rebalance
her exposure to the November (expiry: November 19th) and December (expiry: December 17th)
futures in order to have a 30 day, constant-maturity instrument.

Example:

On Nov 4th, the investor would want exposure until Dec 4th (30 days); in order to
replicate VIX, she would need a portfolio composed at 46% of Nov futures and at 54%
of Dec futures. The weighted average of these percentages gives 30 days.
On Nov 5th, the percentages change respectively to 43% and 57%. This means that the
investor had to sell part of her November futures (cheaper), and buy some December
futures (more costly).

The investor has eﬀectively carried out a “rolling”; the cost of the rolling (i.e., selling
the ﬁrst future and buying the second) is called “carry”.
The trading idea is to have a short exposure to VIX, so that, instead of paying for the carry,
the investor will receive a daily carry in the case the curve is in contango.
The problem with being short on VIX is that, if the VIX had some peaks (for instance, going
from 20 to 25), the investor would receive a “margin call”, since the mark-to-market would become
largely negative.

However, volatility is mean-reverting, so the key idea behind the strategy is:
Open a short position on VIX when the volatility is high and is about to decrease;
Receive the carry that is generated from the rolling on a daily basis.
Close the short position as soon as the volatility increases, thus generating a positive P&L
from the position, and avoiding harmful margin calls in case the VIX rises to levels much
higher than the entry ones.
Reopen the short position and repeat everything.

## What to do in practice

 - Modelling the implicit volatility, using multi-factor statistical models.

 - In particular, trying to model/estimate the UX1 and UX2 indices, which represent the generic
tickers for the ﬁrst and second futures on the VIX.
 - Once a reliable volatility model has been developed, creating signals that tell us:
   - when to open a short position, and;
   - when to close it.

- In a second moment, we may also want to estimate the “conﬁdence” we have about opening
a position, that is, the overall percentage of the capital that we decide to invest/collect when
speciﬁc volatility conditions are present.
