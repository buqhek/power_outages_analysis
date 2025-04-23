# Can Power Outages Predict Your Monthly Electricity Bill?

## Introduction

First and foremost, I would like to acknowledge that this dataset was collected by Purdue University, located [here](https://engineering.purdue.edu/LASCI/research-data/outages). It contains various pieces of information about the climate, electricity consumption patterns, and other pieces of useful information regarding the power outages across the United States between the years 2000-2016. The main point of this analysis is to take a look at how the world around us impacts our lives in ways we might not really think about. 

As a result, what makes this dataset and analysis very interesting seeing whether we can predict the monthly price (per kilowatt-hour) of our electricity, given a variety of parameters, like state, outage start and end time, month, year, etc...

An important piece of information about this dataset: total number of rows = 1476, and total number of columns = 54. However, we won't be using all columns, so don't worry about having to understand each feature that is present in this dataset.

#### <u>Numerical Columns:</u> 

**- Year:** e.g. 2011, 2014.

**- Month:** e.g. January = 1 up to December = 12.

**- Outage Duration:** Duration of outage, in minutes.

**- Total Price:** Average monthly electricity price in the U.S. state (cents/kilowatt-hour).

**- Total Sales:** Total electricity consumption in the U.S. state (megawatt-hour).

**- Total Customers:** 	Annual number of total customers served in the U.S. state.

**- Anomaly Level:** This represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season - essentially indicators for the environmental climate.

#### <u>Categorical Columns:</u>

**- US State:** The state in which the outage occurs in.

**- Nerc Region:** Regions in North America created by "The North American Electric Reliability Corporation" that were involved in the outage event.

<figure>
  <img src="assets/imgs/Nerc_Regions_Map.jpg" alt="Map of NERC Regions of North America">
  <figcaption align="center"><em>Map of the NERC Regions of North America.</em></figcaption>
</figure>

**- Climate Category:** This represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI).

**- Cause Category:** Cause of the power outage event.


## Data Cleaning 

To start, one thing I wanted to do was to do some feature engineering by combining the time of day and time of the year/month/ to make a proper outage start time and outage restoration time - each containing the year, month, day, hour, minute, and second of the outage start and end. 

### Imputation
In order to do this, I identified the missing values in the outage start and end date, and chose to drop them from the dataset. This removed 9 rows from the 1476 starting amount, which was only 0.61% of the data, which is fine by me.

### Feature Engineering
From there, I combined the features 'OUTAGE.START.DATE' and 'OUTAGE.START.TIME' to make 'OUTAGE.START' and combined 'OUTAGE.RESTORATION.DATE' and 'OUTAGE.RESTORATION.TIME' to make 'OUTAGE.RESTORED'.

These two new features are added to my working list of features from the outages dataset to get a subset that looks something like this:


| U.S._STATE   | NERC.REGION   | CLIMATE.CATEGORY   | CAUSE.CATEGORY     |   YEAR |   MONTH |   OUTAGE.DURATION |   TOTAL.SALES |   TOTAL.CUSTOMERS |   ANOMALY.LEVEL | OUTAGE.START        | OUTAGE.RESTORED     |   TOTAL.PRICE |
|:-------------|:--------------|:-------------------|:-------------------|-------:|--------:|------------------:|--------------:|------------------:|----------------:|:--------------------|:--------------------|--------------:|
| Minnesota    | MRO           | normal             | severe weather     |   2011 |       7 |              3060 |       6562520 |       2.5957e+06  |            -0.3 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00 |          9.28 |
| Minnesota    | MRO           | normal             | intentional attack |   2014 |       5 |                 1 |       5284231 |       2.64074e+06 |            -0.1 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00 |          9.28 |
| Minnesota    | MRO           | cold               | severe weather     |   2010 |      10 |              3000 |       5222116 |       2.5869e+06  |            -1.5 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00 |          8.15 |
| Minnesota    | MRO           | normal             | severe weather     |   2012 |       6 |              2550 |       5787064 |       2.60681e+06 |            -0.1 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00 |          9.19 |
| Minnesota    | MRO           | warm               | severe weather     |   2015 |       7 |              1740 |       5970339 |       2.67353e+06 |             1.2 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00 |         10.43 |

## Exploratory Data Analysis

To start, I decided to investigate the outage durations on a monthly and yearly basis. Below is a table containing the total number of minutes in outage duration from each month between 2000-2016, with the total duration of outages summed to in the right-most column:

Below is a table containing the **total sum of power outage durations (in hours) on a "per month per year" basis**:

