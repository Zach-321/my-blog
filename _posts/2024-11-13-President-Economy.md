---
layout: post
title:  "Presidents and the Economy"
author: Zach Perry
description: "Which political party in the White House is better for the economy?" 
image: /assets/images/president.jpg
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

### CPI
![CPI](https://github.com/user-attachments/assets/bc4db114-b828-4f9b-b0cb-6f88ff75efe7)

The vertical lines represent when a new party is elected to the White House and the color represents which party. Red is Republican and blue is Democrat.

CPI is a measure of inflation so a lower value is better. It starts pretty flat before starting to increase faster with the election of Nixon (R) and continued to increase at a faster rate through Carter (D) before slowing down. It has been increasing at a constant rate since about Reagan (R) in 1980. Recently it has spiked again under Biden (D). 

CPI does not show one party being better than the other. There has recently been a big spike under a Democratic president, but that is in line with worldwide inflation so it doesn't seem to be a result of policy.


### Unemployment
![unemploy](https://github.com/user-attachments/assets/dc3dbd84-43e5-4e71-a02b-2742c122c4f9)

This benchmark seems a lot more telling. There is a clear trend of spikes in unemployment under Republicans and big drops under Democrats. This is most evident in the massive increase in unemployment right at the end of W. Bush's (R) presidency in 2008, then it steadily decreasing under Obama (D). Also, the 4 highest peaks in unemployment were all under Republican presidents. Democrats definitely win on unemployment.

### Workforce Participation
![workforce](https://github.com/user-attachments/assets/7791143d-d49f-4721-b149-b5961f48633a)

The workforce is defined as all employed people and all unemployed people who are currently seeking work. This graph shows workforce participation rate for all Americans and specifically for women. Women's participation rate is well below the total rate, but had been increasing at a faster rate until W. Bush (R) in 2001 where it levels off and has decreased slightly since then. The total rate also starts to decline at the same time. This benchmark is also pretty even between the two parties.

### Federal Funds Rate
![FedFunds](https://github.com/user-attachments/assets/6558c836-ab23-44a7-9a81-e6ddb8ea2a2f)

The Federal Funds Rate is the interest rate that banks use to loan money to each other. The US Federal Reserve sets a target range for banks to operate in. A higher rate is typically indicative of high inflation and high unemployment. So lower is generally better. The early 1980s under Reagan (R) had record high unemployment, which was why the rate was so high. There are other big spikes under Carter (D) and Ford (R), but the rest of the graph is not too bad. This benchmark also appears to be pretty even, but I think I would give a slight edge to Democrats.

### Real GDP Per Capita
![realGDP](https://github.com/user-attachments/assets/74f866b6-849a-49c9-8f3d-729bb77eb81e)

This is the big one. GDP stands for Gross Domestic Product, the total value of all goods produced in the US. Real GDP means that it is in 2017 USD to account for inflation and per capita accounts for changes in population. 

This graph shows pretty consistent growth through all presidents. However, the three biggest dips all happened under Republicans. The huge drop under Trump (R) was caused by COVID and GDP quickly recovered. The other two drops under Reagan (R) in 1982 and under W. Bush (R) in 2008 werenpretty small, but bigger than anything else. Those two names have come up alot in this blog, which leads me to believe that those two presidents were not great for the economy. It also looks like real GDP has been growing faster under Biden (D) then ever before. I would give Democrats a very small edge for this benchmark.

## Conclusion

After that nice stroll through American history, lets come back to the present. At the start of this blog I said I wanted to determine if voters are justified in trusting Republicans over Democrats when it comes to the economy, and the results are kind of inconclusive. One party was clearly better than the other for only one benchmark, the rest were fairly even. Democrats are definitely better on unemployment, but beyond that not much can be said. So, I would say the voters are slightly incorrect, but they're close enough.

Isn't economic data so much fun? If you want to mess around with the FRED API you can sign up for an API key [here](https://fred.stlouisfed.org/docs/api/api_key.html) by making an account. If you want to use my data you can visit my repo, linked above and [right here](https://github.com/Zach-321/Data_Curation_Repo). Have fun!





