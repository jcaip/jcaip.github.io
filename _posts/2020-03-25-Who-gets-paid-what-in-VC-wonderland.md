---
published: True
layout: post
tags: [startups]
title: SF Software Engineer Startup Offer Data
---

A look at who gets paid what in San Francisco. 

This is a follow up to an earlier post on [startup equity](/Startup-Equity-TLDR)

<!--more-->

I was researching job offers for startups in San Francisco, and wanted to get some real data. 

I found [this analysis](https://www.codingvc.com/analyzing-angellist-job-postings-part-2-salary-and-equity-benchmarks) from 2014 which is pretty good, but is a bit old. 

Unfortunately AngelList has since deprecated their API so instead I manually scrolled through ~370 job postings and copied over the company name, job title, company size, and salary and equity ranges. I restricted companies to those strictly in SF. 

### Salary and Equity for Different Roles

Firstly, I wanted to see the salary and equity breakdown by role. 

![salary_by_title](/images/startup/salary_by_title.png){: .center}

![equity_by_title](/images/startup/equity_by_title.png){: .center}

Basically EM's get paid bank, but this kind of makes sense - the only startups that are trying to hire engineering managers should be quite large. 

Data Engineers and Devops Engineers see to have the highest salary but this may just be some noise. I was a bit generous with applying the devops engineer label - I catagorized stuff like platform engineer into devops engineer which may have inflated the salaries and also the equity numbers. 

The equity numbers have a lot more noise, but it looks pretty consistent for roles outside of CTO / devops. Most of the difference is probably from sample size. 

I also broke down these numbers for senior roles. This kind of explains why the average data engineering salary is so high - most data engineering roles are senior roles on average. 

It looks like Senior roles offer 20k + salary. It's hard to spot a trend amongst senior roles in equity data. 

Here is the full table of the min, max and avg salary and equity of all job offers, broken down by job title / whether that position is senior or not. You can also check out the notebook I used to make these graphs [here]().

|  Job Title, Senior                   |   Min Salary |   Max Salary |   Avg Salary |   Min Equity |   Max Equity |   Avg Equity |   Count |
|:-------------------------------------|-------------:|-------------:|-------------:|-------------:|-------------:|-------------:|--------:|
|  'Backend Engineer', False           |     103.75   |      150.833 |      127.292 |    0.105     |     0.44125  |     0.273125 |      24 |
|  'Backend Engineer', True            |     130.833  |      167.083 |      148.958 |    0.107583  |     0.679167 |     0.393375 |      12 |
|  'Data Engineer', False              |     115      |      144.286 |      129.643 |    0.0814286 |     0.225714 |     0.153571 |       7 |
|  'Data Engineer', True               |     121.5    |      177.5   |      149.5   |    0.1135    |     0.747    |     0.43025  |      10 |
|  'Devops Engineer', False            |     122.857  |      160.714 |      141.786 |    1.95505   |     2.23024  |     2.09264  |      21 |
|  'Devops Engineer', True             |     127.143  |      173.571 |      150.357 |    0.0631429 |     0.204286 |     0.133714 |       7 |
|  'Embedded Engineer', False          |      76.6667 |      123.333 |      100     |    0.0166667 |     0.4      |     0.208333 |       3 |
|  'Embedded Engineer', True)          |     131.25   |      177.5   |      154.375 |    0.025     |     0.2      |     0.1125   |       4 |
|  'Engineering Manager', True         |     150.417  |      176.667 |      163.542 |    0.1775    |     0.55     |     0.36375  |      12 |
|  'Frontend Engineer', False          |      96.3158 |      145.684 |      121     |    0.138421  |     0.592105 |     0.365263 |      19 |
|  'Frontend Engineer', True           |     125      |      166.111 |      145.556 |    0.171667  |     1.06006  |     0.615861 |      18 |
|  'Fullstack Engineer', False         |     100.909  |      141.212 |      121.061 |    0.177606  |     0.809394 |     0.4935   |      33 |
|  'Fullstack Engineer', True)         |     114.773  |      160     |      137.386 |    0.192955  |     0.825909 |     0.509432 |      22 |
|  'Head of Engineering / CTO', True   |     117.708  |      163.125 |      140.417 |    2.15625   |     5.06042  |     3.60833  |      24 |
|  'Machine Learning Engineer', False  |     108.864  |      156.818 |      132.841 |    0.160909  |     0.840909 |     0.500909 |      22 |
|  'Machine Learning Engineer', True   |     121.364  |      166.364 |      143.864 |    0.189091  |     0.561818 |     0.375455 |      11 |
|  'Mobile Engineer', False            |     101.053  |      152     |      126.526 |    0.0805263 |     0.481579 |     0.281053 |      19 |
|  'Mobile Engineer', True             |     124.562  |      167.75  |      146.156 |    0.185     |     0.636875 |     0.410938 |      16 |
|  'Software Engineer', False          |     101.806  |      148.75  |      125.278 |    0.11375   |     0.488611 |     0.301181 |      36 |
|  'Software Engineer', True           |     124.853  |      166.059 |      145.456 |    0.303118  |     1.17474  |     0.738926 |      34 |

### Salary and Equity for Different Sizes

Additionally I looked on the effect size has on salary/equity. 

![salary_by_size](/images/startup/salary_by_size.png) ![equity_by_size](/images/startup/equity_by_size.png)

No surprise here - the smaller the company, the less money the offer on average and the more equity they offer on average. 

It seems your employee number has a large affect on the equity that you receive, much more than your job title. 


### Salary vs. Equity
Lastly I tried to see if there was a correlation between salary offered and equity offered. Equity and salary are usually presented as two sides of the coin - you give up salary to get equity and vice versa. 

It's quite hard to see a precise inverse correlation between the two. 

![correlation](/images/startup/salary_vs_equity.png){: .center}

So it looks like if you want a lot of equity you need to be one of the first few hires for startups. 
Salary increases and equity decreases with size, and being a senior engineer adds about 20k on average. 

Given the ratio of senior engineers to new engineers, it definitely seems like there are more job expected. 

Hope you find this helpful. 
