---
published: True
layout: post
tags: [startups]
title: SF Software Engineer Startup Offer Data
---

A look at who gets paid what in San Francisco. 

<!--more-->

# AngelList Data Analysis

I found this analysis from 2014 which is pretty good. 

But I wanted to see for myself, so I wanted to do a similar analysis. Unfortunately AngelList has since deprecated their API so instead I manually scrolled through ~370 job postings and copied over the company name, job title, company size, and salary and equity ranges. 

This was very painful and took me like 3 hours. 

This was just companies that offered > 40K salary.  

#### Salary and Equity for Different Roles
![salary_by_title](/images/startup/salary_by_title.png)

![equity_by_title](/images/startup/equity_by_title.png)

Basically EM's get paid bank, but this kind of makes sense - the only startups that are trying to hire engineering managers should be quite large. 
Data Engineers and Devops Engineers see to have the highest salary but this may just be some noise. I was a bit generous with applying the devops engineer label - I catagorized stuff like platform engineer into devops engineer which may have introduced some bias. 

I also broke down these numbers for senior roles. This kind of explains why the average data engineering salary is so high - most data engineering roles are senior roles. 

On average, it looks like Senior roles offer 20k + salary. It's hard to spot a trend amongst senior roles in equity data. 

#### Salary and Equity for Different Roles (Segmented by Senior)
![senior_salary_by_title](/images/startup/senior_salary_by_title.png)

![senior_equity_by_title](/images/startup/senior_equity_by_title.png)

![senior](/images/startup/senior.png)

#### Salary and Equity for Different Sizes

Finally I looked on the effect size has on salary/equity

![salary_by_size](/images/startup/salary_by_size.png) ![equity_by_size](/images/startup/equity_by_size.png)

No surprise here - the smaller the company, the less money the offer on average and the more equity they offer on average. There's a huge dropoff in equity when you join even a moderately larger company.


#### Salary vs. Equity
Lastly I tried to see if there was a correlation between salary offered and equity offered. Equity and salary are usually presented as two sides of the coin - you give up salary to get equity and vice versa. 

But from the data it's very difficult to reach that conclusion. 

![correlation](/images/startup/salary_vs_equity.png)

The full data can be found [here]() and you can see the code / notebook I used to analyze the data [here](). 

The notebook has a couple of tables that are probably more informative than these graphs. 
