# Can Power Outages Predict Your Monthly Electricity Bill?

## Introduction

This dataset was collected by Purdue University. It contains various pieces of information about power outages, electricity consumption patterns, and other pieces of information about the United States. What makes this dataset, and this analysis very interesting, is to see whether we can predict the monthly price (per kilowatt-hour) of our electricity, given a variety of parameters, like state, outage start and end time, month, year, etc...

The main point of this analysis is to take a look at how the world around us impacts our lives in ways we might not really think about. The total number of rows in the dataset are 1476, with a total of 54 columns as well. However, we won't be using all of those columns to predict our variable: 'TOTAL.PRICE' - Average monthly electricity price in the U.S. state (cents/kilowatt-hour).

Out of all the columns, the numerical ones that we will be focusing on are: 

**- Year:** e.g. 2011, 2014.

**- Month:** e.g. January = 1 up to December = 12.

**- Outage Duration:** Duration of outage, in minutes.

**- Total Sales:** Total electricity consumption in the U.S. state (megawatt-hour).

**- Total Customers:** 	Annual number of total customers served in the U.S. state.

**- Anomaly Level:** This represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season - essentially indicators for the environmental climate.

And the categorical ones are:

**- US State:** The state in which the outage occurs in.

**- Nerc Region:** Regions in North America created by "The North American Electric Reliability Corporation" that were involved in the outage event.

**- Climate Category:** This represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI).

**- Cause Category:** Cause of the power outage event.


![Map of NERC Regions of North America](assets/imgs/Nerc_Regions_Map.jpg)



## Data Cleaning and Exploratory Data Analysis
