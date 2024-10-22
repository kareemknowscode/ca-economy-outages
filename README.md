# Analyzing the Impacts of Power Outages on California's Economy: Identifying Root Causes

Authors: Kareem Mazboudi, Mariana Paco

This is a data science project that aims to investigate the correlation between power outages and fluctuations in a state's gross state product (GSP) within California. The dataset utilized for this investigation can be accessed via the following source. Our findings are presented in this report, which was completed as part of the DSC80 course at UCSD. This project is a two-part analysis with the second part found [here](https://kareemknowscode.github.io/outage-severity-model/).

## Introduction

In this project, we want to answer whether or not the occurrence of severe weather events impacts power outages in California, and to what extent do these outages contribute to the overall loss in production output, as measured by the GSP of California? The objective is to conduct exploratory data analysis using detailed information on severe weather incidents to assess the severity of power outages and their economic implications on the state's productivity. The dataset we used contains information on power outages recorded in different areas in the United States. Along with data on major outages, the data contains information describing the geographical features, climate, land use, electricity consumption, and economic characteristics of the area or state in which the outage occurred. The original dataset contained 57 columns containing the previously listed data, and 1535 rows of observations for outages in each US state. For our purposes, we removed all variables not pertaining to our research focus and worked with only 40 variables out of the original 57. 

### Important columns 

|*Variables*				|*Description*|
| --- 						|---|
|`'YEAR'` 					|Year that the outage occurred|
|`'MONTH'`					|Month that the outage occurred|
|`'ANOMALY.LEVEL'`			|Oceanic Niño Index. Values of +0.5 or higher indicate El Niño. Values of -0.5 or lower indicate La Niña.|
|`'CLIMATE.CATEGORY'` 		|Categorical data describing the climate at the time of the outage|
|`'OUTAGE.START.DATE'`  	|Day, month, and year of when exactly the outage began|
|`'OUTAGE.RESTORATION.DATE'`|Day, month, and year of when exactly the outage was fixed|
|`'CAUSE.CATEGORY'` 		|Categorical data describing the cause of the outage. E.g. “severe weather”|
|`'CAUSE.CATEGORY.DETAIL'` 	|Details as to the cause of the outage. E.g. “heavy wind”|
|`'OUTAGE.DURATION'` 		|The length of the outage in minutes.|
|`'PC.REALGSP.STATE'` 		|Per capita real GSP of a given state (adj. for inflation, 2009 chained $USD)|

`‘CAUSE.CATEGORY’` was key in our analysis as it contained information on what caused any given outage and `‘CAUSE.CATEGORY.DETAIL’` described the exact occurrence of that particular cause. We were mostly interested in severe weather from the `‘CAUSE.CATEGORY’` column, and `‘CAUSE.CATEGORY.DETAIL’` described the type of severe weather that occurred. `'PC.REALGSP.STATE'` contains the GSP (inflation adjusted) of a given state in any given year.

---
## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
To heighten the readability and precision of our data, we executed a series of measures to tidy up our DataFrame. A description of each original column can be found in this article.

1. **Create a copy of the Excel original data file and delete columns that were not significant for our analysis** 

	Geographic area columns such as `‘U.S._STATE’`, `‘POSTAL.CODE’`, and `‘NERC.REGION’` were deleted as we are working with one state only, California.
	Anything related to electricity prices and consumption, population, and land area is removed.


2. **Import the Excel sheet to our notebook, remove ‘variables’ column and the first row of DataFrame**

	The `‘variables’` column is removed, since each column type was checked later on with Python.
	The first row of DataFrame is deleted as it contains units for each column not needed for our analysis.

3. **Convert columns into correct types**

	This step was made to provide accuracy for future calculations. For instance, the columns `‘OUTAGE.START.DATE’` and `‘OUTAGE.RESTORATION.DATE’` were converted into DateTime type. `‘YEAR’` and `‘MONTH’` were converted to int type. `‘ANOMALY.LEVEL’` and `‘PC.REALGSP.STATE’` are converted to float type.

