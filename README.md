# Lab 7 -- Geoparsing with Python
## Introduction
Using Python for geoparsing and mapping .txt files. The task is to read the text from a website, extract locations, draw them on a map, and create a word frequency plot. The challenge this week is to create a function that takes a URL as an argument and output a map and a plot.. 
## Result
The book I want to explore is *Travels into Turkey*.The outputs are a spatial footprint map and a word frequency plot. Here are the results.
![Spatial Footprint Map](images/FootprintMap.png)
![Word Frequency Plot](images/FrequencyPlot.png)
## Desicusion
The main problem for me is how to show the both results together. The [matplotlib.subplots](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.subplots.html) can create a figure and a set of subplots. So I use this to show the two figures together. Here is the code.
```
# world map .shp file we down/uploaded
countries_map = gpd.read_file(basemap_url)
f, ax = plt.subplots(figsize=(16, 16))
plt.title("Spatial footprints map")
countries_map.plot(ax=ax, alpha=0.4, color='grey')
geo_df['geometry'].plot(ax=ax, markersize = 30, color = 'r', marker = '^', alpha=.4)
# Create freq dist and plot
f, ax = plt.subplots()
freqdist1 = nltk.FreqDist(cities)
plt.title('Word frequency distribution')
freqdist1.plot(20) #running this w/out an argument plots all words! Here, we're specifying the top 20 geo_df['geometry'].plot(ax=ax, markersize = 30, color = 'r', marker = '^', alpha=.4)
```
## Question
* Is the gazetteer we're using appropriate for the text? What might be some of the challenges of using Nominatim with a book published in 1859?

Using the gazetteer in 2021 to get the coordinates of the names in the book published in 1859 is not appropriate. There are some challenges. First, the names of a city could be changed in 62 years because of the politicc, wars, or other reasons. Second, the border of one city also could be changed, which indicated the center coordinate would be different.
* How accurate do you think the outputs are? Are there any city names that seem suspect? If so, what are some Natural Language Processing methods that you could use to filter words that might be city names, but probably weren't intended as city references in the text?

I think the accuracy of the result is not high. In the word frequency plot, "man" has the highest frequency. "man" is not a place name but Nominatim thinks it as Isle of Man. From Hu et al. (2021), a simple approach is to resolve a place name to its most prominent or default place instance, such as the one that has the highest population or the largest total area (these types of information are often available in gazetteers).
* What are some alternative approaches to mapping the data? The tutorial here uses duplicate entries and opacity to present a type of "color ramp": could this be done better?

We can also count the number of each city. And, show it by the size of marker on the map.