|   Year |     Jan |    Feb |     Mar |    April |   May |     June |    July |     Aug |    Sept |     Oct |    Nov |   Dec | 
|-------:|--------:|--------:|--------:|-------:|-------:|--------:|--------:|--------:|--------:|--------:|-------:|--------:|
|   2000 |  234    |    0    |    1.17 |   0    | 243.33 |    1.1  |    0    |   82    |    0    |    0    |   0    |   54.4  |
|   2001 |   64.83 |    0    |   42.57 |   0    |   5.38 |  180.02 |    0    |    4.02 |    0    |    0    |   0    |    0    |
|   2002 |  455    |    0.78 |   60    |   0    |   0    |    0    |    0    |    9.43 |    0    |    0    | 184    |  399.35 |
|   2003 |   24    |   56.47 |    0    | 128.23 | 780.62 |   25.8  |  413.65 |  458.42 |  494.9  |  573.17 | 345.68 |  265.93 |
|   2004 |  367.35 |  109.02 |  298.95 | 108.17 | 688    |  274.27 | 1823.9  |  309.43 |  840.5  |  237.52 |  21    |   91.63 |
|   2005 |  795.83 |    0    |    4    | 240.13 |  25.5  |  456.27 |  274.58 |  812.17 | 1307.28 |  443    | 235.2  |  166.08 |
|   2006 |   53.05 |  545.53 |   39.5  |  95.42 |  13.37 |  101.45 |  665.45 |   12    |  165.5  |  591.77 | 258.58 | 1120.87 |
|   2007 |  186.5  |  187.68 |   11.5  | 172.63 | 162.95 |   14.38 |  293.65 |  307.03 |   59.88 |  137.87 |   0    |  568.92 |
|   2008 |  410.65 |  532.42 |   44.73 |  93.25 | 242.18 | 1026.67 |  531.38 |  738.1  | 3089.87 |   51    |  37.85 |  872.6  |
|   2009 | 1651.58 |  234.63 | 1388.8  | 219.83 | 178.2  |  438.22 |  137.95 |  159.53 |    0    |   76.27 |  15.75 |  196.9  |
|   2010 |  541.27 |  912.25 |  616.32 |  52.67 |  14.52 |  986.55 |  454.17 |  559.13 |  184.78 |  241.75 | 181.78 |  444.45 |
|   2011 |  347.85 |  608.18 |  645.08 | 615.43 | 743.87 |  330.02 |  602.35 | 2076.92 |  273.32 | 1462.47 |  42.57 |  329.15 |
|   2012 |   80.9  |   66.98 |  145.98 | 248.23 |  71.25 | 1165.87 |  762.47 |  167.35 |   58.47 | 2529.17 |  27.75 |   90.42 |
|   2013 |  158.88 |  229.08 |   36.02 | 158.87 | 232.58 |  393.08 |  271.63 |  108.05 |  103.33 |   38.32 | 512.13 | 1226.57 |
|   2014 | 2085.82 | 1901.15 |  808.42 | 257.55 | 111.3  |  221.85 |    0    |    0    |    0    |    0    |   0    |    0    |
|   2015 |   17.13 |   74.17 |   31.98 | 167.07 | 159.93 |  424.23 |  137.53 |  308.08 |    7.1  |   99.4  | 125.08 |  101.55 |
|   2016 |   35.3  |   35.37 |  941.55 | 106.57 | 446.98 |    0.28 |    0    |    0    |    0    |    0    |   0    |    0    |

Here is an additional table showing the **average duration of power outages of each month (in hours)**:

|     Jan |    Feb |     Mar |    April |   May |     June |    July |     Aug |    Sept |     Oct |    Nov |   Dec |
|--------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|
| 56.4658 | 41.6191 | 54.4316 | 24.8977 | 34.6216 | 32.4734 | 38.5983 | 40.4746 | 71.5754 | 60.0156 | 28.8027 | 54.8965 |


And the **total duration of outages each year (in hours)**:

|   2000 |    2001 |    2002 |    2003 |    2004 |    2005 |    2006 |   2007 |   2008 |    2009 |    2010 |   2011 |    2012 |    2013 |    2014 |    2015 |    2016 |
|-------:|--------:|--------:|--------:|--------:|--------:|--------:|-------:|-------:|--------:|--------:|-------:|--------:|--------:|--------:|--------:|--------:|
|    616 | 296.817 | 1108.57 | 3566.87 | 5169.73 | 4760.05 | 3662.48 |   2103 | 7670.7 | 4697.67 | 5189.63 | 8077.2 | 5414.83 | 3468.55 | 5386.08 | 1653.27 | 1566.05 |

These tables allow us to look and identify any trends in the outages from the entirety of our population. For starters, I identified the year with the longest power outage durations to be 2011. This allowed me to focus on 2011 to see if I can identify any distributions or patterns in the data by looking at the largest sub-sample within my data.

Below is a histogram showing the distribution of power outages on a monthly basis - a similar collection of data as our tables from earlier, but this time a bit more visual.

 <iframe
    src="assets/hist.html"
    width="800"
    height="600"
    frameborder="0"
    
 >
 <figcaption align="center"><em>Graph of monthly power outages in 2011</em></figcaption>
 </iframe>

An interesting observation is that the distribution of outages in this visualization could possibly be normally distributed, or at least looks like it, with a peak reported number of outages in August of 2011.

 <iframe
 src="assets/map.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

Continuing on the same vein as earlier, I created a choropleth map of the United States, visualizing the number of outages between the years of 2000 and 2016. The goal of this visualization was to see if there were certain regions that outages were more common in. As you can see, there **does not seem to be a clear focus of outages in certain NERC regions**, but there seems to be specifically a higher amount of outages in California, Texas, Michigan, and Washington.

## Framing a Prediction Problem

Looking having spent a lot of time looking at outage durations, I was sure I wanted to focus on that as a predictor for whatever type of regression or classification problem I wanted to look at. At this point, I had developed a short list of ideas I had:


### Classification Ideas
- Predict the severity (in terms of number of customers, duration, or demand loss) of a major power outage: This is a multiclass classification problem, with options for the classes being:
    1. Heavy outage
    2. Medium outage
    3. Light outage

### Regression Ideas
- Predict the Oceanic Niño Index (ONI) -- or ANOMALY.LEVEL based on the number of customers, start/end date, etc...
- Predict the Average monthly electricity price in the U.S. state (TOTAL.PRICE)
 
 ---

Looking at the handful of options I created, I decided to focus on regression. Classification sounded very intersting, but with an interest in looking at the financial side of this dataset, I found myself more drawn to building a regression model. As a result, I chose to predict the average monthly electricity price in the U.S. state!

### Final Decision
Predict the Average monthly electricity price in the U.S. state (TOTAL.PRICE)

## Step 4: Baseline Model

## Step 5: Final Model
