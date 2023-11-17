# ca-economy-outages

Analyzing the Impacts of Power Outages on California's Economy: Identifying Root Causes
Authors: Kareem Mazboudi, Mariana Paco

Project Overview
This is a data science project and aims to investigate the correlation between power outages and fluctuations in a state's gross state product (GSP) within California. The dataset utilized for this investigation can be accessed via the following source. Our findings are presented in this report, which was completed as part of the DSC80 course at UCSD.

Introduction
In this project, we want to answer whether or not the occurrence of severe weather events impacts power outages in California, and to what extent do these outages contribute to the overall loss in production output, as measured by the GSP of California? The objective is to conduct exploratory data analysis using detailed information on severe weather incidents to assess the severity of power outages and their economic implications on the state's productivity. The dataset we used contains information on power outages recorded in different areas in the United States. Along with data on major outages, the data contains information describing the geographical features, climate, land use, electricity consumption, and economic characteristics of the area or state in which the outage occurred. The original dataset contained 57 columns containing the previously listed data, and 1535 rows of observations for outages in each US state. For our purposes, we removed all variables not pertaining to our research focus and worked with only 40 variables out of the original 57. 

Important columns 
'YEAR': Year that the outage occurred 
'MONTH': Month that the outage occurred
'ANOMALY.LEVEL': Oceanic Niño Index. Values of +0.5 or higher indicate El Niño. Values of -0.5 or lower indicate La Niña.
'CLIMATE.CATEGORY': Categorical data describing the climate at the time of the outage
'OUTAGE.START.DATE' : Day, month, and year of when exactly the outage began
'OUTAGE.RESTORATION.DATE': Day, month, and year of when exactly the outage was fixed
'CAUSE.CATEGORY': Categorical data describing the cause of the outage. E.g. “severe weather”
'CAUSE.CATEGORY.DETAIL': Details as to the cause of the outage. E.g. “heavy wind”
'OUTAGE.DURATION': The length of the outage in minutes.
'PC.REALGSP.STATE': Per capita real GSP of a given state (adj. for inflation, 2009 chained $USD)

Our study relied mostly on the following columns. ‘CAUSE.CATEGORY’ was key in our analysis as it contained information on what caused any given outage and ‘CAUSE.CATEGORY.DETAIL’  described the exact occurrence of that particular cause. We were mostly interested in severe weather from the ‘CAUSE.CATEGORY’ column, and ‘CAUSE.CATEGORY.DETAIL’ described the type of severe weather that occurred. 'PC.REALGSP.STATE' contains the GSP (inflation adjusted) of a given state in any given year.
