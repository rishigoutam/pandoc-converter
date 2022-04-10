---
title: Improving Citi Bike Operations through Bike Rebalancing
subtitle: Using map visualizations for bike stations and rebalances
author: Rishi Goutam, Srikar Pamidi, James Goudreault
author-url: 'https://github.com/rishigoutam/citibike'
date: 'April 10, 2022'
abstract: In this article, we show how we analyzed bike rebalances, based on station ridership, capacity, elevation, and time of day
keywords: Citi Bike; NYC Data Science Academy
description:
return-url: '..'
return-text: '← Return home'
---

## Summary
- locations
- bike angels program successful
- elevation matters

## Rebalance Operations

### What is rebalancing? Why do we care?

Citi Bike faces a perpetual dillemma - how can they ensure there are enough bikes in the right places to satisfy rider demand? Will the natural flow of bikes from rider be enough?

Consider the flow of bikes through a popular commuter station on a weekday:

![Commuter-Station-Bike-Flow](https://hackmd.io/_uploads/B1Cspm979.jpg)

The blue line represents the cumulative change in quantity of bikes at the station throughout the day. The gray and red lines represent the typical and maximum high-volume dock station capacities respectively.

Even if the station were to naturally start each day fully stocked, the station would be depleted halfway through morning commuter hours with significant remaining demand. Someone needs to bring in more bikes so all riders can get to work - and conversely shuffle them away at the end of the day...

Simply put, rebalancing is the manual movement of bikes from one station to another to:
- ensure there are sufficient bikes at each dock station to satisfy predicted demand
- ensure there is space at ending dock station to receive incoming bikes
- minimize number of underutilized bikes sitting at unpopular locations

### How does Citi Bike Rebalance?

Citi Bike employs several methods to manage rebalancing needs:

- *Valet Service* - team members staff popular stations to manage incoming and outgoing bikes, artificially elevating station capacity and temporarily alleviating rush-hour problems
- *Bike Trains* - team members ride ebikes towing carriages of 12-16 bikes, ideal for rebalances in and around tight neghborhood streets
- *Motorized Vehicles* - higher capacity vans operate 24/7 to keep bikes moving to and from the most popular stations
- *Bike Angels* - Launched in 2018, this program grants Citi Bike riders (non-employees) traveling along in-demand rebalance routes a free trip and rewards points

### Identifying Rebalance Movements

The Citi Bike dataset does not contain information on individual bike rebalances, nor is that generally available online except as aggregated data in monthly reports. So how can someone tell when a bike was rebalanced, and to where?

![Identifying-Rebalances](https://hackmd.io/_uploads/HkE2EN9m5.jpg)

The easiest method to do so is (for a given bike) compare the starting station for each trip with the ending station of the previous trip. If the bike appears to have teleported from one station to another between trips, it most likely was rebalanced!

### General Rebalance Statistics

Predictably, the number of rebalance operations increases with number of rides. The large drop after 2017 coincides with the introduction of the Bike Angels program and clearly demonstrates how successful this clever initiative is.

![Rides-vs-Rebalances](https://hackmd.io/_uploads/HyMLHN9mc.jpg){ width=350px }
![Rides-per-year](https://hackmd.io/_uploads/BkctLE9Xc.png){ width=350px }

For each year, almost every unique bike in service was rebalanced at least once and almost every station was invovled in rebalancing operations.

![rebalancing-unique-bikes](https://hackmd.io/_uploads/SkDsi4qX5.jpg){ width=350px }
![rebalancing-unique-stations](https://hackmd.io/_uploads/Hycis4qm9.jpg){ width=350px }

The vast majority of rides occurred in Manhattan, so it is not suprising to see that is where most rebalance activity occurs.
![Rebalance-By-Boro](https://hackmd.io/_uploads/S1OGFI9X9.png)

It is important to note that bikes are rebalaced around and across all Boros - confirming Citi Bike actively works to satisfy all rider demand.
![Log-Rebalance-By-Boro](https://hackmd.io/_uploads/ryYuFUc79.png)

Individual stations with the highest rebalanace activity are mostly commuter stations. Many have both a high number of bikes moved to and from them based on commuter demands as shown above.
![Stations-Rebalance](https://hackmd.io/_uploads/BkEV6dhX9.png)

### Rebalance Routes

We can glean more information about rebalance operations if we look not just at bikes moving to and from individual stations, but consider pairwise movement between two stations.

![Rebalance-Routes](https://hackmd.io/_uploads/HJhBaLqXq.png)

Not only is the majority of rebalance activity focused on a small number of stations in Manhattan, most of it occurs between a small number of station pairs. This makes sense in the context of commuter bike flow, where there is a need for a constant flow of bikes away from destination stations to origin stations to keep up with demand.

Notice that some of the top routes are just the reverse direction of others. Visualizing bi-directional flow between station pairs further emphasizes how critical rebalance operations are between a small number of station pairs.

![Bi-Directional-Rebalance-Routes](https://hackmd.io/_uploads/S1nzCI979.png)

It should be noted that rebalance operations are probably more nuanced then bikes being moved along static 'routes' - it's very possible a van will stop at several stations along a circuit - but this aggregate analysis still helps us understand how much effort and expense is required to support the highest volume stations.

Perhaps the most intersting way to look at station related rebalance data is visualized on a map - check out our dash app to see this for 2019!

### Rebalance Timing
We don't know precisely when a bike was rebalanced, only when a trip started after the bike was rebalanced These time estimates (in hours) all reflect the maximum time a rebalance operation could take. We can conclusively say a good part of Citi Bike rebalancing is done to alleviate immediate demand, such as during commutes.

![Rebalance-Timing](https://hackmd.io/_uploads/r1La58cX9.png){ width=350px }

Again, it appears that the Bike Angels program and Valet Service had profound impacts on the need for immediate rebalances.
![Rebalance-Timing-YearFacet](https://hackmd.io/_uploads/SyWzoL5X9.png)

It appears that there are different reblance strategies used for popular stations. Some seem to have the majority of rebalances done overnight while others seem to be more frequently rebalanced for immediate use. Both modes are necessary to keep up with demand.
![Rebalance-Timing-Stations](https://hackmd.io/_uploads/r1ZQaKh75.png)

Investigating timing for rebalance routes provides deeper insight, where we can see some routes are used to stock stations overnight for the next day's commute and others are used to satisfy immediate demand.
![Rebalance-Timing-Station-Pairs](https://hackmd.io/_uploads/ryMH6t2m9.png)

### Seasonality

As expected, rebalance activity tracks with ridership where most of the activity occurs in the summer months. The Bike Angel program launched in 2018 had a profound effect on flattening this trend compared to previous years.

![Annual-Rebalance-Season](https://hackmd.io/_uploads/SkoCY8cmc.png)

Similarly, post Bike Angel rebalance activity tracks with ridership where most acitivity occurs during the week supporting commuters.
![Weekly-Rebalance-Season](https://hackmd.io/_uploads/HkqX585Xq.png)

### Bikers Dislike Going Uphill

In aggregate more bikes were rebalanced from lower elevation stations than to, and more bikes were rebalanced to higher elevation stations than from. This suggests bikers who ride downhill often find other modes of transport for return trips.

![rebalance-elevation-bins](https://hackmd.io/_uploads/r1KB3N5Xc.jpg){ width=350px }
![rebalance-elevation-density](https://hackmd.io/_uploads/By422Vqm9.png){ width=350px }

This becomes even more apparent when looking at a density plot of years pre and post launch of the Bike Angel program. In 2016, the difference between station elevations for rebalance movements is somewhat normally distributed. In 2019, there is a long right tail indicating significantly more bikes are rebalanced to higher elevations - even the Bike Angels aren't willing to climb hills.

### Future Analysis?

There are nearly endless ways to dive into ridership and rebalance data to understand needs and motivations of bikeshare participants or the general movement of individuals around New York. This analysis focused mostly on data aggregated over the years available (2014-2000) which helps us interpret rebalance operations as a whole. Future work could include:

- Focusing on a specific year, season, or days to understand and predict usage and rebalance needs with respect to changes in overall ridership and events such as holidays and professional sports games
- Comparing rides and rebalance statistics across years as docking stations are introduced, moved, or retired to evaluate past decisions and inform future strategy/goals
- Developing a predictive model to inform Citi Bike if a dock stations is on track to be depleted or filled and needs immediate rebalancing outside of normally scheduled operations, and if or how it may be influenced by external factors such as weather

## Visualizing Bike Stations and Rebalances

look at our dash app!



