---
layout: post
title: Earthquake Tracking Dashboard Using Tableau
description: This is a Tableau Dashboard that tracks global Earthquake activity across a 30-day period
image: "/posts/earthquake-map.jpg"
tags: [Tableau, Data Viz, Earthquakes]
---

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
* Then we created a top ten largest earthquakes list, showing their location and magnitude.
* Then we built an element showing the percentage of earthquakes by broad location.
* Finally we created a sortable visualisation of earthquakes by frequency, average and maximum magnitude.
* These elements were arranged within a single dashboard, and all hooked up to a date filter so you can scroll through the data day by day over the 30 day period, or look at it all at once. 

### Results <a name="overview-results"></a>

<iframe seamless frameborder="0" src="https://public.tableau.com/shared/469YTCT49?:embed=yes&:display_count=yes&:showVizHome=no" width = '1090' height = '900'></iframe>
