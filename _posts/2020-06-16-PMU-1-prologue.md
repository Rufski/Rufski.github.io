---
title: PMU project part 1 — Prologue
tags: [Statistics]
style: fill
color: primary
description: On the stupid and less stupid ways of making money through horse races.
---

<script type="text/javascript"
        src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS_CHTML"></script>

*On the stupid and less stupid ways of making money through horse races*

<img src="../images/dice.jpg">
<div style="color: #BABABA; text-align:right">Photo by Jonathan Petersson on Unsplash</div>
<br>

## The stupid way to win at the races

In my early 20s I fell into a dumb trap.

Somewhere on the internet I had bumped into one of those "one weird tricks found by a single mom in your area." The trick itself
was presented as a fresh, unknown and logic-tight method to win at the horse races. I later came to find out that that trick had actually been around since the XVIIIth century (fresh indeed) and had been debunked by a mathematician, Joseph L. Doob, about 60 years ago.

The trick went as follows:

> - Find a bet that, if won, would double your ante
> - Bet one unit on this game
> - If you lose, keep finding other similar bets and bet until you win, each time betting twice the amount you previously bet
> - If you win, return to the first step

The naive idea was that even if you would lose *n* bets, having successively yet unsuccessfully bet 1 dollar, 2 dollars, 4 dollars... \\( 2^{n-1} \\) dollars, the sum of those bets equals \\( 2^n -1 \\) (finite geometric series), so that by finally betting \\( 2^n \\) and winning twice that, the net gain from this series of bet would be \\( -(2^n - 1) - 2^n + 2\cdot 2^n = 1 \\). **With this strategy, you always end up winning one dollar, no matter what!**

Terms and conditions apply, of course. I logged onto a registered online betting website and started applying the method. And I won. And I won. And I kept winning, one euro at a time. And I felt like a smart guy, having hacked the system with a fool-proof way to keep winning... until, for one series of games, I started to have to double my bets again, and again, and again, until I found myself having to bet *all my money* (a mere 100 euros, but a fortune for me at the time). That's when I realized the trap: **you need to have an infinite amount of money available to bet in order to keep applying the trick, otherwise you will eventually run into a situation where you cannot double your previous bet.**

Here's one easy way to visualize this:

<img src="../images/pmu-1-img1.png" align="center">

I'm plotting the approximate chances of reaching 100 dollars as a function of the initial sum available to play (these chances have been computed by Monte-Carlo simulations over 10,000 games for each initial sum; I assumed a 50% chances of winning). Notice how the graph slightly curves down, meaning you will generally have *less than a 50% chance* of doubling your initial investment. If you're looking to reach the 100 dollars with a 95% confidence, you need to start with... 97 dollars! With a 3% gain that's not the most lucrative of games.

There's further problems with the application of this strategy, among others the fact that, while I assumed above a 50% chances of winning, nothing says that a horse with odds 2:1 (for which your bet will be doubled if you win) has a 50% chances of winning. Rather, odds represent the pool of betters' **confidence** in the horse (how many betters placed bets on the horse), which might, or might not, prove justified. One might find out horses with 2:1 odds only win 10% of the time (let me actually hold onto that thought and verify this later).

## The less stupid way to win at the races

Fast forward ten years later. I bump into <a href="https://www.bloomberg.com/news/features/2018-05-03/the-gambler-who-cracked-the-horse-racing-code">this very entertaining article</a> telling the story of Bill Benter, who made a fortune by applying statistical anlysis to Hong Kong horse races. Much better than the single mom and her one weird trick. Over the span of 15 years, Benter and his associates developed a quantitative analysis team that worked on strategies that would eventually win them the biggest jackpot of the Hong Kong races (amounting to about $13 millions). That single sentence does not give credit to Benter's fantastic story so, if you got time, do read the Bloomberg article.

That article found me in times where I was waist deep into machine learning. I found myself excited at the idea of applying those new tools to a topic that had made a fool out of me ten years ago (nah, let me be humble: *I*'m the only one responsible for looking like a fool).

**So here is what this PMU project is about:** horce races results are widely available online. I will be collecting this data, analyze it and come up with a classifier that can predict, given the set-up of a race, what the outcome will be.

Now, with maturity comes humility: I don't believe I can come up with a system akin to that of Bill Benter; after all, he was working with a team of mathematicians, software engineers and other researchers, and as a Ph.D. there's no way I can beat that on my own. I can still hope to develop a classifier that allows for decent returns while keeping the chances of gambler's ruin minimal, but I do have doubts as for whether that's possible. We'll see.

*However*, it is entirely possible to develop a system that will just beat in predictive terms the tips given by specialized websites. If I can manage to develop a classifier good enough, and honestly advertise this machine as, for instance, "a predictive system that makes you 1000 times more likely to win the jackpot" (sweeping under the rug the fact that it's still just one chance in a million), *then* I would have a product that can be sold to betters. <a href="https://www.boturfers.fr/">Such website actually already exists</a>.

## Journey plan

Links to the corresponding pages will be updated as I go.

1. Find and collect the data
2. Clean the data found
3. Analyse the data
4. Develop a predictive classifier for the outcome of races

Let's see if I can prove smarter this time around!
