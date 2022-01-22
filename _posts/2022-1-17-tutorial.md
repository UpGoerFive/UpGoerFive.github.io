---
layout: post
title:  "Using GeoPandas to Plot and Manipulate Geographic Data"
date:   2022-1-18 22:52:18 -0600
categories: [Flatiron, GeoPandas]
published: False
---
## Using GeoPandas to Plot and Manipulate Geographic Data

While working on a recent project, I wanted to map our data for presentation. Too late in the process I came across GeoPandas, and I wasn't able to get it working in time. Thankfully another team member was skilled in Tableau (thanks Abass!) and got the map done, but I still was interested in learning more about GeoPandas for future use. Before reading the rest of this post I'd recommend taking a look at the [Introduction](https://geopandas.org/en/stable/getting_started/introduction.html) on the GeoPandas site, as that's what I started with.

#### What is it and why?
GeoPandas is a python library for dealing with geographic and spatial data. Rather than using multiple libraries in order to import, clean, and plot this type of data, GeoPandas bundles all this functionality for a more seamless experience. In this tutorial, I'll show how to get a basic notebook up and running with GeoPandas and stitch a few of its more interesting features together.

Much of the functionality available in GeoPandas could be accomplished by employing CartoPy, Pandas, and GeoPy. Unsurprisingly, those last two are dependencies of GeoPandas (though I believe GeoPy is optional). As the name might imply, GeoPandas primarily is set up to use and mimic Pandas to deal with data, most exemplified by GeoSeries and GeoDataFrame being subclasses of the equivalently named Pandas structures. Effectively this gives all the power and versatility of dealing with data in Pandas, without needing to add in different conversions beforehand.

#### Importing Data
To start with we'll take [this](https://www.kaggle.com/yamqwe/omicron-covid19-variant-daily-cases) popular dataset of Covid cases from kaggle. There are entries for each country, on different days, for different variants. We'll use GeoPandas to build an interactive map of Omicron cases, and create a time-lapse of overall Covid cases with the help of a package called Celluloid. The accompanying repository for this post has the data in a csv file already, but kaggle has easy download instructions on their site.

#### Limitations
The large amount of dependencies and their specific version requirements has been a hurdle to using GeoPandas so far, though this is understandable when the main advantage of the library is packaging together smaller modules in to something more coherently usable. The conda dependency manager wasn't able to easily install GeoPandas into any of my previously existing environments, and running `conda create -n temp-env geopandas` produced an environment with 119 packages. Obviously not the end of the world and many of them are packages that you would have up and running in your environment anyway, but it lends to dependency conflicts and long solves from the package manager. If you're only using GeoPandas to create visualizations, this is of course not as much of an issue, as you can just make a standalone environment, import your data, and save your map. Otherwise though it's probably better to make sure this is added in first.

More significantly, the geocoding ability is somewhat limited. `.geocode` calls in **GeoPy** will accept keyword arguments to help you select an appropriate point. Unfortunately, the GeoPandas `.geocode` method does not allow passing these arguments into the query, only parameters for the initialization of the Geocoder object. Since GeoPy is a needed dependency to use geocoding in GeoPandas, this isn't the end of the world, but in that case why bother with the GeoPandas version if it's more restrictive. In the above example, I had to use a separate GeoPy Geocoder to correct the locations for Georgia, Switzerland, and Morocco (using `exactly_one = False` which was not able to be passed in the GeoPandas version), since the initial GeoPandas call confused those locations for ones in the US 

#### Works referenced



<script src="https://utteranc.es/client.js"
        repo="UpGoerFive/UpGoerFive.github.io"
        issue-term="pathname"
        theme="github-dark"
        crossorigin="anonymous"
        async>
</script>
