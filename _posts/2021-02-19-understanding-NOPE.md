---
layout: post
title: Understanding NOPE
categories: [NOPE, market makers, delta hedging]
excerpt: An attempt to give you a working understanding of what NOPE is and the theory behind it. Requires some options knowledge.
---

This post aims to provide a working understanding of what NOPE is and the theory behind it. I will cover:

- Who is on the other side of your option trades
- How they avoid being stuck holding the opposite position to yours
- What this does to the market
- What NOPE is measuring and how it does it

## Prerequisites

This post is aimed at someone with a beginner-to-intermediate understanding of options trading. Specifically, I assume you already know:

- What options are
- What the order book is
- A basic understanding of the options greeks
- That the Black-Scholes formula exists and that it's used to price options using the greeks. (But don't need to know it or be able to derive it!)

If you don't know these things, the good news is that there are **many** resources online that can teach you, right on the other side of your favourite search engine :)

I also assume you know who [Lily](https://twitter.com/nope_its_lily) is and have heard about this NOPE thing. Otherwise how did you even get here?

With that out of the way: let's get started.

# What happens when I make a trade?

It is natural to assume that when you make a trade, you are making a bet with someone else who wants to take the opposite position. For instance, if you're buying a call, you're buying it from someone who thinks you're wrong, and thinks the security will go down.

This is, in general, completely incorrect.

The *vast* majority of the time you are actually interacting with a **market maker**. Per [Investopedia](https://www.investopedia.com/terms/m/marketmaker.asp):

> A market maker (MM) is a firm or individual who actively quotes two-sided markets in a security, providing bids and offers (known as asks) along with the market size of each. 

MMs place _both_ bids and asks on the order book. Their goal is to make money from the difference between those two prices (known as the **spread**) - buy from people who want to sell _now_, sell to people who want to buy _now_, keep the change. That "keep the change" _is_ the business model of a market maker - they have no interest in holding either long or short positions, so they need to get rid of any positions (or otherwise neutralize them) ASAP. Holding a position introduces a _ton_ of risk that they don't want.

In the case of trading stocks, there is typically enough volume that the amount of time they're actually holding a security - between buying it from you and flogging it to someone else - is small, so the whole thing works nicely. Impatient people get to make their trades, the spread is narrowed, they pocket some change - everybody wins. This function is so important that exchanges approve designated market makers who are _contractually obliged_ to place both bid and ask orders, in return for a commission fee.

However.

The "buy it and flog it to someone else quickly" approach does not work for options. The demand is just not there. Think of an options chain: a zillion dates, and a zillion strikes for each date, multiplied by two (for puts and calls). The daily volume for the most popular single option is less than the _minutely_ volume for its underlying!

A market maker cannot take the other side of your trade and flog it to someone else if there's no demand for it. But they still have that pesky contractual obligation to take the trade. So they have to do something else, because being forced to take positions (that you didn't even choose!) is, um, terrible for your portfolio and not anywhere in your business model! That something else is called **hedging**, and it's the core behaviour that NOPE watches.

# How do MMs avoid being stuck with the other side of my option trades?

Let's say you are a market maker, and you just sold me 100 at-the-money SPY calls. You did this because you were _contractually obliged to_ - you had no choice in the matter. At the moment I buy the calls from you, this is fine, because the options were priced fairly. By fairly we mean "we both value the calls to be worth the money I paid", i.e. the cost of the premium I paid perfectly matches the risk that you're taking. You could - assuming you found a buyer - pay someone the money I just gave you to take on your obligation, and they would choose the same price.

But of course the value of one SPY share (aka its **spot** price) is going to change, which means the value of the option is going to change. You'll remember the value of an option changes by **delta** for every $1 the underlying moves deeper in-the-money.

In our example we said that the calls were at-the-money, so let's say their delta is 0.52. This means that if the price of SPY goes up by $1, the price of the option will increase by $0.52. Multiply that by the 100 calls I bought, and in order to get rid of them you would now have to pay someone an extra $52, plus the money I originally paid you. Thus your portfolio is effectively worth $52 less.

As a market maker, you want the total value of the securities you're holding to be "net zero" at all times, because guessing which way the market is going isn't your business model. So you need to offset this risk in such a way that if the price of SPY goes up $1, your portfolio somehow gains the $52 in value that it'll lose from the calls you sold me.

Well, that's easy: buy 52 SPY shares!

That way, if the price moves up $1, your 52 shares gain a dollar each, exactly offsetting the loss in value of the options you sold me. If the price moves down $1, the opposite happens, and you're still good. If the price doesn't move: even better! Perfect!

This is it. This is the key insight; if you can wrap your head around this, everything else will follow. It's so important I'm going to write it again in bold.

**Important Thing #1: MMs respond to option sales by taking positions in the underlying.**

They do this as part of their effort to "net zero" their portfolio and insulate themselves from the risk of price movements. Doing things to protect yourself from risk is called **hedging**.

### Um, the price isn't going to move just once, though...

Right. You hedged the initial options, the underlying (spot) price moved, your hedge worked, but now you're back to where you started - exposed to the risk of the price moving _again_. And because the option is further in-the-money now, delta is a little bigger, say 0.54. So if the spot price of SPY moves up $1, the total price adjustment for the calls you sent me would be $54.

Thankfully you already have the 52 shares, so if the price of SPY goes up $1, they'll earn $52. So you'd only be $2 short of the $54 you'd lose, and can solve that by buying two more shares.

This corresponds to the 0.02 difference in delta between your last position and your current one. By buying those two extra shares, your delta is now back to "even" - we call this being **delta neutral**. Not surprisingly, the process of hedging your delta risk is called **delta hedging**.

And so you keep adjusting as delta fluctuates. If the spot price goes back down a dollar, you sell off those two shares. You do this _repeatedly_, as the spot price moves, in order to maintain your delta neutral position.

**Important Thing #2: MMs holding option positions make predictable trades in the underlying as spot price changes.**

This is interesting: by trading options, you force MMs to take certain positions in the underlying _and_ force them to keep adjusting those positions in predictable ways until option expiry. How often do they re-hedge? We don't know. But it's a good question, and we'll come back to it in a bit. For now, let's take a step back, because we've learned a lot already.

# What does this mean for the market?

Let's write this all out explicitly:

* Buying a call from a MM forces them to buy "delta" number of shares, and buy more as price goes up.
* Selling a call to a MM forces them to sell (short) delta shares, and sell more as price goes up.
* Buying a put from a MM forces them to sell (short) delta shares, and sell more as price goes down.
* Selling a put to a MM forces them to buy delta shares, and buy more as price goes down.

The easy way to think about this is that in order to neutralize the _other_ side of your option position, the MM replicates _your_ option position, just using shares. If you profit if the price increases, they buy; if you profit if the price decreases, they sell.

There's something important hiding in this list. Look at the impact of the MM hedges in each situation. In some cases, the MM hedge moves _against_ the price action, providing a stabilizing effect. In other cases, the MM hedge moves _with_ the price action. Notice anything?

**Important Thing #3: Selling options to MMs stabilizes price action. Buying options from MMs amplifies price action.**

Hmm. So that means the aggregate balance between option buying and option selling is important. If they're in balance, the market moves normally. If option selling dominates, price movements are is dampened, unable to move as much as they might otherwise. And if option buying dominates...

### Uh oh.

Yeah. If more people are buying options than selling them, then MM hedging _amplifies price moves_.

So, is this happening? Can we tell?

The open interest for an option is the number of option contracts outstanding at each price. But that doesn't tell you whether the non-MM side of the transaction bought or sold. For that you have to watch the trades as they happen - or just buy the historical data.

Of course, someone did that, so we know the answer:

**Important Thing #4: Option buying outweighs option selling.**

Which makes total sense from the perspective of any individual: buying options has limited downside risk, selling them has unlimited risk. Easy choice.

In Lily-speak, we are now in an options degenerate market ([part 1](https://medium.com/swlh/options-degenerate-marketplaces-part-1-b0ddf1c96fa6), [part 2](https://medium.com/the-shadow/options-degenerate-marketplaces-part-2-57c9816c5977)). That is to say, a significant factor in price action is not how people value the asset but the forced moves that MMs make to hedge option trades.

# What NOPE is measuring and how it does it

Let's recap: we know option buying outweighs option selling, which means that price movements are amplified by MM hedging.

Hold on though. A while back we asked how often MMs hedge price movements, and we said we didn't know. But we _can_ watch the option trades as they happen. So for each one, we can calculate the required hedge in the underlying, and then expect to see that trade come through. If we watch all option trades on all strikes on all expirations, we can build up a picture of "how much the price has been shifted because of MM hedging".

This is what NOPE is doing. In brief, the NOPE calculation is:

1. Add up the deltas of all calls across all strikes across all expirations, weighted by volume; call it `all-call-deltas`
2. Add up the deltas of all puts across all strikes across expirations, weighted by volume; call it `all-put-deltas`
3. `NOPE = (all-call-deltas - all-put-deltas) / total-shares-traded`

A rising NOPE implies more calls have been sold and thus that the price has been artificially bid up past its usual by MM hedging. (Option buying increases the numerator, MM share buying increases the denominator by the same absolute amount.) The price may have been pushed out of balance - away from its "correct" level, if you like - and could move back down as people sell the underlying shares for a profit. Those share sales increase the denominator without affecting the numerator, and NOPE reverts.

You'll notice this isn't perfect. In particular, it's not paying any attention to the ratio of buys to sells for each option; the calculation assumes that _all_ options traded are buys and none are sells. Given that we know that there are _more_ buys than sells this means that NOPE will give us some signal but it won't have the fidelity it could if you kept up with every single trade.

It's also a unitless indicator. This means it's not measuring an amount of anything: it's just a floaty number. The number varies wildly for each security (though it is always centred around zero) and it takes some experience to learn each security's normal range. There was a brief attempt to build an indicator that normalized the values to a moving average standard deviation, called NOPE_MAD, but it got shelved in favour of other projects.

**Important Thing #5: Lily already knows NOPE isn't perfect. You don't need to tell her.**

Lily has had people approach her making this point (and other criticisms of NOPE) countless, innumerable times. She has explicitly said "the nope calculation is shit". She recognizes it is a very handwavey approximation. If you want to write a better implementation, go for it! Just... don't bug her about it, okay? Just... be cool.

### That's it. You did it!

Congratulations - you made it! We've covered a lot: the existence of market makers, how they hedge the risk of options trades, what this means for the market, and how NOPE watches options to guess at upcoming MM hedges. You now have a decent understanding of how NOPE works!

In the next post, we'll take a deeper look into how the behaviour of market makers can make things worse when markets go screwy. Stay tuned!

# References

Sources I used to wrap my head around this:

- [Lily's NOPE paper](https://www.scribd.com/document/487296659/)
- [Bionic Turtle on MM delta hedging](https://www.youtube.com/watch?v=N7kOPxvRbRI)

Want to trade NOPE?

- [NOPEChart](https://nopechart.com/) - buy the premium version, it's worth the money
- [Hole Corporation video on how to use NOPE](https://youtu.be/E8CvDfZ6Aaw) 

More advanced reading on NOPE:

- [The NOPE brain worm](https://thisismy.substack.com/p/the-nope-brain-worm): some further notes on NOPE
- [Lily's writing](https://nope-its-lily.medium.com/) - in particular I'd recommend:
    - [Let's Talk About The NOPE](https://nope-its-lily.medium.com/lets-talk-about-the-nope-9426d6cf350a)
    - [Interpreting the NOPE: A Brief User's Guide](https://nope-its-lily.medium.com/interpreting-the-nope-a-brief-users-guide-41c57c1b47a0)
    - Options Degenerate Marketplaces: [part 1](https://medium.com/swlh/options-degenerate-marketplaces-part-1-b0ddf1c96fa6), [part 2](https://medium.com/the-shadow/options-degenerate-marketplaces-part-2-57c9816c5977)
    - No gods, no kings, only NOPE: [part 1](https://www.reddit.com/r/thecorporation/comments/jck2q6/no_gods_no_kings_only_nope_or_divining_the_future/) (on reddit), [part 2](https://www.reddit.com/r/thecorporation/comments/jd2q2u/no_gods_no_kings_only_nope_or_divining_the_future/) (on reddit), [part 3](https://nope-its-lily.medium.com/hello-friends-451d71104111) (on Medium)

# Thanks

Proofreaders:
- @specs
- @Toven
- @sean,
- @Sophos
- @invaderkay
- @Goat 
