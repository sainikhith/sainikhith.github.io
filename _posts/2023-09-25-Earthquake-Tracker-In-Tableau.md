---
layout: post
title: Earthquake Tracking Dashboard Using Tableau
description: Implementing a Tableau Dashboard that tracks global Earthquake activity across a 30-day period
image: "/posts/earthquake-map.jpg"
tags: [Tableau, Data Viz, Earthquakes]
---

# Introduction  <a name="introduction"></a>
In this project we're going to look at visualising some raw data using a tool called Tableau Public.  This is a free version of Tableau that is pretty functional.  It can consume data in CSV format, which is what we will use in this project.  Here, we will look at implementing some typical client requirements for getting insights and visualising patterns from a data-set of Earthquake incidence data.

### What is Tableau?
Tableau is a popular business intelligence and data visualization tool. Tableau is designed to help people visualize and understand data through interactive dashboards, reports, and graphs. It connects to various data sources like SQL databases, spreadsheets, and csv to analyze the data.

Some of the key capabilities and features of Tableau include:
* Drag and drop interface to easily create visualizations like charts, maps, dashboards etc. without coding.
* Support for large data sets with in-memory analytics.
* Interactive dashboards that allow users to filter, drill down, and slice and dice data.
* Built-in analytics and forecasting functions.
* Options to publish and share visualizations and dashboards online or via the Tableau server.
* Integration with R and Python for advanced analytics.
* Tableau Public for sharing public data visualizations.
* Tableau is used by businesses, organizations and researchers across many industries who need to make sense of large, complex datasets through intuitive visualizations and insights. It helps them analyze data, identify trends and make data-driven decisions.
* Some key competitors and alternatives to Tableau include Power BI, Qlik Sense, Domo, Looker, Sisense.

So in summary, Tableau is a widely used and powerful business intelligence and analytics tool for data visualization and analysis. It makes working with data and uncovering insights much easier through its highly visual and interactive capabilities.

# Project Overview  <a name="overview-main"></a>

### Context <a name="overview-context"></a>

Our client has requested an earthquake tracking dashboard to help them gain insights from 30 days worth of global Earthquake data.  Here are the requirements:

* It must have a map showing where Earthquakes took place, and this also needs to show their magnitude.
* It must have a list of the top 10 biggest earthquakes.
* It must have a breakdown of the percentage of earthquakes that occurred, by broad location.
* For more granular locations, it must show how many earthquakes took place, the average magnitude and the maximum magnitude.

We need all these elements on one dashboard so that we can easily see patterns.  They all need to be attached to the same date filter, where we can track earthquakes day by day, and have the ability to scroll through the data one day at a time.

### Actions <a name="overview-actions"></a>

* First we needed to import the supplied data into Tableau.
* Second, we built a map view, showing the locations of each of the earthquakes on a map, and using the size and colour of the circle to indicate the magnitude of the earthquake.
* Then we created a top ten largest earthquakes grid, showing their location and magnitude.
* Then we built an element showing the percentage of earthquakes sliced by broad location.
* Finally we created a sortable visualisation of earthquakes by frequency, average and maximum magnitude.
* These elements were arranged within a single dashboard, and all hooked up to a date filter so you can scroll through the data day by day over the 30 day period, or look at the data all at once. 

### Results <a name="overview-results"></a>

We have created an interactive earthquake tracking dashboard, based on the supplied dataset for earthquakes that occurred within a 30 day period.
It has all the functionality requested, on a single dashboard and hooked up to a single date control.
It has been published to Tableau Public, so you can try it out below.

The default value on the date control shows all the data from the entire 30 day range.
You can then use the date control to page through day by day and see how the data changes in all the charts.  

The response-time is a little slow, so you have to wait a couple of seconds before the data changes.  I'm assuming this is because the free Tableau Public cloud deployment isn't that fast, and a paid-for enterprise version could be faster.

Looking at our analysis, we can quickly see that over the 30 day period:

* The Philippines had the biggest earthquake, followed by Chile and Oklahoma
* However the locations with the largest earthquakes on average were Samoa, Svalbard & Jan Mayen, and Vanuatu
* Most of the earthquakes were in North America by broad location, by a large margin
* At a more granular level, Puerto Rico, Hawaii and Alaska had the most frequent earthquakes
* Visually we can see the earthquake fault lines on the map

<iframe seamless frameborder="0" src="https://public.tableau.com/shared/469YTCT49?:embed=yes&:display_count=yes&:showVizHome=no" width = '1090' height = '900'></iframe>
