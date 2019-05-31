---
layout: post
title:  "Finding the best location for an Art Book Store in Paris : A geospatial data analysis"
date:   2015-05-25 14:00:00
categories: jekyll update
tags: 
image: 
---


## Introduction 
<br/>
### Background and business problem  
<br/>
Paris, «city of light», is a well known location for art lover’s. With more than 1,000 art gallery, many museum and exposition, she has becomed a famous place for  cultural tourism, attracting millions people from all over the world each year. The cultural offer is wide and numerous thanks to, in particular, the support of the ministry of culture. The market of art and derived products is likewase well devolopped and very attractive for professionals, investissor and enterprises. 

We are going to realize a study for an artbook store chain who want to install a shop in French capital. The analysis will consist of a first approach to find a selection of the best potantial borough on which more advanced investigation will be processed later.

To find the best location for an artbook store we identify three major business requierement :
* As all big city, Paris is an expensive place, so we have to take into account the store rent costs by borough.
* The attractivness of the borough, with the number of art-culture and store venues.
* The accessibility, with the transports venues.

For this purpose, firstly, we will create a density map of all venues (culture, store and transport) to locate the best places and well deserved borough.
Secondly we will study the correlation between rent price and number of venues, using for this purpose regression analysis. We will also make a histogram to compare borough by categories of venues and rent price variation.
Finally, to facilitate the choice of the stakeholder, we will create cluster of different type of borough in fonction of the most common venues.

This study is representative of the first step in geomarketing analysis and would be the same for other kind of store by modifying the categories of venues selected.

### Data description  
<br/>
For this study, we will use free data from different websites and services, in different formats:

* A free account on Foursquare API [1] to get culture, store and transport venues of borough of Paris.  This free account have restriction on the number of venue we could request in a limited time. The request result are in geojson format.

<p align="center">
<img src="/assets/article_images/2018-05-25-art_bookstore/DF_venues.jpg"><br/>  
<small>Table 1 : Selected venues of Paris</small>
</p>

* The geometry of each borough come from open data Paris [2]. We downloaded the shape format. 
<p align="center">
<img src="/assets/article_images/2018-05-25-art_bookstore/DF_geom.jpg"><br/>  
<small>Table 2 : Geometry data of Paris borough</small>
</p>

* The location store per square meter come from localcommercial.net [3], in csv format. 

<p align="center">
<img src="/assets/article_images/2018-05-25-art_bookstore/DF_rentPrice.jpg"><br/>  
<small>Table 3 : Paris real estate price per borough</small>
</p>

## Methodology
<br/>
### Rent cost by number of venues

In a first time to represent the rent price per number of venues, we merged data of the borough geometry and the rent cost. And we create a map using the folium library to show the rent cost per borough :
<p align="center">
<img src="/assets/article_images/2018-05-25-art_bookstore/Map_rentCost.jpg"><br/>  
<small>Figure 1: Average store rent price per square meter per year in Paris</small>
</p>

Then we use the foursquare API to get the data on culture, store and transport venues categories. And we put them on the map to have a fisrt understanding of the relation between rent price and the number of venues:
<p align="center">
<img src="/assets/article_images/2018-05-25-art_bookstore/Map_venues_rentCost.jpg"><br/>  
<small>Figure 2 : Map of Venues get from Foursquare API</small>
</p>

Due to the Foursquare free account limitation we have a limited number of venues (150) distributed on the map accross the borough. It seems to have a correlation between number of venues and rent price, but at first look it’s not very clear. let's try to verify this impression.


### Correlation between rent price and number of venues

To make correlation between rent price and venues we first use the library geopandas to make a spatial join between venues and geometry data. Then we merged the result with price data.
So we could count the number of venues per borough, and prepare a dataframe to realize the repgression analysis :

<p align="center">
<img src="/assets/article_images/2018-05-25-art_bookstore/DF_regression.jpg"><br/>  
<small>Table 4 : Number of venues and rent price per borough</small>
</p>

We use the library Seaborn to create the plot of linear regression and the residual plot to test the linearity correlation :
<p align="center">
<img src="/assets/article_images/2018-05-25-art_bookstore/residualPlot.jpg"><br/>  
<small>Figure 3 : Residual plot of venues per rent price</small>
</p>
<p align="center">
<img src="/assets/article_images/2018-05-25-art_bookstore/RegPlot.jpg"><br/>  
<small>Figure 4 : Plot of linear regression of venues per rent price</small>
</p>

