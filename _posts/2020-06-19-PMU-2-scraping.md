---
title: PMU project part 2 — Data scraping
tags: [Python, Pandas, SQL, AWS]
style: fill
color: secondary
description: First step of the project is collecting data and storing it in a SQL database.
---

*First step of the project: collecting data and storing it in a SQL database.*

<script type="text/javascript"
        src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS_CHTML"></script>

<img src="../images/digging.jpg">
<div style="color: #BABABA; text-align:right">Photo by Aubrey Rose Odom on Unsplash</div>
<br>

## The source

I took the data from the following website: <a href="https://www.turf-fr.com">www.turf-fr.com</a>. Dozens of websites exist that repertory the results 
of French races, but I chose that one first because it seemed to have more complete information on each race, but also 
because it had a readily accessible "archive" section where one could find race results going back up to 2004 ordered 
in a HTML format that made it easier to webscrape.

## The data collected

I chose a straightforward approach for data collection, considering that more refined classifications could be made 
later on: each database entry corresponds to one horse running at one race. That means that, for a given race where, 
say, 10 horses compete, the database hosts 10 entries, one for each horse.  

**Nota bene:** the data was collected from a source written in French. I am not familiar with the English terms in 
usage, so I did my best to come up with translations, which might or might not be what English-speaking professionals 
use. I nonetheless did my best to explain what each term refers to. For bilingual readers, the French term has been included in parenthesis.

Below is a breakdown of the informations that were collected *about the horse*:

