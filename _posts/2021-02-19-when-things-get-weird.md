---
layout: post
title: When Things Get Weird
categories: [market makers, delta hedging, gamma hedging, vanna, charm]
excerpt: A deeper look into market maker hedging can move the market.
---

Squeezemetrics paper

H TODOs:

- read https://nope-its-lily.medium.com/a-story-of-liquidity-volatility-and-returns-754e0019c2d0
- read https://sixfigureinvesting.com/2019/02/what-caused-the-february-5th-2018-volatility-spike-xiv-termination/
- read https://spotgamma.com/all-you-ever-wanted-to-know-about-gamma/
- read https://spotgamma.com/options-gamma-trap/ - likely integrate

This post is a deeper look into the world of market making, and what happens when things get weird. If you've heard terms like _gamma hedging_ or _gamma squeeze_, or seen people mention the more mysterious Greeks like vanna and charm, this post is for you. I will cover:

- fill
- this
- list
- in

## Prerequisites

This is not really beginner stuff any more. I'm not going to start drawing formulas at you, but you'll find this one much harder to get by without a grounding in the basics.

**TODO: fix the below link**

You should understand everything in [this post](understanding-NOPE) (what, you think I'm above shilling my own work?), plus:

- A strong understanding of these options greeks: delta, theta, vega


---

topics:

- gamma hedging
- gamma squeeze
- what is vanna, and how can it mess things up
- what is charm

# Delta, theta, and vega: not enough on their own

In the last post we walked through how a market maker hedges their risk were you to buy SPY calls from them [TODO: link]. At the beginning of the example, the delta on those calls was 0.52, but then the underlying rise and the option went further in-the-money:

> And because the option is further in-the-money now, **delta is a little bigger**, say 0.54. So if the spot price of SPY moves up $1, the total price adjustment for the calls you sent me would be $54.

I glossed over something which I've bolded here: why did delta change? To understand that, we need to think a bit more about what delta really represents.

This is the payoff graph for a call option, something you've doubtless seen before:

<p align="center">
<img src="../images/delta-call-option.png">
<small>A payoff graph for a call option. [<a href="https://www.tradingcampus.in/option-greeks/">source</a>]</small>
</p>

The maroon curve is the one we're interested in for now, showing the value of the option for various spot prices some time before expiry. The green line is the tangent to that curve at the current spot price, and **the slope of that line is the option's delta**.

You can think about it from the other perspective, if you like: if you plotted the definiton of delta ("the change in option value for a $1 change in spot price"), you'd get a `y = mx + c` straight line where `m = delta`.

So: we have a curved option payoff line, and a tangent line delta. As with all tangent lines, delta is only a useful predictor for a change in options value for small changes in the underlying spot price: for larger changes, the curve moves away from the tangent line. That's why in our example, delta got adjusted: the spot price moved, so the slope of the tangent line increased.

Can we quantify this adjustment? Yes. It's a higher-order Greek called **gamma**.

## Gamma

Gamma describes the _curve_ of the options payoff line. It is defined as **the change in delta for every $1 the spot price moves**.

You'll notice this is different to the Greeks you know so far. Delta, theta, and vega all describe the change in the option price; gamma describes the change in _delta_. We say Gamma is a **second-order** Greek.

### Gamma hedging

Let's think back to our market maker from the example. They were delta hedging, and having to do so repeatedly. Since gamma explains why they had to update their delta hedge, can our MM _gamma hedge_ and save ourselves the trouble? Well... yes and no.

This time I'll let someone else explain it (you only need to watch as far as 9m35s):

<p align="center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/GCAM8UyCitE" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</p>

So yes, gamma hedging is possible: ultimately it's just a system of simultaneous equations like you learned in high school. Notice that the option used for gamma hedging also provides some amount of delta hedge, reducing the number of shares you need to buy (compared to a delta-only hedge). Of course, you've now bought some options, so the person who sold you them will have to hedge the delta on _those_ options - so the same number of shares have to be bought anyway, you're just pushing around who has to do it.

But if you're a MM and already in the business of writing options, why would you go buy an option from a different MM? You could write it yourself for cheaper, but then you'd need to hedge _that_ option! In doing so you'd buy all the shares you were trying to avoid in the first place. Seems pointless.

### Gamma squeeze




## Other higher-order Greeks

https://en.wikipedia.org/wiki/Greeks_(finance)

---

# References

SqueezeMetrics has written some great papers on this kind of behaviour:

- [Gamma Exposure: Quantifying hedge rebalancing in SPX options](https://squeezemetrics.com/download/white_paper.pdf)
- [The Implied Order Book](https://squeezemetrics.com/download/The_Implied_Order_Book.pdf)

KeyPaganRush on YouTube has some good videos:

- [Gamma and Vanna Exposures](https://www.youtube.com/watch?v=zfkOCc2evEk)
- [Vanna and Charm Exposure](https://www.youtube.com/watch?v=-RhSCoElB9Y)

Other stuff:

- [Advanced Options Greeks](https://www.youtube.com/watch?v=ngweIHiKOUg) - good visualisations of how higher greeks 



