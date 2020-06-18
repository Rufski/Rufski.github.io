---
title: PMU project part 2 — Data scraping
tags: [Python, BeautifulSoup, Pandas]
style: fill
color: primary
description: First step of the project: collecting data and storing it in a SQL database.
---

*First step of the project: collecting data and storing it in a SQL database.*

<img src="../images/digging.jpg">
<div style="color: #BABABA; text-align:right">Photo by Aubrey Rose Odom on Unsplash</div>
<br>

## The source

I took the data from the following website: https://www.turf-fr.com. Dozens of websites exist that repertory the results of French races, but I chose that one first because it seemed to have more complete information on each race, but also because it had a readily accessible "archive" section where one could find race results going back up to 2004.

## The data collected

I chose a straightforward approach for data collection, considering that more refined classifications could be made later on: each database entry corresponds to one horse running at one race. That means that, for a given race where, say, 10 horses compete, the database hosts 10 entries, one for each horse.  

**Nota bene:** the data was collected from a source written in French. I am not familiar with the English terms in usage, so I did my best to come up with translations, which might or might not be what English-speaking professionals use. I nonetheless did my best to explain what each term refers to.

Below is a breakdown of the informations that were collected *about the horse*:
- the horse's number (a form of ID for the race)
- the horse's name
- whether the horse is shoed or unshoed (some horses might run with horseshoes on every hoof, just on their front or rear hooves, or might not have any shoes on; considering the shoes' weight, this is sometimes believed to have an impact on the horse's performance; more details in French <a href="https://hippique.blog-pmu.fr/2015/06/11/le-deferrage/">here</a>)
- the jockey's weight
- the horse's barrier position (the equivalent of a sprinter's starting block)
- the horse's country of origin (seemingly the place of birth)
- whether the horse has blinkers or not (to keep nervous horses from being influenced by peripheral presences; several types of blinkers exist the source only informs on the presence or absence of them)
- the horse's sex: female, male, gelding (castrated males)
- the horse's age
- the jockey's name
- the trainer's name
- the horse's average timing for a km during the race
- how much money the horse won at this race
- the horse's former performance (more on that later)
- the odds on the horse (how much people bet on the horse as opposed to other horses; calculation detailed below)
- whether the horse ends up running the race (some horses are scheduled to run but end up not running)
- the horse's finishing position

I added to these informations more info about the race itself:
- the race category ()
- the race subcategory
- the race's name
- the date of the race
- the time of the day when the race is scheduled
- the race's total money prize
- the race's length
- the number of horses supposed to run

Before I move on, two quick precisions on the horse's performance and the way odds are calculated.

### Music: the horse's past performance
For each running horse their past performance is indicated. In French this is poetically refered to as "music." 

### How odds are computed
Odds follow the below formula:
