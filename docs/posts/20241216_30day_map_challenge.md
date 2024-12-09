---
date: 2024-12-16
authors:
  - Sahil590
categories:
  - Technology
tags:
  - 30DayMapChallenge
  - Technology
  - Python
  - Mapping
---


# **The 30-Day Map Challenge**

![Map challenge overview](images/30day_map_challenge/30dmc_2024.png)

Every year, cartography enthusiasts, geographers, data visualisers, and mapmakers from around the world come together to participate in the [30-Day Map Challenge](https://30daymapchallenge.com/). Organised by Topi Tjukanov, this event celebrates the art and science of mapmaking. Whether you’re an experienced GIS professional or just starting out, this challenge invites you to create and share a map daily, guided by a unique daily theme.

The 30-Day Map Challenge is an inclusive and open-ended initiative that fosters creativity and experimentation in mapmaking. Each day in November presents a different theme, ranging from “Points” and “Lines” to “Fantasy” and “Historical.” Participants are encouraged to interpret these prompts flexibly, utilising any data, tools, or artistic styles that resonate with them.
<!-- more -->

Participation is voluntary, and there’s no competition involved. Instead, this challenge is a celebration of cartographic creativity. Participants share their creations on social media platforms using the hashtag #30DayMapChallenge, creating a vibrant global gallery of diverse and imaginative maps.

This was my first year participating in the 30-day map challenge. I was inspired to take on the challenge being part of a recent project the RSE team worked on. We created an interactive map that displayed location data gathered from mobile devices to display trends over a map of England. The app used Django, which limited my options for mapping abilities. Since I wanted to directly query the Django database, I chose the library [django-plotly-dash](https://github.com/GibbsConsulting/django-plotly-dash).

What I loved most about this challenge was how approachable it felt. Whether I was using familiar tools like Plotly or stepping out of my comfort zone with QGIS, every map I created taught me something new about geospatial visualisation. It was incredible to see how simple datasets could tell compelling stories when mapped out thoughtfully.

I created the first 3 maps below using [Plotly](https://plotly.com/examples/), a free and open-source browser-based graphing library that can be used in Dash applications. I was already very familiar with this tool, as I have used it in many projects to portray data in maps, charts, and much more. Eventually, I decided to make a Dash app which is a Python framework that allows developers to build interactive web apps, especially with analytical interfaces like dashboards, allowing users to interactively explore the data. Usually, users are able to hover over regions to see detailed information about, say, a car's or a van's availability in each area.

If you want to see more of my maps or try recreating them yourself, you can check out my [GitHub repository](https://github.com/Sahil590/30daymapchallenge).
In this post, I’ll focus on the four maps that I created during this challenge:

## Maps

### 1. Choropleth

![Classic choropleth map. Use colour to show data variation across regions. This simple but effective technique is a staple for showing thematic differences. 🎨. Number of vehicles accessible across the UK](images/30day_map_challenge/Choropleth.png)

This map type got me started on my journey, so it’s only fitting that this is the first map to showcase. For this challenge, I explored some information about vehicle access per household gathered through the 2021 UK census. I thought it would be a nice visualisation to see where the most concentrated population of vehicles would be across the UK.

The app reads vehicle data from a CSV file, aggregates it by local authority, and merges it with geographical data from a GeoJSON file containing county boundaries. By ensuring all regions are represented - even those without data. I created a comprehensive dataset that assigns a default observation value to regions lacking specific data. This dataset is then used to generate the choropleth map with the object `px.choropleth_map`, mapping the aggregated observations to geographical regions using colour gradients.

As you can see in Figure 2 there are a lot of white spaces in the map this is a result of lack of data and also that one of the issues I was facing when trying to find data to display on this map I ended up using the Census 2021 estimates that classify households in England and Wales by car or van availability and by household. The data was sourced from [ons.gov.uk](https://www.ons.gov.uk/datasets/RM008/editions/2021/versions/3).
The code that produced this map can be found [here](https://github.com/Sahil590/30daymapchallenge/blob/main/Chropleth.py)

### 2. Points

![Day 1 Start the challenge with points. Show individual locations - anything from cities to trees or more abstract concepts. Simple, but a key part of the challenge. 📍 Nature reserves across the UK](images/30day_map_challenge/Points_map.png){:style="display:block;margin:auto;width:50%" }

As mentioned in the caption, this was a simple but effective map that primarily focuses on points, where each point represents a local nature reserve across the UK. The data were sourced from [data.gov](https://www.data.gov.uk/dataset/acdf4a9e-a115-41fb-bbe9-603c819aa7f7/local-nature-reserves-england).

This app reads location data from a csv file, which contains the latitude, longitude, and names of the reserves. The `go.Scattermapbox` graph object plots these points on a Mapbox map with green markers. The map is centred based on the average latitude and longitude of all points, providing an optimal initial view of the reserves' distribution. The code that produced this map can be found [here](https://github.com/Sahil590/30daymapchallenge/blob/main/Points_Day1.py)

### 3. Lines

![Day 2 A map with focus on lines. Roads, rivers, routes, or borders — this day was all about mapping connections and divisions. Another traditional way to keep things moving. 📏. Undersea internet cables ](images/30day_map_challenge/Lines.png)

The Internet is a global network of interconnected cables that span continents, connecting people and devices worldwide. I wanted to visualise this network on a map.

To create this map, I used a GEOJSON file, a geospatial data interchange format based on JavaScript Object Notation (JSON). These files contain data that helps plot lines, points, and polygons on maps. In this case, I used it to plot lines but will also use it to plot polygons later. This was made with the same tools mentioned in the points map. This was a simple map to produce as it only needed a single file to be plotted. It’s a very simple but effective way of portraying this type of data.

The data displayed on the map was sourced from the [ArcGIS hub](https://hub.arcgis.com/maps/c12642b516bc4ee5bc9e89870ab14089/about).
The code that produced this map can be found [here](https://github.com/Sahil590/30daymapchallenge/blob/main/Lines_Day2.py)

### 4. New tool: QGIS

![Day 13 Use a tool you’ve never tried before. The challenge has always been about trying new things. Use a tool, software, or drawing technique you’ve never worked with before. 🧪🔧 QGIS ](images/30day_map_challenge/Qgis.png)

This day revolved around trying out a new tool - something entirely unfamiliar to me. For this challenge, I decided to use [QGIS](https://qgis.org/), a free and open-source software specifically designed for geospatial data creation, editing, analysis, visualisation, and publication.
QGIS is incredibly powerful. It can handle various tasks, from thematic maps to geospatial analysis and interactive visualisations. Despite its capabilities, I had never used it before. So, I saw this as an ideal opportunity to learn something new and challenge myself.
QGIS is a low-code solution to creating maps, but it does have the capability to use Python that allows you to automate tasks, manipulate map layers and elements, and create complex spatial analysis workflows. I decided to go for the no-code solution since it was my first time using QGIS.

For my initial map, I opted for simplicity and focused on visualising the East African Region. I found a detailed dataset from [Natural Earth Hub](https://www.naturalearthdata.com/) that was recommended in my tutorial for QGIS. Once I found the data I needed, setting up the map became the next challenge. Since I had no prior experience, it took some time to get up to speed and produce a map that I was satisfied with, as shown in Figure 5.

When creating this map, I discovered that it was much easier to create a highly detailed and visually appealing map compared to the Python tools that I am familiar with.
My initial impression of QGIS was that it was a tool with immense potential, but it also felt overwhelming due to the sheer number of options available. The interface was cluttered with menus, toolbars, and panels, but once I began exploring, everything became intuitive.
If you would like to edit the map, I have included the file in the repository that can be opened in QGIS. It is [here](https://github.com/Sahil590/30daymapchallenge/blob/main/30daychallenge.qgz). All you need is to [download](https://qgis.org/download/) QGIS and open the file. It should render and be able to be edited on your own device. It supports Linux, Windows, and macOS.

## Closing thoughts

Taking part in the 30-Day Map Challenge this year has been an eye-opening experience for me. Each theme pushed me to think outside the box and experiment with new tools and techniques. It was a chance to combine creativity with data, and I learned so much along the way.

Some of the aspects of the challenge were more frustrating than challenging. It is how open-ended the instructions are. There is not a lot specified, such as what data is to be used. I found myself spending most of the time finding data to display on my map rather than actually creating the map.
