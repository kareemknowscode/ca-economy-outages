# Analyzing the Impacts of Power Outages on California's Economy: Identifying Root Causes

Authors: Kareem Mazboudi, Mariana Paco

This is a data science project that aims to investigate the correlation between power outages and fluctuations in a state's gross state product (GSP) within California. The dataset utilized for this investigation can be accessed via the following source. Our findings are presented in this report, which was completed as part of the DSC80 course at UCSD.

## Introduction
In this project, we want to answer whether or not the occurrence of severe weather events impacts power outages in California, and to what extent do these outages contribute to the overall loss in production output, as measured by the GSP of California? The objective is to conduct exploratory data analysis using detailed information on severe weather incidents to assess the severity of power outages and their economic implications on the state's productivity. The dataset we used contains information on power outages recorded in different areas in the United States. Along with data on major outages, the data contains information describing the geographical features, climate, land use, electricity consumption, and economic characteristics of the area or state in which the outage occurred. The original dataset contained 57 columns containing the previously listed data, and 1535 rows of observations for outages in each US state. For our purposes, we removed all variables not pertaining to our research focus and worked with only 40 variables out of the original 57. 

### Important columns 

| Variables					|Description|
| --- 						|---|
|'YEAR' 					|Year that the outage occurred|
|'MONTH' 					|Month that the outage occurred|
|'ANOMALY.LEVEL' 			|Oceanic Niño Index. Values of +0.5 or higher indicate El Niño. Values of -0.5 or lower indicate La Niña.|
|'CLIMATE.CATEGORY' 		|Categorical data describing the climate at the time of the outage|
|'OUTAGE.START.DATE'  		|Day, month, and year of when exactly the outage began|
|'OUTAGE.RESTORATION.DATE'  |Day, month, and year of when exactly the outage was fixed|
|'CAUSE.CATEGORY' 			|Categorical data describing the cause of the outage. E.g. “severe weather”|
|'CAUSE.CATEGORY.DETAIL' 	|Details as to the cause of the outage. E.g. “heavy wind”|
|'OUTAGE.DURATION' 			|The length of the outage in minutes.|
|'PC.REALGSP.STATE' 		|Per capita real GSP of a given state (adj. for inflation, 2009 chained $USD)|

‘CAUSE.CATEGORY’ was key in our analysis as it contained information on what caused any given outage and ‘CAUSE.CATEGORY.DETAIL’  described the exact occurrence of that particular cause. We were mostly interested in severe weather from the ‘CAUSE.CATEGORY’ column, and ‘CAUSE.CATEGORY.DETAIL’ described the type of severe weather that occurred. 'PC.REALGSP.STATE' contains the GSP (inflation adjusted) of a given state in any given year.

---
## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
To heighten the readability and precision of our data, we executed a series of measures to tidy up our DataFrame. A description of each original column can be found in this article.

1. **Create a copy of the Excel original data file and delete columns that were not significant for our analysis** 

Geographic area columns such as ‘U.S._STATE’, ‘POSTAL.CODE’, and ‘NERC.REGION’ were deleted as we are working with one state only, California.
Anything related to electricity prices and consumption, population, and land area is removed.


2. **Import the Excel sheet to our notebook, remove ‘variables’ column and the first row of DataFrame**

The ‘variables’ column is removed, since each column type was checked later on with Python.
The first row of DataFrame is deleted as it contains units for each column not needed for our analysis.

3. **Convert columns into correct types**

This step was made to provide accuracy for future calculations. For instance, the columns ‘OUTAGE.START.DATE’ and ‘OUTAGE.RESTORATION.DATE’ were converted into DateTime type. ‘YEAR’ and ‘MONTH’ were converted to int type. ‘ANOMALY.LEVEL’ and ‘PC.REALGSP.STATE’ are converted to float type.

4. **Check the type of all columns. Find a table with the final types below**

|Variables 							|Data Type|
|---								|---|
|'YEAR'                               |int64|
|'MONTH'                              |int64|
|'ANOMALY.LEVEL'                     	|float64|
|'CLIMATE.CATEGORY'                   |object|
|'OUTAGE.START.DATE'          		|datetime64[ns]|
|'OUTAGE.RESTORATION.DATE'    		|datetime64[ns]|
|'CAUSE.CATEGORY'                     |object|
|'CAUSE.CATEGORY.DETAIL'              |object|
|'OUTAGE.DURATION'                   	|float64|
|'PC.REALGSP.STATE'                  	|float64|

Here is the head of our clean DataFrame:
 **INSERT DF HERE**

### Performing Univariate Analysis

Since we were interested in the different causes of power outages, we proceeded to investigate markers of severe weather patterns by using the ‘ANOMALY.LEVEL’ column.

**#Insert graph**

This indicates that the anomaly levels' distribution is slightly right-skewed. One could suggest that the graph's center lies within the range of -0.5 to 0. This interval pertains to the average temperature anomaly and not to the El Niño or La Niña phenomena.
We visualized the count of power outages per cause category to identify the most common ones.

**#Insert graph**

After analyzing the data, it became evident that severe weather and system operability disruption were the most frequent causes.

### Performing Bivariate Analysis and Aggregation

Then, we performed a bivariate analysis between outage duration and anomaly levels to investigate the relationship between longer outages and weather using the indicator of anomaly levels.

**#Insert graph**

We can see that there are many outliers during normal and non-normal anomaly levels. Thus, we cannot confirm that longer outages are caused by severe weather under the index of anomaly levels.

### Interesting Aggregates

For our aggregate analysis, we calculated the average outage duration by climate category (cold, normal, and warm). Look at our grouped table below:

**#Insert table**

**#Insert plot**

It is observed that longer outages tend to happen more frequently during warmer temperatures, which can be attributed to the common occurrences of wildfires in California during the hotter seasons.

Next, we examined the different categories of causes and the total amount of time that outages lasted each year using a pivot table. See below the head of the mentioned table:

**#Insert table**

**#Insert plot**

Our plot shows the trends in outage duration per cause category from 2000 to 2016. We can see again that severe weather is the leading cause of power outages accounting for most of the total duration each year. The total outage duration has seen noticeable rises in certain years, particularly during the severe weather events that coincided with the Cedar Fire in 2003 and a major storm in 2011.
