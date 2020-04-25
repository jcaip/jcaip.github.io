---
title: Startup Equity Primer
tags: [startups]
published: True
layout: post
---

This is a short explanation of early employee stock options. 


<!--more-->

## Options and Vesting
There are two many different types of stock options, the two most common are: ISOs and RSUs. 

Usually if you get RSUs it means your startup is late stage (think Uber, Airbnb), most early-stage employees receive ISOs. 

These options allow you to buy a share of stock at your **strike price**, which is usually much lower than the actual share price. It's basically a contract allowing you to purchase stock at a reduced price. 

The equity in offers is always structured as "$n$ shares", versus % of the company. So one of the first things you when evaluating an offer is try to figure out what % they are granting you. 

Companies do not give you all your options at once, they **vest** over a period of time. The most common scheme is vesting over 4 years with a one year cliff. 

This means that you get no options until you hit one-year from your starting date, upon which you get $\frac{12}{48}$ of your promised options. After that you get $\frac{1}{48}$ of your options every month until you've received all your options. 

## Dilution and Preferred vs. Common Stock 

Companies raise money by issuing stock, which VCs then buy.  Dilution occurs when new stock is issued as part of financing, which increase the overall total number of shares and reduces your relative %. 

However, ideally each subsequent round also raises the company valuation, so you still stand to make money. 

From what I can tell you can expect 10-15% dilution from each round to the next. 

Companies usually differentiate between sweat equity and cash equity - 
Employees get **common stock** and investors get **preferred stock**.


Preferred stockholders get the option to either have their investment equity or their investment back, before the common stockholders in the event of an exit. I'll go over an example later demonstrating this. 

Some VC funding will also come with a multiple, which allows a sort of gaurenteed ROI for the VC. 

## Exiting and Taxes
One thing to keep in mind: A share, like any stock is only worth what other people are willing to pay for it.

There are some markets for early startup stock but by and large, these are a very illiquid asset - you usually can only cash out if the startup is acquired or IPOs.

If you ever leave the company, your options usually expire after 90 days. This means that you'll need to either exercise them or give them up. 

If you buy your options and then sell them within one year, you'll be charged short term capital gains tax on any return, which is currently 37%.

Otherwise if it's longer than a year before when you buy and sell you'll be charged long term capital gains tax, which is much lower - the highest bracket is 20%.

The American tax code is so complicated that there's something else called the Alternative Minimum Tax. Basically you need to pay at least $n\% $ in taxes, so if you make a bunch of money when you sell your stock then you may be taxed under the AMT rather than by the normal tax brackets. 

# Real World Example
Let's consider a concrete example: Say Sally Startup offers you 10000 shares out of a total of 1M shares.

For simplicity we'll say the strike price is $1 and is equal to the current share price, making the current valuation of Sally Startup 1M. 

This means you have 1% of Sally Startup or nominally $10000. 

Sally Startup then releases a new blockchain based iced tea and this peaks the interest of HardBank, a VC firm who decides to invest.

Sally Startup finances this investment (Series A) by issuing an additional 1M shares of preferred stock, which HardBank buys at $5 a share and a 1x multiple.

HardBank has essential invested 5M at a 5M pre/10M post valuation for 50% of Sally Startup. 

When Sally Startup financed that HardBank investment, your 1% was diluted to 0.5%, as you own 10000 shares out of a new total of 2M.  

However, as the valuation of the company has went from 1M to 10M your options have increased in value from \\$10000 to \\$50000.

#### *Scenario A*

Say later on in a year your company struggles a bit because blockchain is no longer hype and is acquired for 7M.

You pull out your calculator and compute (0.005) 7M = $35000, which is the amount of money you expect to receive. 

However, when Sally Startup sells for 7M this represent a "down round" or a decrease in valuation.

So in this scenario HardBank has the option to either get his investment back (5M) or his share value which is 50% of 7M or 3.5M. This is because HardBank has preferred stock. 

So after HardBank's team of quants does the math and takes the 5M, there's only 2M left, which is split among the plebs who have common stock. 

This is also where the multiple comes in - if the multiple was say 7/5x, then HardBank could take the entire 7M and leave nothing. 

So after the preferred investors recover their investment, you get (0.01) (2M) = 20000.

But you need to exercise your options to for a dollar a piece, so your net profit is $10000.

This is a bit of a contrived example, but hopefully it shows why down rounds are so bad. The only time when you get the full (acquisition value) * (equity percentage) is if all the perferred investors choose to get their shares instead of their investment back. 

#### *Scenario B*
Suppose Sally Startup does really well and get's acquired for 40M. 

In this case, the perferred stockholders have the option to either get their investment back (5M) or their equity value which is 50% of 40M or 20M.

So after consulting taking the 20M, there's still 20M left for the common shareholders, of which you get 1% of. 

You still need to exercise your options for a dollar so you're looking at a (20-1)(10000) = 190000 net profit. 

It's interesting to note that in Scenario B the company sold for 6x than in Company A, yet you recieve 19x the profit in Scenario B, largely due to the existing financing/cap structure. Long term vs short term capital gains tax could make this difference more pronounced. 

Hope you found this helpful. If it helps you can find a analysis of SF SWE offer data [here](/Who-gets-paid-what-in-VC-wonderland).
