---
name: S2DS x EPRI solar farm project
tools: [Python, Keras, Tensorflow, Pandas, Numpy, Matplotlib, Literature Research]
image: ../images/appa.jpg
description: How fast does electricity production decrease in solar farms? A neural network approach.
---

<h1><b>S2DS x EPRI: solar farm project</b></h1>
<br>
<div style="color: #888888; font-style: oblique">How fast does electricity production decrease in solar farms? A neural network approach.</div>
<br>
<img src="../images/appa.jpg">
<div style="color: #BABABA; text-align:right">Photo by the American Public Power Association on Unsplash</div>
<br>
<div style="background-color: #CEE6FF; border-width: 3px; border-color: #007BFF; border-style:solid; margin: 15px; padding: 15px">
<h2> In 60s or less:</h2>
  <div>As part of the [S2DS](http://www.s2ds.org/) bootcamp/hackaton, in partnership with the [Electric Power Research Institute](https://www.epri.com/) (EPRI), I worked within a team of 5 to quantify the decrease over time of the capacity of a solar farm to produce electricity. This post presents the problem statement and my own contributions to this team project, mainly:</div>
<ul>
    <li>Perform a thorough analysis of the data provided by EPRI, and detect anomalies that allowed EPRI to revise their solar farm simulations (from which the data was produced);</li>
    <li>Develop a neural network that quantifies the deterioration of electricity production due to material ageing with an accuracy 5 times greater than the industry's current tools;</li>
    <li>Present a layman-accessible introduction to the business and technical case of the project to other businesses partnering with the S2DS bootcamp.</li>
  </ul>
</div>

### The business case
Solar energy currently accounts for the largest investment worldwide in means of energy production, and is expected to be the world’s first source of electricity by 2050. Given the skyrocketing interest in this domain, we need solutions that guarantee the smallest risk in investment and the highest accuracy in ROI.

The performance of solar farms, as with any other piece of electric equipment, declines over time. This can be due to factors such as the natural ageing of the material, or dirt and snow damaging the photovoltaic cells. Because solar-generated electricity is still a young commercial practice (95% of solar farms having been active for less than 10 years), little is known about how badly those factors affect electricity generation. Yet this information is crucial to the industry: it is estimated that **80 million dollars** could be saved **each year** by having an accurate estimate of the long-term impact on electricity production of weather, dust and material used.

To respond to this demand, our team was asked to come up with methods that analyze the amount of electricity produced over time at a given solar farm, and return a history over the same period of time of two indexes representing the impact on electricity production of, respectively, the material's natural ageing, and the dirt on photovoltaic cells. Once developed, these methods could be applied to existing solar farms so as to classify them according to these indexes, see which solar farms deteriorate more quickly, and identify common factors among worst-performing solar farms.

### Team work and personal contribution

The team used an agile development approach, holding scrum meeting every morning, meeting with a technical contact from EPRI in the afternoon. Each member focused on a task, with occasional collaborations when a task needed several pairs of hands. The team's approach was to explore a variety of techniques and tools (Facebook's Prophet, Unobserved Components, ARIMA, EEMD...) aimed at time series analysis, and single out which technique could hold the best results.

#### Data Analysis

As stated above, the task was to develop a model that takes as input a time series of the electricity production of a solar farm. For that purpose we received data from our technical partner at EPRI of such time series, not measured on actual solar farms but created by software simulation. This allowed us to start with naively simple data and gradually increasing its complexity: first ignoring all degradation effects except the material's ageing, then including damages due to dirt, then including variations due to weather.




#### Neural network model

### Code
[The code is here](https://github.com/Rufski/PhD_work_Effective_parameters_retrieval_program)