4. **Check the type of all columns. Find a table with the final types below**

	|*Variables* 							|*Data Type*|
	|---									|---|
	|`'YEAR'`                               |*int64*|
	|`'MONTH'`                              |*int64*|
	|`'ANOMALY.LEVEL'`                      |*float64*|
	|`'CLIMATE.CATEGORY'`                   |*object*|
	|`'OUTAGE.START.DATE'`          		|*datetime64[ns]*|
	|`'OUTAGE.RESTORATION.DATE'`    		|*datetime64[ns]*|
	|`'CAUSE.CATEGORY'`                     |*object*|
	|`'CAUSE.CATEGORY.DETAIL'`              |*object*|
	|`'OUTAGE.DURATION'`                   	|*float64*|
	|`'PC.REALGSP.STATE'`                  	|*float64*|

Here is the head of our clean DataFrame:

|   YEAR |   MONTH |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | OUTAGE.START.DATE   | OUTAGE.RESTORATION.DATE   |
|-------:|--------:|----------------:|:-------------------|:--------------------|:--------------------------|
|   2007 |       9 |            -0.9 | cold               | 2007-09-04 00:00:00 | 2007-09-04 00:00:00       |
|   2008 |       5 |            -0.7 | cold               | 2008-05-08 00:00:00 | 2008-05-08 00:00:00       |
|   2006 |       5 |             0   | normal             | 2006-05-19 00:00:00 | 2006-05-19 00:00:00       |
|   2015 |      10 |             2   | warm               | 2015-10-13 00:00:00 | 2015-10-13 00:00:00       |
|   2014 |       2 |            -0.5 | cold               | 2014-02-06 00:00:00 | 2014-02-06 00:00:00       |

| CAUSE.CATEGORY                | CAUSE.CATEGORY.DETAIL   | OUTAGE.DURATION | PC.REALGSP.STATE  | OUTAGE.DURATION.HOURS  | Anomaly |
|:-----------------------------:|:-----------------------:|:---------------:|:-----------------:|:----------------------:|:-------:|
| severe weather                | heatwave                | 420             | 55008             | 7                      | La Niña |
| system operability disruption | nan                     | 155             | 54713             | 2.58333                | La Niña |
| severe weather                | thunderstorm            | 437             | 54508             | 7.28333                | Normal  |
| public appeal                 | nan                     | 247             | 56365             | 4.11667                | El Niño |
| fuel supply emergency         | Natural Gas             | 540             | 54606             | 9                      | La Niña |



### Performing Univariate Analysis

Since we were interested in the different causes of power outages, we proceeded to investigate markers of severe weather patterns by using the
`‘ANOMALY.LEVEL’` column.

<iframe src="assets/plots/univariate plots/univariate-distr-anomaly-levels.html" width=600 height=400 frameBorder=0></iframe>

This indicates that the anomaly levels' distribution is slightly right-skewed. One could suggest that the graph's center lies within the range of -0.5 to 0. This interval pertains to the average temperature anomaly and not to the El Niño or La Niña phenomena.
We visualized the count of power outages per cause category to identify the most common ones.

<iframe src="assets/plots/univariate plots/univariate-outage-count-cause-category.html" width=600 height=400 frameBorder=0></iframe>

After analyzing the data, it became evident that severe weather and system operability disruption were the most frequent causes.

### Performing Bivariate Analysis and Aggregation

Then, we performed a bivariate analysis between outage duration and anomaly levels to investigate the relationship between longer outages and weather using the indicator of anomaly levels.

<iframe src="assets/plots/bivariate plots/bivariate-outage-dir-scatter.html" width=600 height=400 frameBorder=0></iframe>

We can see that there are many outliers during normal and non-normal anomaly levels. Thus, we cannot confirm that longer outages are caused by severe weather under the index of anomaly levels.

### Interesting Aggregates

For our aggregate analysis, we calculated the average outage duration by climate category (cold, normal, and warm). Look at our grouped table below:

