---
title: Startup Equity Primer + SF Software Engineer Offer Data
tags: [misc, non-technical]
published: True
layout: post
---

There was a time where I thought I knew how startup equity works. 
I now realize I was wrong and in fact have never known how any of this shit works. 

<!--more-->

This posts consists of essentially two parts
- A very short and general explanation of stock options and some ways you will get fucked. 
- A small analysis of startup software engineering job offers in the San Francisco Bay Area. 

## Options, Vesting, Exiting and Taxes

There are two main types of stock options: ISOs and RSUs. 

If you get RSUs it means your startup is a "startup" in that it's still private, but it's probably fucking massive. (Think Uber, AirBNB, etc.)
So I'm not going to talk about them that much, since this is really focused on actual startups. 

An option allows you to buy a share of stock at your **strike price**, which is usually much lower than the actual share price. When you exercise the option you pay the strike price and get a share of stock back.

Companies do not give you all your options at once, they **vest** over a period of time. The most common scheme I've seen is vesting over 4 years monthly with a one year cliff. 

This means that you get 0 options until you hit one-year from your starting date, upon which you get $\frac{1}{4}$ of your promised options. After that you get another bit of options every month until the four years are up and you've received all your options. 

For some reason startups always frame offers as "$n$ shares vested four years". One of the first things you should figure out is exactly how many shares outstanding there are, so you can figure out what % of the company they are offering. 

One thing to keep in mind: Startup shares are worth zero until they're worth something. 

A share, like any stock is only worth what other people are willing to pay for it. There are some markets for early startup stock but by and large, these are a very illiquid asset - you usually only can cash out if the startup is acquired or IPOs.

If you ever leave the company, your options usually expire after 90 days. This means that you'll need to either buy them by then or lose them. 

If you exercise your options and then sell them again within the year, you'll be charged short term capital gains tax on your return, which is currently 37%.

Otherwise if it's longer than a year before you exercise and sell you'll be charged long term capital gains tax.  This is much lower - the highest bracket (it's progressive like income tax) is 20%.

The American tax code is so complicated that there's something else called the AMT. Basically you need to pay at least x% in taxes, so if you negotiated very well and made a bunch of money when you sell your stock then you may be taxed under the AMT rather than by the normal tax brackets. 
But again this only matters if you make a lot of money, and if that's the case you should really just get a CPA. 

## Dilution and Preferred vs. Common Stock

Dilution occurs when new stock is issued as part of financing. Basically since there's more stock, the relative value of your options decreases. Companies usually issue new stock when they get new funding. 

But in theory, companies grow more valuable over time, so when they raise capital the stock price should go up too, so you benefit as well. 

Let's consider a concrete example: 

Say STARTUP offers you 10000 shares out of a total of 1M shares. For simplicity we'll say the strike price is 1 dollar and the current valuation of STARTUP is 1M. 

This means you have 1% of STARTUP or a nominal 10000 dollars. 

Let's say STARTUP releases a new blockchain based iced tea and this peaks the interest of HardBank, a VC firm.

STARTUP finances this investment by issuing another 1M shares, which HardBank buys at 5 dollars a share.

Now your 1% has been diluted to 0.5% but the valuation of the company has went from 1M to 10M so your options have increased in paper value from 10000 to 50000. You are happy. 

Say later on in a year your company struggles a bit because blockchain is no longer hype and is acquired for 7M - You pull out your calculator and compute $0.005 \times 7M = 35000$.
This means you're getting 35000 dollars right?

No, the thing is you're not getting nearly that much. 

See cash equity (what investors get) is usually preferred stock. What this means is that in the event of an exit, cash equity investors get paid back first, and you split what's left. 

Essentially they get the option to either have their investment equity or their investment back (or some multiple). 

So in this scenario HardBank has the option to either get his investment back (5M) or his share value which is 50% of 7M or 3.5M. 

So after HardBank's team of quants does the math and takes the 5M, there's only 2M left, which is split among the plebs who have common stock. 

So you get 0.005 * 2M or 20000. But you need to exercise your options to for a dollar a piece, so you really only "get" 10000 dollars.

And you have to pay short term capital gains tax on that so you walk away with ~6k. 

This is a bit of a contrived example, but hopefully it shows why down rounds are so bad. The only time when you get the full acquisition value * equity percentage is if all the VC's choose to get their shares instead of their investment (or investment * multiple) back. 

This can only happen if your company continues to grow bigger and bigger. If STARTUP is acquired for 100M instead of 7M then you do walk away with 500k pre-tax. 

In the case of ASDF you're not factoring in the following things:
- money needed to exercise your options
- perferred stock payout scheme
- short term capital gains tax

I should say this is far from a comprehensive review of all the weird financial shit, but hopefully you can see some of the money that is lost when valuing stock options. 

You can read more in depth here:
- ASDF
- ASDF
- ASDF

## AngelList Data 
OKay so what % of a company is good? The only place that puts cold hard numbers to this is Nivi's blog (who founded angellist. )

I also found this analysis from 2014 which is pretty good. 

But I wanted to see for myself, so I wanted to do a similar analysis. Unfortunately AngelList has since deprecated their API so instead I manually scrolled through ~370 job postings and copied over the company name, job title, company size, and salary and equity ranges. 

This was very painful and took me like 3 hours. 





