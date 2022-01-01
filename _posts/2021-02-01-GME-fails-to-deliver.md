---
layout: post
title: Analyzing GME fails-to-deliver to look for illegal naked short selling
categories: [GME, short selling]
excerpt: Have hedge funds been illegally shorting GME?
---

Like a lot of people I read [this interesting comment](https://www.reddit.com/r/wallstreetbets/comments/l9jbc5/listen_to_me_we_cannot_trust_the_short_interest/glib7cs/?context=3) on Reddit. I stayed up until 2am watching all the videos and it got me pretty worked up.

If you haven't seen the videos, a brief summary follows. (I am not _at all_ a finance person and am just repeating my understanding, so I might be completely or subtly wrong here.) When you short a stock, you do the following:

1. Borrow a stock from someone (for the cost of ongoing interest);
2. Sell it to someone else (at full cost);
3. Wait for the share price to go down, then buy it back and return it to the person in #1;
4. Congrats, the difference is your profit.

The system contains a little bit of wiggle room in step 2, where you sell the borrowed share. The money the buyer gives you settles immediately, but you have a few days to deliver the share. This is reasonable, because the share might be in a bank vault or the back of a closet or something. To keep things running smoothly, in this situation you write a notional "IOU 1 real stock", and _that_ goes to the buyer's broker. This IOU is treated "as good as" one stock, because the expectation is you'll make good on the IOU. Once you've yanked the share out of the closet, you swap it for the IOU and all is well. You have two days after the trade date to do this.

If you are a Bad Hedge Fund willing to do some crimes, you can take advantage of this by simply writing the IOUs without ever having found and borrowed the stock from someone. You can write as many IOUs as you like, and since everyone assumes they're equivalent to a real share, you're essentially inflating the supply of shares beyond those that actually exist. This is extremely illegal and is called naked short selling (because you have no real stock to "cover" yourself with). It is also sometimes called counterfeiting.

If you take longer than two days after the trade date to settle the IOU, this is known as a "failure to deliver". Twice a month, the SEC publishes the number of outstanding fails-to-deliver shares for any security with more than 10,000 outstanding fails-to-deliver shares.

The theory behind the videos linked above is that a campaign of aggressive naked shorting led to the collapse of Lehman Brothers in 2008. The video shows a significant and sustained uptick in fails-to-deliver shares of Lehman right around the time the price tanked, which is what you would expect to see if a shady hedge fund took a short position and decided to counterfeit shares to artificially depress the price. If you like 90 minute PowerPoint lectures with dense financial data the videos are a good watch and have more information; I have summarized what's relevant for this post.

Since there have been some rumors swirling that GME short sellers were doing so illegally as part of a campaign to manipulate the price, I decided to pull the FTD data from SEC and take a look at GME. Below is a graph which shows the aggregate number of FTD shares over time, and also the price of GME. As of today, the SEC has released data up to January 14th and no further.

Analysis is further complicated by the fact that the SEC reports only the _total_ number of FTD shares; if a day's FTD shares count is 1000, you can't tell the difference between "all 1000 shares were outstanding yesterday too" and "all of yesterday's FTD shares were settled and these are fresh new ones".

That said, here's the graph:

<div align="center">
<img src="https://imgur.com/9gPkSfO.png">
<p><small>A graph of the SEC's FTD reports for GME vs price.</small></p>
</div>

I will not analyze this for you but I will leave some comments and observations.

* It does not appear that there was a severe and sustained uptick on outstanding FTD shares. Even where there are periods of sustained volume, they do clear back to zero; that is to say, the shares seem to eventually be delivered.

* The mere _existence_ of FTD shares is not evidence of any illegal activity. As mentioned before, some degree of FTD is a normal part of yanking-shares-out-of-the-closet and is fine.

There is one more wrinkle here. My understanding is that the SEC only reports FTDs for trades that were settled in the National Securities Clearing Corporation (aka NSCC). This is _most_ but not _all_ trades, as it's possible for brokers to settle trades between themselves directly; this trades are said to settle "ex-clearing". I have no idea how many trades are settled ex-clearing and have no FTD data on them. One could make the argument that there are four quadrillion counterfeit shares that have entered the system through ex-clearing, or one could make the argument that there are zero, and there would be no way of telling either.