| CLIMATE.CATEGORY | OUTAGE.DURATION |
|:-----------------:|-----------------:|
| cold              |         1787.67 |
| normal            |         1273.5  |
| warm              |         2117.45 |

<iframe src="assets/plots/interesting aggregates plots/intr-agg-grouped-plot.html" width=600 height=400 frameBorder=0></iframe>

It is observed that longer outages tend to happen more frequently during warmer temperatures, which can be attributed to the common occurrences of wildfires in California during the hotter seasons.

Next, we examined the different categories of causes and the total amount of time that outages lasted each year using a pivot table. See below the head of the mentioned table:

| equipment failure | fuel supply emergency | intentional attack | islanding | public appeal | severe weather | system operability disruption |
|-------------------:|----------------------:|---------------------:|----------:|--------------:|---------------:|-------------------------------:|
|                  0 |                     0 |                    0 |         0 |              0 |              0 |                              0 |
|                  0 |                     0 |                    0 |         0 |            769 |              0 |                           4455 |
|                  0 |                     0 |                    0 |         0 |              0 |          14760 |                            383 |
|               2039 |                     0 |                    0 |         0 |              0 |          41021 |                              0 |
|                 64 |                     0 |                    0 |         0 |              0 |          10655 |                            194 |

<iframe src="assets/plots/interesting aggregates plots/intr-agg-pivot-plot.html" width=600 height=400 frameBorder=0></iframe>

Our plot shows the trends in outage duration per cause category from 2000 to 2016. We can see again that severe weather is the leading cause of power outages accounting for most of the total duration each year. The total outage duration has seen noticeable rises in certain years, particularly during the severe weather events that coincided with the Cedar Fire in 2003 and a major storm in 2011.

---
## Assessment of Missingness

### NMAR analysis

Based on our dataset, it seems that the `‘OUTAGE.RESTORATION.DATE’` column is NMAR (Not Missing At Random). This column provides information on the power outage end date, but we have noticed that it contains NaN values that are consistent for every outage in the dataset. We speculate that there could be various reasons for this missing data, including instances where data was not recorded for minor outages that were not considered significant enough to be included in the dataset. Additionally, a power outage may not have ended officially, which could contribute to the missingness. In fact, we believe that this missingness could be explained as MAR (Missing At Random) if we could obtain specific geographic data for any given power outage, such as the city that which the outage occurred or even the location of the power station that went down. Through the geographic data, we could perform a permutation test to determine whether or not the missingness could be attributed to any of the reasons listed beforehand.

### Missingness Dependency

We want to focus on the missingness of two very important columns in terms of our analysis. In the `‘CAUSE.CATEGORY.DETAIL’` column, there are some NaN values we discovered when filtering for values in California. From this, we hypothesized that these values might be missing for a multitude of reasons. For example, at the time of the creation of the dataset, the researcher may not have been able to include the details of an outage caused by something, such as severe weather. For instance, when the dataset was being created, the researcher might not have had enough knowledge to include the specifics of a power outage caused by severe weather. Factors like the type of weather leading to the disruption or other possible causes of the outage could have been overlooked. Given this logic, the data in this variable is NMAR. If we wanted more information to understand the missingness, we would need data on something like the season at the time of the outage and the region of occurrence, the latter of which is what we have access to, but only at a general level. With this data, we could likely find evidence and reason why our data is missing, thus making the data MAR. Unfortunately, since we do not have this data, we must perform a permutation test. 

To test the missingness of the `‘CAUSE.CATEGORY.DETAIL’` column, we hypothesized that the missingness might be dependent on either its parent column, the `‘CAUSE.CATEGORY’` column, or the `‘CLIMATE.CATEGORY’` column. Our hypotheses for the first test are as follows:

#### ‘CAUSE.CATEGORY.DETAIL’  and ‘CAUSE.CATEGORY’ 

**Null**: The distribution of `‘CAUSE.CATEGORY’` when `‘CAUSE.CATEGORY.DETAIL’` is missing is the same as the distribution of `‘CAUSE.CATEGORY’` when  `‘CAUSE.CATEGORY.DETAIL’` is not missing.

