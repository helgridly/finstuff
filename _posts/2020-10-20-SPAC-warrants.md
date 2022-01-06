---
layout: post
title: Trading SPAC warrants
categories: [SPAC, warrants]
excerpt: Is there money to be made trading SPAC warrants?
---

## Introduction

A warrant is a kind of security that's redeemable for a share at a certain exercise price, usually $11.50. So a holder of a warrant can opt to pay $11.50 and get a share, regardless of the price of the share. The intrinsic value of a warrant is thus `share_price - 11.50`, though its traded price can drift from that (in either direction) for various reasons.

Warrants are often issued by SPACs. SPACs are "special purpose acquisition companies" - companies that list on the stock exchange for the express purpose of acquiring another company. It's a way for the acquired company to get listed without having to go through an IPO process - they just merge with a SPAC who finds them appealing.

A warrant is thus a cheap incentive to get people invested in SPACs. If the SPAC fails to merge with a target company within the time allotted (usually two years), the warrants are worthless. The warrants are also worthless if, post-merger, the acquired company is trading below $11.50. (A holder of a warrant usually has to wait until 30 days after the merger has been completed to exercise their warrant.)

Despite the risk of their value going to zero, warrants are appealing to trade because:
1. They're cheap - single-digit dollars, often under $1 while a SPAC is still searching for a target.
2. Small relative changes in the share price make for large relative changes in the warrant price. This allows for the possibility of large multiples of gains (a recent SPAC's warrants briefly traded at $13.50).

The aim of this project is to determine if and how warrants should be traded over the life of a SPAC.

## What does the warrant price look like over the lifecycle of a SPAC?

The "golden path" lifecycle of a SPAC is:

1. **Searching for target**. Low volume and not much price movement because there's no information. If the managers of the SPAC are well known there may be hype based on that alone.
2. **Letter of Intent (LOI)**. They publish a formal letter to some private company saying "we'd like to acquire you. Are you interested?". This is the first news the public gets of any discussion. There is usually a bump around here, since this means probable progress.
3. **Definitive Agreement (DA)**. The target company says yes, we'd like to merge with you and become a publically traded company. Another bump.
4. **Merger vote**. The shareholders of the SPAC vote on whether to go ahead with the merger. They will not do this if they think the target company will trade under $10, as the SPAC will refund $10 of share value to any shareholder if the SPAC doesn't acquire a company within the allotted two years. Recent SPACs have seen a large _sell-off_ at this point, as long-time investors take their profits.
5. **Merger executed**. The companies officially merge: the SPAC ticker changes to the target company's ticker, and all shares and warrants are converted to the new ticker. The SPAC is now a publically traded company just like any other.
6. **Warrants exercisable**. 30 days after the merger, warrants can be exercised. By this time both the warrant and share prices will have adjusted such that `warrant_price = share_price - 11.50`.


So, to recap, the price action is:
* Flat and very low volume while the SPAC is searching.
* A bump at LOI, followed by a tail-off as people lose interest.
* Another bump at DA when the deal is done.
* Get the hell out before the long-term investors take their profits.

Some examples of SPAC price graphs follow. The first three are small, the others are larger.

* [GreenVision Acquisition](https://warrants.tech/details/GRNV)
* [Andina Acquisition](https://warrants.tech/details/ANDA)
* [Alberton Acquisition](https://warrants.tech/details/ALAC)
* [Forum Merger II](https://warrants.tech/details/FMCI)
* [Flying Eagle](https://warrants.tech/details/FEAC)
* [Spartan Energy](https://warrants.tech/details/SPAQ)

As you can see, the LOI and Definitive Agreement both show clear bumps but their respective sizes vary.

Outlier cases:
* Multiple LOIs, if one of the potential companies isn't interested.
* The merger vote fails.
* The SPAC fails to find a target company within the allotted two years.

The idea of this project is to pick up warrants during the searching phase, hold them until LOI and/or DA, and then sell.

## Research

I pulled the list of all SPACs from the SEC for the years 2015-2020 and found all their warrants. I will skip the part where gathering and cleaning this data was a nightmare 🙂 The dataset contains all SPACs that IPO'd during that period and subsequently issued warrants, regardless of whether they successfully merged with a target. This should (hopefully) eliminate survivorship bias.

### Results

There were 214 warrants that met the criteria. Our dataset is biased towards smaller SPACs where warrants traded well below $1 for long periods of time. (74% of the SPACs in our dataset never had warrants trading above $3, whereas we now see some big name SPACs trading $3 warrants _at launch_.)

I selected their highest traded price and picked the lowest price before that date to calculate a maximum return.

|                    | min   | median | mean   | max     |
|--------------------|-------|--------|--------|---------|
| low price          | $0.01 | $0.45  | $0.61  | $5.20   |
| high price         | $0.08 | $1.73  | $2.58  | $17.50  |
| max return (hi/lo) | 1x | **3.58x** | **12.26x** | 383.51x |

If you attempted to buy every SPAC at $1.50, the best price to sell at would be $6.83. This would net you a return of 1.24x. However, this would be extremely stupid, as a lot of these warrants trade _well_ below $1.50 for long periods. Of the 120 SPACs (56% of our dataset) that failed to hit $2, their median max return was 2.86x, with a mean of 7.36x. It'd be extremely obvious that you were overpaying at $1.50.

Catching the high and low prices accurately is hard. If you bought at **triple** the low price and sold for a **third** of the high price, your median return would be 0.4x  - a 60% loss! - but your mean return would still be 1.36x. You shouldn't use this strategy, but if you did, you'd likely see a string of losses before one SPAC took off and saved your account. Not for the faint of heart!

Here's a chart of some potential entry prices, and some limit order numbers that X% of SPACs that ever reached that price subsequently hit. Note that "best sell price" means "you sell all your warrants if they hit this price", thus you never get the upside. At low entry prices it is heavily skewed by the few SPACs that took off (better to sell high and maximize returns from outliers than get more reliable returns across the board). If you want more reliable returns, use the percentiles for guidance.

|   entry price |   95% SPACs reach |   80% |   50% |   25% |   best sell<br/>price |   return at best<br/>sell price |
|--------------:|------------------:|------:|------:|------:|----------------------:|--------------------------------:|
|          0.10 |              0.28 |  0.39 |  0.74 |  1.11 |                  5.10 |                            4.64 |
|          0.25 |              0.29 |  0.50 |  0.95 |  2.10 |     14.20<sup>†</sup> |                            3.50 |
|          0.50 |              0.31 |  0.64 |  1.29 |  2.84 |                  2.80 |                            1.45 |
|          1.00 |              0.38 |  0.76 |  1.38 |  2.56 |                  1.90 |                            0.73 |
|          1.50 |              0.40 |  0.85 |  1.53 |  2.90 |                  1.90 |                            0.55 |
|          2.00 |              0.41 |  0.89 |  1.65 |  2.99 |                  2.00 |                            0.42 |

<sup>† Beware: this price is likely an artifact of one SPAC that reached 0.25 and then took off.</sup>

Unsurprisingly, bigger gains come from lower initial prices. By the time you're willing to buy warrants above \$0.50, there's someone willing to sell you a warrant worth \$0.10, which drags your return ratio downward. If you want to make sure that you at least break even (on average), your highest entry price should be somewhere between \$.5 and \$1.

If you can mystically refrain from buying a warrant until it reaches the bin where it won't go any lower (e.g. you hold off buying a warrant at $0.25 because you _somehow_ know it will later go below $0.10), your odds get better:

|   entry price |   95% SPACs reach |   80% |   50% |   25% |   best sell<br/>price |   return at best<br/>sell price |
|--------------:|------------------:|------:|------:|------:|----------------------:|--------------------------------:|
|          0.10 |              0.28 |  0.39 |  0.74 |  1.11 |                  5.10 |                            4.64 |
|          0.25 |              0.31 |  0.63 |  0.99 |  2.94 |                 14.20 |                            5.28 |
|          0.50 |              0.58 |  0.93 |  1.55 |  3.01 |                  6.80 |                            1.87 |
|          1.00 |              0.74 |  1.01 |  1.66 |  2.50 |                  1.70 |                            0.85 |
|          1.50 |              1.50 |  1.71 |  2.85 |  3.25 |                  2.65 |                            1.06 |
|          2.00 |              1.76 |  2.24 |  2.52 |  3.21 |                  2.30 |                            0.92 |

You can probably get closer to this kind of return by continuing to invest in a warrant as its share price drops; more below.

Here's a graph of low prices (x-axis) against ROI (y-axis). Note the log-scale on the Y. The truly eye-watering returns come when the low prices are VERY low.

```
  1000 +--------------------------------------------------------------------+
       |                                                                    |
       |                                                                    |
       |                                                                    |
       |                                                                    |
       |                                                                    |
       |.                                                                   |
   100 |..                                                                  |
       |  .                                                                 |
       | ..                                                                 |
       |. .                                                                 |
       | ..                                                                 |
       |. . ..                                                              |
       |...  .                                                              |
    10 |......  .                                                           |
       |.......                                                             |
       | ... .. .    . .                                                    |
       |........ . ..   .                                                   |
       | ...... ..  ... .            .                                      |
       | ...........  ...    . .                                   .        |
       | .  ........... ..   .      .     .                                 |
     1 +--------------------------------------------------------------------+
       0           1          2           3          4           5          6
```

Some 52-week lo/hi values and ratios for famous recent SPACs:

* HYLN: 0.15 - 29, 193x(!)
* VLDR: 0.05 - 7.90, 158x
* SPAQ: 0.27 - 7.6, 28x
* IPOB: 2.6 - 6.95, 2.7x

### Conclusion

**Assuming these patterns hold,** - i.e. assuming that past performance predicts future performance:

The main takeaway is fairly obvious in hindsight: the cheaper you can pick up a warrant, the more return you'll make on it. Chasing high-value SPACs will only limit your profit.

The real money is in buying cheap warrants. If you can pick up a warrant at 0.5, you'll make around 145% in the long run. If you can pick one up for 0.1, you can get 464% returns.

#### **I'm greedy. Can we do better?**

Yes.

First: you can dollar cost average into your positions. As the price of a warrant drops, the return ratio increases, but the probability it will cover its earlier cost decreases. You will do better spending $100 on a warrant at 0.5 and another $100 at 0.1 than simply spending the $200 at 0.5. This will increase the likelihood that you'll make back enough money to cover your (expensive in hindsight) purchase at 0.5.

Second: Don't just set a limit order to sell everything. Manage your trades!
* If your limit order triggers, the price is likely to keep going up. So cash out _some_, and then have fun trying to call the top.
* Alternatively, trigger an alert at your limit price, and then set up your sell order as a trailing stop. Now you can ride the price action much closer to the top.
* Similarly: pay attention to the SPAC's lifecycle and be prepared to cut your losses if the limit order doesn't hit. The calculations here assume that if you don't hit your limit price, you lose 100% of your initial investment. This isn't going to be true in practice; the price of your warrants will peak when the SPAC signs Definitive Agreement, so if you've passed that phase and your limit order hasn't been hit, get out with whatever profits you can.

# Epilogue

This was written in late 2020, using data going back to 2018. Around this time, the SPAC craze really kicked off, and [_everyone_ launched a SPAC](https://www.nytimes.com/2021/02/27/style/SPACS-celebrity-craze.html) (including Shaq! Shaq SPAC!). The market massively oversaturated; very few SPACs found a target, and of those that did, [few traded above the $11.50 required for warrants to be profitable](https://spactrack.io/closedspacs/). I would be very surprised if the results still hold!
