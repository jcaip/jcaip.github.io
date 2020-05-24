---
title: Startup Equity Primer
tags: [startups]
published: True
layout: post
---

This is a analysis of SF AngelList job postings and a short explanation of early employee stock options. 

I was researching job offers for startups in San Francisco, and wanted to get some real data. 

Hopefully this can be a resource for anyone looking for work at startups in SF right now. 


<!--more-->

## SF Startup Job Data

I found [this analysis](https://www.codingvc.com/analyzing-angellist-job-postings-part-2-salary-and-equity-benchmarks) from 2014 which is pretty good, but is a bit old. 

Unfortunately AngelList has since deprecated their API so instead I manually scrolled through ~370 job postings and copied over the company name, job title, company size, and salary and equity ranges. I restricted companies to those strictly in SF with a min salary of at least 40k. 

Quick note - this is just raw job posting data, and may not be reflective on actual offers. 

#### Salary and Equity for Different Roles


<p align="middle">
  <img src="/images/startup/salary_by_title.png"/>
  <img src="/images/startup/equity_by_title.png"/>
</p>

Firstly, I wanted to see the salary and equity breakdown by role. 

Basically EM's get paid bank, but this kind of makes sense - the only startups that are trying to hire engineering managers should be quite large/unicorns. 

Data Engineers and Devops Engineers see to have the highest salary but this may just be some noise. I was a bit generous with applying the devops engineer label - I catagorized stuff like platform engineer into devops engineer which may have inflated the salaries and also the equity numbers. 

The equity numbers have a lot more noise, but it looks pretty consistent across roles outside of CTO / devops. Most of the difference is probably from sample size. 

I also broke down these numbers for senior roles. This kind of explains why the average data engineering salary is so high - most data engineering roles are senior roles on average. 

It looks like on average Senior roles pay ~20k more. It's hard to spot a trend amongst senior roles in equity data. 

Here is the full table of the min, max and avg salary and equity of all job offers, broken down by job title / whether that position is senior or not. You can also check out the notebook I used to make these graphs [here](https://github.com/jcaip/startup).


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Job Title (Normalized)</th>
      <th>Senior</th>
      <th>Min Salary</th>
      <th>Max Salary</th>
      <th>Avg Salary</th>
      <th>Min Equity</th>
      <th>Max Equity</th>
      <th>Avg Equity</th>
      <th>Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">Backend Engineer</th>
      <th>False</th>
      <td>104.0</td>
      <td>151.0</td>
      <td>127.0</td>
      <td>0.10</td>
      <td>0.44</td>
      <td>0.27</td>
      <td>24</td>
    </tr>
    <tr>
      <th>True</th>
      <td>131.0</td>
      <td>167.0</td>
      <td>149.0</td>
      <td>0.11</td>
      <td>0.68</td>
      <td>0.39</td>
      <td>12</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Data Engineer</th>
      <th>False</th>
      <td>115.0</td>
      <td>144.0</td>
      <td>130.0</td>
      <td>0.08</td>
      <td>0.23</td>
      <td>0.15</td>
      <td>7</td>
    </tr>
    <tr>
      <th>True</th>
      <td>122.0</td>
      <td>178.0</td>
      <td>150.0</td>
      <td>0.11</td>
      <td>0.75</td>
      <td>0.43</td>
      <td>10</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Devops Engineer</th>
      <th>False</th>
      <td>123.0</td>
      <td>161.0</td>
      <td>142.0</td>
      <td>1.96</td>
      <td>2.23</td>
      <td>2.09</td>
      <td>21</td>
    </tr>
    <tr>
      <th>True</th>
      <td>127.0</td>
      <td>174.0</td>
      <td>150.0</td>
      <td>0.06</td>
      <td>0.20</td>
      <td>0.13</td>
      <td>7</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Embedded Engineer</th>
      <th>False</th>
      <td>77.0</td>
      <td>123.0</td>
      <td>100.0</td>
      <td>0.02</td>
      <td>0.40</td>
      <td>0.21</td>
      <td>3</td>
    </tr>
    <tr>
      <th>True</th>
      <td>131.0</td>
      <td>178.0</td>
      <td>154.0</td>
      <td>0.02</td>
      <td>0.20</td>
      <td>0.11</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Engineering Manager</th>
      <th>True</th>
      <td>150.0</td>
      <td>177.0</td>
      <td>164.0</td>
      <td>0.18</td>
      <td>0.55</td>
      <td>0.36</td>
      <td>12</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Frontend Engineer</th>
      <th>False</th>
      <td>96.0</td>
      <td>146.0</td>
      <td>121.0</td>
      <td>0.14</td>
      <td>0.59</td>
      <td>0.37</td>
      <td>19</td>
    </tr>
    <tr>
      <th>True</th>
      <td>125.0</td>
      <td>166.0</td>
      <td>146.0</td>
      <td>0.17</td>
      <td>1.06</td>
      <td>0.62</td>
      <td>18</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Fullstack Engineer</th>
      <th>False</th>
      <td>101.0</td>
      <td>141.0</td>
      <td>121.0</td>
      <td>0.18</td>
      <td>0.81</td>
      <td>0.49</td>
      <td>33</td>
    </tr>
    <tr>
      <th>True</th>
      <td>115.0</td>
      <td>160.0</td>
      <td>137.0</td>
      <td>0.19</td>
      <td>0.83</td>
      <td>0.51</td>
      <td>22</td>
    </tr>
    <tr>
      <th>Head of Engineering / CTO</th>
      <th>True</th>
      <td>118.0</td>
      <td>163.0</td>
      <td>140.0</td>
      <td>2.16</td>
      <td>5.06</td>
      <td>3.61</td>
      <td>24</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Machine Learning Engineer</th>
      <th>False</th>
      <td>109.0</td>
      <td>157.0</td>
      <td>133.0</td>
      <td>0.16</td>
      <td>0.84</td>
      <td>0.50</td>
      <td>22</td>
    </tr>
    <tr>
      <th>True</th>
      <td>121.0</td>
      <td>166.0</td>
      <td>144.0</td>
      <td>0.19</td>
      <td>0.56</td>
      <td>0.38</td>
      <td>11</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Mobile Engineer</th>
      <th>False</th>
      <td>101.0</td>
      <td>152.0</td>
      <td>127.0</td>
      <td>0.08</td>
      <td>0.48</td>
      <td>0.28</td>
      <td>19</td>
    </tr>
    <tr>
      <th>True</th>
      <td>125.0</td>
      <td>168.0</td>
      <td>146.0</td>
      <td>0.18</td>
      <td>0.64</td>
      <td>0.41</td>
      <td>16</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Software Engineer</th>
      <th>False</th>
      <td>102.0</td>
      <td>149.0</td>
      <td>125.0</td>
      <td>0.11</td>
      <td>0.49</td>
      <td>0.30</td>
      <td>36</td>
    </tr>
    <tr>
      <th>True</th>
      <td>125.0</td>
      <td>166.0</td>
      <td>145.0</td>
      <td>0.30</td>
      <td>1.17</td>
      <td>0.74</td>
      <td>34</td>
    </tr>
  </tbody>
</table>

#### Salary and Equity for Different Sizes


<p align="middle">
  <img src="/images/startup/salary_by_size.png"/>
  <img src="/images/startup/equity_by_size.png"/>
</p>

Additionally I looked on the effect size has on salary/equity. 

No surprise here - the smaller the company, the less money the offer on average and the more equity they offer on average. 

It seems your employee number has a large affect on the equity that you receive, much more than your job title. 

#### Salary vs. Equity

Lastly I tried to see if there was a correlation between salary offered and equity offered. Equity and salary are usually presented as two sides of the coin - you give up salary to get equity and vice versa. 

It's quite hard to see a precise inverse correlation between the two. 

![correlation](/images/startup/salary_vs_equity.png){: .center}

So it looks like if you want a lot of equity you need to be one of the first few hires for startups. 
Salary increases and equity decreases with size, and being a senior engineer adds about 20k on average. 

There seems to be more job postings for senior engineers than I would have expected, but senior talent is something that's hard to find.

## Startup Equity Primer

This is an explanation of some basic ideas so you can try and evaluate how much your equity is worth. 

### Options and Vesting

There are two many different types of stock options, the two most common are: ISOs and RSUs. 

Usually if you get RSUs it means your startup is late stage (think Uber, Airbnb), most early-stage employees receive ISOs. 

These options allow you to buy a share of stock at your **strike price**, which is usually much lower than the actual share price. It's basically a contract allowing you to purchase stock at a reduced price. 

The equity in offers is always structured as "$n$ shares", versus % of the company. So one of the first things you when evaluating an offer is try to figure out what % they are granting you. 

Companies do not give you all your options at once, they **vest** over a period of time. The most common scheme is vesting over 4 years with a one year cliff. 

This means that you get no options until you hit one-year from your starting date, upon which you get $\frac{12}{48}$ of your promised options. After that you get $\frac{1}{48}$ of your options every month until you've received all your options. 

### Dilution and Preferred vs. Common Stock 

Companies raise money by issuing stock, which VCs then buy.  Dilution occurs when new stock is issued as part of financing, which increase the overall total number of shares and reduces your relative %. 

However, ideally each subsequent round also raises the company valuation, so you still stand to make money. 

From what I can tell you can expect 10-15% dilution from each round to the next. 

Companies usually differentiate between sweat equity and cash equity - 
Employees get **common stock** and investors get **preferred stock**.


Preferred stockholders get the option to either have their investment equity or their investment back, before the common stockholders in the event of an exit. I'll go over an example later demonstrating this. 

Some VC funding will also come with a multiple, which allows a sort of guaranteed ROI for the VC. 

### Exiting and Taxes
One thing to keep in mind: A share, like any stock is only worth what other people are willing to pay for it.

There are some markets for early startup stock but by and large, these are a very illiquid asset - you usually can only cash out if the startup is acquired or IPOs.

If you ever leave the company, your options usually expire after 90 days. This means that you'll need to either exercise them or give them up. 

If you buy your options and then sell them within one year, you'll be charged short term capital gains tax on any return, which is currently 37%.

Otherwise if it's longer than a year before when you buy and sell you'll be charged long term capital gains tax, which is much lower - the highest bracket is 20%.

The American tax code is so complicated that there's something else called the Alternative Minimum Tax. Basically you need to pay at least $n\% $ in taxes, so if you make a bunch of money when you sell your stock then you may be taxed under the AMT rather than by the normal tax brackets. 

## Real World Example
Let's consider a concrete example: Say Sally Startup offers you 10000 shares out of a total of 1M shares.

For simplicity we'll say the strike price is $1 and is equal to the current share price, making the current valuation of Sally Startup 1M. 

This means you have 1% of Sally Startup or nominally $10000. 

Sally Startup then releases a new blockchain based iced tea and this peaks the interest of HardBank, a VC firm who decides to invest.

Sally Startup finances this investment (Series A) by issuing an additional 1M shares of preferred stock, which HardBank buys at $5 a share and a 1x multiple.

HardBank has essential invested 5M at a 5M pre/10M post valuation for 50% of Sally Startup. 

When Sally Startup financed that HardBank investment, your 1% was diluted to 0.5%, as you own 10000 shares out of a new total of 2M.  

However, as the valuation of the company has went from 1M to 10M your options have increased in value from \\$10000 to \\$50000.

#### Scenario A

Say later on in a year your company struggles a bit because blockchain is no longer hype and is acquired for 7M.

You pull out your calculator and compute (0.005) 7M = $35000, which is the amount of money you expect to receive. 

However, when Sally Startup sells for 7M this represent a "down round" or a decrease in valuation.

So in this scenario HardBank has the option to either get his investment back (5M) or his share value which is 50% of 7M or 3.5M. This is because HardBank has preferred stock. 

So after HardBank's team of quants does the math and takes the 5M, there's only 2M left, which is split among the plebs who have common stock. 

This is also where the multiple comes in - if the multiple was say 7/5x, then HardBank could take the entire 7M and leave nothing. 

So after the preferred investors recover their investment, you get (0.01) (2M) = 20000.

But you need to exercise your options to for a dollar a piece, so your net profit is $10000.

This is a bit of a contrived example, but hopefully it shows why down rounds are so bad. The only time when you get the full (acquisition value) * (equity percentage) is if all the perferred investors choose to get their shares instead of their investment back. 

#### Scenario B
Suppose Sally Startup does really well and get's acquired for 40M. 

In this case, the perferred stockholders have the option to either get their investment back (5M) or their equity value which is 50% of 40M or 20M.

So after consulting taking the 20M, there's still 20M left for the common shareholders, of which you get 1% of. 

You still need to exercise your options for a dollar so you're looking at a (20-1)(10000) = 190000 net profit. 

It's interesting to note that in Scenario B the company sold for 6x than in Company A, yet you recieve 19x the profit in Scenario B, largely due to the existing financing/cap structure. Long term vs short term capital gains tax could make this difference more pronounced. 

Hope you found this helpful. 

