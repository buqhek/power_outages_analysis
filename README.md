# Can Power Outages Predict Your Monthly Electricity Bill?

## Introduction

First and foremost, I would like to acknowledge that this dataset was collected by Purdue University, located [here](https://engineering.purdue.edu/LASCI/research-data/outages). It contains various pieces of information about power outages, electricity consumption patterns, and other pieces of information about the power outages across the United States between 2000-2016. The main point of this analysis is to take a look at how the world around us impacts our lives in ways we might not really think about. 

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

To start, one thing I wanted to do was to do some feature engineering by combining the time of day and time of the year/month/ to make a proper outage start time and outage restoration time - each containing the year, month, day, hour, minute, and second of the outage start and end. In order to do this, I identified the missing values in the outage start and end date, and chose to drop them from the dataset. This removed 9 rows from the 1476 starting amount, which was only 0.61% of the data, which is fine by me.

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

During my initial time with the data, I decided to ask myself a variety of surface level questions to see if I can determine any trends that might make for interesting analysis questions when developing a model for the dataset. 

 <iframe
    src="assets/hist.html"
    width="800"
    height="600"
    frameborder="0"
 ></iframe>

To start, I quickly developed this quick histogram to see if I could identify any trends in the number of outages in particular based on the time of year (in this case, finding the total number of outages per month). The reason I chose to depict 2011 in particular from this dataset, was that 2011 had the most amount of outages, so I was hoping to utilize the fact that the year 2011 had the highest population of outages, and therefore could maybe identify any interesting trends. 

An interesting observation is that the distribution of outages in this visualization could possibly be normally distributed, or at least looks like it, with a peak reported number of outages in August of 2011.


 <iframe
 src="assets/map.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

Continuing on the same vein as earlier, I created a choropleth map of the United States, visualizing the number of outages from the population of all recorded outages from Purdue University between the years of 2000 and 2016. The goal of this visualization was to see if there were certain regions that outages were more common in. As you can see, there does not seem to be a clear trend in NERC regions (meaning that there seems to be a cluster of high-power outages in a single given NERC Region), but there seems to be a higher amount of outages in California, Texas, Michigan, and Washington.