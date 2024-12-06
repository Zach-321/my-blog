---
layout: post
title:  "Data Analysis with Streamlit"
author: Zach Perry
description: Analyzing the relationship between the president and the economy using Streamlit
image: "/assets/images/economy.jpg"
---

When reading a data analysis blog, you, the reader, are at the mercy of me, the author. You can only see the data that I show you, only see the graphs that I make, only reach the conclusions that I illustrate. The solution to this problem is to put the controls in your hands, to let you make the decisions and draw your own conclusion. So, I made a [Streamlit](https://streamlit.io/) app, which can be accessed [here](https://presidents-economy-blogapp.streamlit.app/).

This app was built using data that I outlined in a [seperate blog post](https://zach-321.github.io/my-blog/2024/11/13/President-Economy). If you didn't read it you should. But if you don't want to, my goal was to analyze whether the president's political party has an effect on the economy. My streamlit app includes the visualizations from my previous blog, as well as some fun, new ones.

## Streamlit

My Streamlit app is split into 3 tabs; 'Time Series', which shows how a variable of your choice changed over time, 'Plots', where you chose at least one variable to see density and scatterplots by party along with a correlation matrix of all variables, and 'Summaries' where you can choose mean, median, maximum, or minimum to see that summary for all variables and when they occur.

Using  my app, you can analyze relationships between variables by political party, and check out when variables were at their highest or lowest.

## Analysis

### Time Series

I've already done a good bit of time series analysis on my other blog post, so I wont talk about it here. Please read my other [post](https://zach-321.github.io/my-blog/2024/11/13/President-Economy).

### Plots

There are two parts to this tab, comparison plots and the correlation matrix. The comparison plots lets you select any number of variables and view them individually as density plots and with each of the other variables in scatterplots. Using this, you can see that unemployment rate is generally lower under Democrats, and that is has basically no linear relationship with GDP. You can also see that there is a pretty strong negative relationship between hourly wages and workforce participation rate. The second part of this tab is the correlation matrix. I like to use the correlation matrix to find interesting relationships that can be modeled by the comparison plots. Look for strong positive and negative relationships.

### Summaries



## Conclusion

After that nice stroll through American history, lets come back to the present. At the start of this blog I said I wanted to determine if voters are justified in trusting Republicans over Democrats when it comes to the economy, and the results are kind of inconclusive. One party was clearly better than the other for only one benchmark, the rest were fairly even. Democrats are definitely better on unemployment, but beyond that not much can be said. So, I would say the voters are slightly incorrect, but they're close enough.

Isn't economic data so much fun? If you want to mess around with the FRED API you can sign up for an API key [here](https://fred.stlouisfed.org/docs/api/api_key.html) by making an account. If you want to use my data you can visit my repo, linked above and [right here](https://github.com/Zach-321/Data_Curation_Repo). Have fun!
