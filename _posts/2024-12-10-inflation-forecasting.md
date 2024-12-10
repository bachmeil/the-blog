---
layout: post
title: Forecasting Long-Term Inflation
---
My early research was dominated by the topic of inflation. I'm not going to go into the details of that today. Instead, I want to respond to some things I see in various places on the internet (Reddit, HN, YouTube, ...).

It's trendy to talk about "In today's economy". The economy is actually pretty good relative to past decades. It might be trendy to claim the economy is horrible, but trendy and accurate are too different things, perhaps even mutually exclusive.

> the cost of living has risen markedly in recent years as inflation has soared

That's a quote from the *Chronicle of Higher Education* in November 2024. It paints a picture of living costs that are out-of-control and rising rapidly. October's CPI inflation rate was 2.6%. Inflation hasn't soared. Maybe what was meant is that the cost of living has soared. It exceeded the Fed's target in March 2021 and rose until June 2022. So, it "soared" for a 15-month period that ended more than two years before the article was written.

More likely what the author (who is probably not a PhD economist) means is that the cost of living rose as things opened up after the pandemic and didn't return to their previous levels. Of course, that's nonsense, because we always expect there to be some inflation. The average inflation rate over the last 30 years has been 2.5%. On the eve of the pandemic, the inflation rate was 2.5%, and I don't recall a lot of screaming about out-of-control inflation at that time. Inflation was at 2.9% in 2018, and I don't remember a lot of "in this economy" comments then, either.

Let's do an exercise. Let's look at where the CPI is today, and where it would be with a constant 2.5% annual inflation rate from October 2019 to today (just before the pandemic, to make the math simple). The current CPI value reported on FRED is 315.454. October 2019 it was 257.155. At 2.5% inflation, the CPI would be 290.9 rather than 315.5. That's an 8.4% increase in the cost of living. That's something, for sure, but it's not a crisis, and it doesn't account for the fact that at least part of that 8.4% increase was a direct response to higher wages. 

Income is harder to measure (how do you account for someone that retired due to the pandemic) but average hourly earnings of all employees rose from 28.23 in October 2019 to 35.48 in October 2024. That's an increase of 26%. We don't have a natural benchmark for hourly wage growth, but we can compare with the good old days of the five years earlier. October 2014-October 2019 wage growth was 15%. So at least in terms of per-hour wages, the cost of living rose 8.4 percentage points more than target, but wages grew 11 percentage points more than in the five pre-pandemic years.

The hourly wage absolutely excludes retirees, business owners, children, and others. We can then zoom out to a higher level. Is real GDP still growing? The answer is yes. It's up 12.2% in the last five years. There's more output, there's more income, so if there's a problem, it's one of distribution. That's outside the scope of this post.

> Well if inflation is so low, what about [one category of expenditure] or [one location] or [personal experience]

This useless argument got a lot of attention when CNN ran a story about a family that consumed an extremely unrepresentative amount of milk each month, and then attempted to mislead viewers into thinking this was a normal situation. You see a similar argument all over the internet. The whole point of constructing a measure of the cost of living is that you don't want to focus on one tiny part of your overall consumption when calculating how much it costs to live. 

Do you see retirement planners telling people that they can retire based on projections of the price of milk over the next 30 years? Of course not. Anyone doing that would cease to be a retirement planner for long. The question is so absurd that you're dismissive of the premise. Yet that is what CNN was proposing we do.

Also notoriously absent from their reporting, and another reason you don't want to focus on just one item, is that the price of milk started rising in the middle of 2018. It went from \$2.84 in July 2018 to \$3.25 by January 2020, a 14% increase in only 18 months. I searched but did not find any evidence that CNN did stories on the soaring price of milk before the pandemic.

The non-misleading version of the story is that milk prices peaked in 2014 at \$3.86 a gallon. The price collapsed to \$2.84 by July 2018, and then recovered to \$4.20 in May 2022. In real terms, the 2022 milk price was substantially lower than the 2014 price. Prices go up and down. You can't take a highly volatile price and only run news stories on a selected sample of the times they go up. This is all stuff that a basic fact-checker would have raised if the goal was accuracy in reporting.

> Price increases are permanent

This is a common claim. As far as I can tell, the idea is that since prices never go down, *higher inflation today* means *higher prices by the same amount forever*. As natural is this might feel, it's built on very strong, unstated assumptions that are hard to justify in practice.

Let's start by stating the obvious: for any long slice of post-WWII data you grab, the US CPI is going to have an upward trend. But why would that alone imply that inflation being a couple percentage points above target this year translates into a two percentage point higher forecast of the cost of living next year, the year after, and so on for the rest of your life? That claim is nothing more than an arbitrary, unjustified assumption.

Here's an example to illustrate my point. In both cases, the Federal Reserve is targeting 2.5% inflation over a ten-year horizon. Scenario 1 has 2.5% every year for 10 years. Scenario 2 has a burst of inflation due to a supply chain shock, leading to 10% higher prices versus 5% in 2022. From 2022 to 2024, inflation is much lower in Scenario 2 than in Scenario 1. By 2030, the price level is the same in both Scenario 1 and Scenario 2. The common view seems to be that you can't have Scenario 2 at all. The right way to forecast inflation is Scenario 3, where future inflation is independent of its previous values. My understanding is that the Fed is trying to do something like Scenario 2 rather than Scenario 3.

| Year | Scenario 1 | Scenario 2 | Scenario 3 |
|:----:|:----------:|:----------:|:----------:|
| 2020 | 100        | 100        | 100        |
| 2022 | 105        | 110        | 110        |
| 2024 | 110        | 113        | 115        |
| 2026 | 116        | 118        | 121        |
| 2028 | 122        | 123        | 127        |
| 2030 | 128        | 128        | 133        |

However, the premise that prices always go up is wrong. The CPI does in fact decrease every so often. There was a bout of inflation prior to the financial crisis that was followed by a noticeable deflation in 2009. Then in the aftermath of the crisis, inflation floated around zero, and was even negative, for most of 2015. On a month-to-month basis, prices fell in the first three months of the pandemic, though never enough for the year-over-year value to go below zero. There's no law stating the cost of living has to rise - we've had three counterexamples in the past 15 years!

You might then ask whether the Federal Reserve is willing to run inflation at less than 2.5%. Some internet commentators will tell you the Fed wants 4% or higher inflation. Well, if you're one of those skeptics, look at inflation from 2013 to 2016. That time period had only five months with an inflation rate of 2% or higher, the highest inflation reading was 2.2%, and it fluctuated around 1%. They loosened things up a bit starting in 2017, but it never got above 3% again prior to the pandemic. It was under 2% as late as October 2019. So, if you're wondering, the Federal Reserve is willing to engineer inflation rates under 2.5%.

We might then ask what would be needed for 2020-2029 to have an inflation rate of 2.5% per year (in other words, what is needed so that someone retiring in 2019 and planning for 2.5% inflation over the next decade would have been correct)? The inflation rate would have to be 1% per year for the next five years. I'm not sure they'll get there, but we've seen those inflation rates before. Even at 2%, inflation until the end of 2029 would bring the 10-year inflation rate to 3%.

The choice of a decade is probably too short given the nature of our inflation shocks. Surely nobody is going to fail in their retirement plan because inflation was 3% instead of 2.5% over a decade. Stretching the horizon to 15 years, 1.8% inflation over the next decade will result in a 15-year inflation rate of 2.5%. I don't think it's a stretch to use 2.5% as a long-run inflation forecast. Maybe 2.8% as a "just in case" ceiling.