**Alternative**: The distribution of `‘CAUSE.CATEGORY’` when `‘CAUSE.CATEGORY.DETAIL’` is missing is different from the distribution of `‘CAUSE.CATEGORY’` when `‘CAUSE.CATEGORY.DETAIL’` is not missing.

To test this, we plan on using the TVD to compare these two categorical distributions, and we will test at the 5% significance level.

In our permutation test, we shuffled `‘CAUSE.CATEGORY’` 1000 times.

<iframe src="assets/plots/missingness analysis plots/nmar-analysis-tvd-plot.html" width=600 height=400 frameBorder=0></iframe>

Wrapping up our test, we found that the p-value of the test  `p = 0.00099` is far below our significance level of 5%. From this, we rejected the null hypothesis, meaning that there is a high probability that the distributions we tested are different, which in turn implies that there is a high likelihood of the missing data being dependent on the `‘CAUSE.CATEGORY’` column.

#### ‘CAUSE.CATEGORY.DETAIL’  and ‘CLIMATE.CATEGORY’

**Null**: The distribution of `‘CLIMATE.CATEGORY’` when `‘CAUSE.CATEGORY.DETAIL’` is missing is the same as the distribution of  `‘CLIMATE.CATEGORY’` when  `‘CAUSE.CATEGORY.DETAIL’` is not missing.

**Alternative**: The distribution of `‘CLIMATE.CATEGORY’` when `‘CAUSE.CATEGORY.DETAIL’` is missing is different from the distribution of  `‘CLIMATE.CATEGORY’` when  `‘CAUSE.CATEGORY.DETAIL’` is not missing.

Like our previous test, we used the TVD to compare each categorical distribution at the 5% significance level.

This time, we shuffled the `’CLIMATE.CATEGORY’` column 1000 times.

<iframe src="assets/plots/missingness analysis plots/nmar-analysis-tvd2-plot.html" width=600 height=400 frameBorder=0></iframe>

For this hypothesis test, we found that the p-value of the test `p = 0.501` is greater than the significance level which we tested at. We fail to reject the null hypothesis in this case, implying a high probability that the `‘CAUSE.CATEGORY.DETAIL’` does not depend on the `‘CLIMATE.CATEGORY’` column.

---
## Hypothesis Testing

During our previous analysis, we found that severe weather was the cause of most power outages in California. Our current objective is to determine if there is a notable difference in the average GSP loss during these outages compared to those caused by other reasons in our `‘CAUSE.CATEGORY’` column.

### Setup

**Null Hypothesis/H0**: The mean GSP loss during power outages caused specifically by severe weather is equal to the mean GSP loss during power outages caused by other reasons.

**Alternative Hypothesis/H1**: The mean GSP loss during power outages caused specifically by severe weather is different from the mean GSP loss during power outages caused by other reasons.

For this test, we selected the `‘CAUSE.CATEGORY’` and `‘PC.REALGSP.STATE’` columns. When performing this two-sided test, we wanted to take into consideration any form of outage causation and the real GSP of California itself, not including the rest of the United States. The test statistic we used was the absolute difference in means, to try and capture any change between the two groups we selected. 
We decided to use a permutation test to account for variability in the data and assess whether or not our observed difference in means was real or random, and we will test at the 5% significance level.

The absolute difference in means that we observed was `60.87142857143044`

### Testing

<iframe src="assets/plots/hypothesis test plots/hyp-test-distr-plot.html" width=600 height=400 frameBorder=0></iframe>

The graph above shows our observed difference in means relative to the distribution of our simulated test results.

### Conclusion
We conclude that at the 5% significance level, given `p = 0.854`, we are unable to reject our null hypothesis. From this, we can conclude that there may be no significant difference between the average loss in GSP for outages caused by severe weather versus outages caused by other reasons. The strong evidence in favor of our null hypothesis suggests that any change in real GSP for California just might be utterly random in terms of reasons for an outage.
