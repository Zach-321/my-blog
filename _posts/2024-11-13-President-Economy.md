---
layout: post
title:  "Presidents and the Economy"
author: Zach Perry
description: "Which political party in the White House is better for the economy?" 
image:
---

I don't know if you knew this, but the election just happened. During every election, media organizations conduct exit polls to learn what type of people voted for each candidate and why. One of the most interesting questions asked in these polls is "Who do you trust to run the economy better?" In [2024](https://www.nbcnews.com/politics/2024-elections/exit-polls) 52% of voters answered Trump, and 46% answered Harris. In [2020](https://www.cnn.com/election/2020/exit-polls/president/national-results) it was a 49-49 tie between Trump and Biden. And in [2016](https://www.cnn.com/election/2016/results/exit-polls) it was 48-46 in favor of Trump over Clinton. So, just going back 3 presidential elections it seems that voters slightly favor Republican candidates when it comes to the economy. My question is: Are they right? Does historical data agree that Republican presidents are better for the economy them Democrats?

## Data
To test this I decided to gather data from the [Federal Reserve Economic Data (FRED) API](https://fred.stlouisfed.org/docs/api/fred/). I chose several different 'economic benchmarks' that I will use to judge the health of the economy. Gathering the data from FRED was pretty simple. Its well documented and easy to use. My API requests looked something like:
```{python}
# import necessary packages
import pandas as pd
import requests

# read in API key
with open('api.txt', 'r') as file:
    api_key = file.read()

# create list of series IDs that correspond to benchmarks
ids = ['FEDFUNDS', 'CPIAUCSL', 'PAYEMS', 'UNRATE', 'LNS11300002', 'GDP', 
       'CIVPART', 'A939RX0Q048SBEA', 'CES0500000003']

i = 0
while i < (len(ids)): # initialize while loop to API request for each ID
    url = f"https://api.stlouisfed.org/fred/series/observations?series_id={ids[i]}&api_key={api_key}&file_type=json"
    response = requests.get(url)

    data = response.json() 
    variable = pd.DataFrame(data['observations']) # turn requested data into a dataframe
    variable.drop(['realtime_start', 'realtime_end'],axis = 1, inplace = True)
    variable.columns = ['Date', ids[i]] #drop and rename columns

    if i == 5: # The first 4 values of the GDP ID are just '.' so they need to be removed
        variable = variable.iloc[4:]

    if i == 0: # create dataframe for first variable
        df = pd.DataFrame(variable)
    else: # add all other variables to created DataFrame
        df = df.merge(variable, on = 'Date', how = 'outer')
    i += 1

# rename columns to proper names
df.columns = ['Date','Fin_Market_Interest_Rates', 'CPI', 'Num_Workers(thousands)','Unemployment_Rate',
'Work_Partic_Rate_Women', 'GDP(billions)', 'Work_Partic_Rate_All', 'Real_GDP_per_Capita', 'Hourly_Wages_Private($)']

#change data types to correct ones
df = df.astype({'Fin_Market_Interest_Rates': float, 'CPI': float, 'Num_Workers(thousands)': int, 
                'Unemployment_Rate' : float,'Work_Partic_Rate_Women' : float, 'GDP(billions)': float, 
                 'Work_Partic_Rate_All' : float, 'Real_GDP_per_Capita' : float, 'Hourly_Wages_Private($)' : float})
df['Date'] = pd.to_datetime(df['Date'], errors='coerce')
```
That might look like a lot, but its really just a big while loop that makes the API request for a benchmark, then cleans the data and adds it to a master data frame and repeats. 

For more info on the variables I chose, check out [my repo](https://github.com/Zach-321/Data_Curation_Repo)

## Analysis

![CPI](https://github.com/user-attachments/assets/bc4db114-b828-4f9b-b0cb-6f88ff75efe7)