| Feature's name   | Information |
|:----------------:|-------------|
|horse_ID          |the horse's number (a form of ID for the race)|
|horse_name        |the horse's name|
|unshoed           |whether the horse is shoed or unshoed (some horses might run with horseshoes on every hoof, just on their front or rear hooves, or might not have any shoes on; considering the shoes' weight, this is sometimes believed to have an impact on the horse's performance; more details in French <a href="https://hippique.blog-pmu.fr/2015/06/11/le-deferrage/">here</a>)|
|jockeys_weight    |the jockey's weight|
|barrier           |the horse's barrier position (the equivalent of a sprinter's starting block)|
|country           |the horse's country of origin (seemingly the place of birth)|
|blinkers          |whether the horse has blinkers or not (to keep nervous horses from being influenced by peripheral presences; several types of blinkers exist, but the source only informs on the blinker's presence or absence, not on their type)|
|sex               |the horse's sex: female, male, gelding (castrated males)|
|age               |the horse's age|
|jockey            |the jockey's name|
|trainer           |the trainer's name|
|average_km_timing |the horse's average timing for a km during the race|
|wins              |how much money the horse won at this race|
|former_performance|the horse's former performance (see below)|
|odds              |the odds on the horse (the ratio of total money not bet on the horse to the total money bet on the horse)|
|running           |whether the horse ends up running the race (some horses are scheduled to run but end up not running)|
|finish_position   |the horse's finishing position|

I added to these informations more info *about the race*:

|Feature's name   | Information|
|:---------------:|------------|
|race_category    |the race category (see below)|
|race_subcategory |the race subcategory (see below)|
|race_name        |the race's name|
|race_date        |the date of the race|
|race_time        |the time of the day when the race is scheduled|
|race_prize       |the race's total money prize|
|race_length      |the race's length|
|race_horse_number|the number of horses supposed to run|

Before I move on, one quick precisions on the races categories and the horse's performance.

### Races categories and subcategories

The races have been divided using two indications that I named "category" and "subcategory" even though there is no hierarchy between the two qualifiers. 

The categories of races are:
- **trotting** ("trot"), where horses run in a medium-paced gait, the trot
- **jump racing** ("obstacle"), where the horses have to race over obstacles
- **flat racing** ("plat"), where horses run full speed (gallop) between two points

The subcategories are:
- for trotting:
  - **harnessed** ("attelé"), where the horses are pulling a two-wheeled cart called "sulky" where the jockey sits
  - **saddled** ("monté"), where the jockey rides atop the horse
- for jump racing:
  - **hurdling** ("haies"), where the horses have to jump over meter-high obstacles made of brush, racing typically around 3 to 4kms
  - **steeplechase** ("steeple-chase"), where the horses encounter a greater variety of obstacles such as mounds, pits, water streams... and race a bit longer than for hurdling, typically more than 4kms
  - **cross-country** (same in French), where the race takes place in the open country, making use of various obstacles in the wild and extending from 5 to more than 7kms
- for flat racing there is no subcategory

### Music: the horse's past performance
For each running horse their past performance is indicated as a list of pairs of letters and numbers. In French this is
poetically refered to as the horse's "music."

Here's a few examples:
> 0a Da Da Dm 6a Da 7a 9a 2a 5a Da 6a

> 7h Ah 6p 13p 0p 6p 0p (16) 5s 9h 4s 4s 1s  

These indicate the horse's results in previous races, going from the most recent race (left) to oldest (right).

For a given pair, the first element indicates the horse's finishing position or the reason why they didn't finish, while the second element indicates the race's sub/category.

For the first element the possible characters are:
- **0** if the horse didn't finish among the first 9 horses
- **1 to 9** to represent the finishing position of the horse
- **A** if the jockey decided to stop ("arrêté") the horse mid-race
- **D** if the horse was disqualified ("disqualifié") for not adopting the required pace; this only applies to trotting races
- **R** ("rétrogradé") if the race's result were revised by the judges because the horse obstructed another horse's way
- **T** if the horse fell ("tombé")

For the second element:
- **a** for harnessed trot
- **m** for saddled trot
- **h** for hurdling
- **s** for steeplechase
- **c** for cross-country
- **p** for flat racing

## Scraping strategy

The website archive's section is neatly arranged by year, each year leading to a page presenting a list of the months, each month leading to a list of links of the day races happening (one link per race *and* location, meaning races can happen at several locations on a given day; to note too, several races usually occur on a given day within a same location).

For each feature I chose to take the content between the HTML tags *as is* instead of assumming what the possibilities could be, in case some feature would have possible values that I wasn't aware of (say, a special race subcategory).

I did my best to prepare for missing values, filling in with a "n/a" whenever the value wasn't indicated.

**To be noted:** "n/a" does not necessarily mean the website's information was missing; some races category, for instance, do not specify the jockey's weight or do not have barriers numbers, so that it is expected to have a "n/a" for those features in those cases.

**Also:** some feature values are inferred by the *absence* of content within the tags: for instance, in the case of blinkers, only their presence is indicated on the HTML page, so that it is impossible to distinguish a no-blinkers status from a lack of data (as opposed to the jockey's name feature, where absence of information clearly conveys lack of data).

Speaking of missing values, the page of the races of Sunday the 31st of 2019 at Auteuil was empty, with no race being indicated. I reluctantly added two lines of code to that effect to check whether the page was containing races (I say reluctantly because those lines make the program run a `if` statement for every single race just to be able to deal with one exception; but I suppose it's always better to build a program that can deal with as many special cases as possible).

I regularly noticed abnormal data, such as the number of horses listed being greater or lower than the announced number of horses running, horses being listed twice, non-running horses listed as finishing the race (although that case might be explained by a decision from the judges) and so on.

A procedure for removing horses listed twice in a given race was run anytime the size of the race's dataframe wasn't matching the announced number of horses running, instead of running it for every race, to save computing time. This however will backfire in the special case where one horse is missing and one is listed twice; I consider this case to be highly unlikely, and amendable during the wrangling process anyways.

## Saving the data

The data was initially saved in csv format, first year by year (my computer being unable to handle holding all the data in a given dataframe), then starting 2017 semester by semester. I later decided to upload the data onto an AWS database using a MySQL engine, less out of necessity and more to familiarize myself with those tools. 

## Code

The code for the webscraping program is <a href="https://github.com/Rufski/DS_project_Horse_races/blob/master/PMU_webscraping.py">here</a>.