It seems we have a relatively good correlation (with a variability around ± 150 €), if we make abstraction of two points. This two outliers (corresponding to borough 4 and 6), cercled in red on the figures, are very interresting as they could indicate overpriced borough. But before making conclusion we must go further on investigating different venues.


### Attractiveness of borrough

Let’s take a look at the different venues per borough and their relation with rent cost using a histogram.

We first group the number of venues by type and borough :

<p align="center">
<img src="/assets/article_images/2018-05-25-art_bookstore/DF_catVenues.jpg"><br/>  
<small>Table 5 : Number of venues per borough and type</small>
</p>

And then create an histogram with pyplot :
<p align="center">
<img src="/assets/article_images/2018-05-25-art_bookstore/Histo.jpg"><br/>  
<small>Figure 5 : Number of venues per categories and rent price</small>
</p>
Here we can see two things: first, we see that the outliers lack data  (borough 4 and 6). Making a zoom on folium map and openstreetmap data we could verify the presence of metro access in this borough. A quick search on google let us find many outdoor bookstore venues on this borough :

<p align="center">
<img src="/assets/article_images/2018-05-25-art_bookstore/bouquinistes.jpg"><br/>  
<small>Figure 6 : Map of open-air bookstore</small>
</p>

So we have confirmation of a lack of venues on this places.
Secondly, correlation between rent price and number of venues seems pretty good. Not surprisingly, many of the boroughs in the center are too expensive (districts 2 to 6, taking into account the lack of data due to the limitation of the free foursquare account).

### Cluster of boroughs

We have 20 borough with different venues of culture, store and transport. Let’s try to group them to simplify the presenttion and the choice of the stakeholder. For this we used the algorithm k-means clustering :
<p align="center">
<img src="/assets/article_images/2018-05-25-art_bookstore/mostComVenues.jpg"><br/>  
<small>Table 6 : Most common venues per borough</small>
</p>

We selected 5 clusters. Some of them are the consequence of a lack of data. The darkest red group is the borough's best candidate with the most cultural venues and shops. The second darker could be selected if necessary. More residential, it has cultural places that can be exploited. The other groups are less interesting and could be grouped together.
<p align="center">
<img src="/assets/article_images/2018-05-25-art_bookstore/Map_clusters.jpg"><br/>  
<small>Figure 7 : Map of cluster</small>
</p>

## Results
<br/>
With the selected culture, store and transport venues from Foursquare and the geometry data from open data Paris, we have located places of interest on a map. To better show the density of venues we made  a density map or « Heatmap ».
This map gives, more precisely, the best location, the best borough of Paris. It could be used for futur small scale surveys to choose a selection of neighborhood in each borough.
<br/>
<p align="center">
<img src="/assets/article_images/2018-05-25-art_bookstore/Map_HeatMap.jpg"><br/>  
<small>Figure 8 : Heatmap of venues</small>
 <br/>
 We find that the number of venues and the price are correlated. We also do cluster to highlight the most attractive borough based on the most common categories of venues. With this results we can study the cost per venue of of the most interesting borough group. 

We could see non negligeable difference beetween the selected borough. The most attractive one are also the most expensive. The stakeholder must choose between the total cost of the rent or its attractiveness and its profitability (cost per site) : 
 
</p>
<p align="center">
<img src="/assets/article_images/2018-05-25-art_bookstore/DF_costPerVenues.jpg"><br/>  
<small>Table 7 : Rent cost per venues</small>
</p>
 
## Discussion
<br/>
The results seem realistic, but in fact the study must be reworked.

As we have noted several times, the limitation of Foursquare free account data was a significant bias in the result we give ; even if the result seems close to what we would have reasonably expected. 

In addition, many other analyzes could have been carried out. For example : study of different categories of sites and their density, more precise selection of different sites in each category.  

## Conclusion
<br/>
We give stakeholders a better understanding of the borough's attractiveness, depending on the sites, for the potential client of an art bookstore. We also show the place more or less interesting in terms of cost per venues.

That’s a good start point to make a decision, select the best borough on wich to make more investigations.

## References:
<br/>
* [1] https://foursquare.com 
* [2] https://opendata.paris.fr
* [3] https://www.localcommercial.net 


