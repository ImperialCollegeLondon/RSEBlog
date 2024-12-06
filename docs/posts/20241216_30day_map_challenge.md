---
date: 2024-12-16
authors:
  - Sahil590
categories:
  - Technology
tags:
  - Django
  - Python
  - Technology
  - 30DayMapChallenge
  - Mapping
---


# **Mapping with Python: The 30-Day Map Challenge**

![Map challenge overview](images/30day_map_challenge/30dmc_2024.png)

Every year, cartography enthusiasts, geographers, data visualizers, and mapmakers from around the world come together to participate in the [30-Day Map Challenge](https://30daymapchallenge.com/). Organized by Topi Tjukanov, this event celebrates the art and science of mapmaking. Whether you’re an experienced GIS professional or just starting out, this challenge invites you to create and share a map daily, guided by a unique daily theme.

The 30-Day Map Challenge is an inclusive and open-ended initiative that fosters creativity and experimentation in mapmaking. Each day in November presents a different theme, ranging from “Points” and “Lines” to “Fantasy” and “Historical.” Participants are encouraged to interpret these prompts flexibly, utilizing any data, tools, or artistic styles that resonate with them.
<!-- more -->

Participation is voluntary, and there’s no competition involved. Instead, this challenge is a celebration of cartographic creativity. Participants share their creations on social media platforms using the hashtag #30DayMapChallenge, creating a vibrant global gallery of diverse and imaginative maps.

This was my first year participating in the 30-day map challenge. I was inspired to take on the challenge being part of a recent project the RSE team worked on. We created an interactive map that displayed location data gathered from mobile devices to display trends over a map of England. The app used Django, which limited my options for mapping abilities. Since I wanted to directly query the Django database, I chose the library [django-plotly-dash](https://github.com/GibbsConsulting/django-plotly-dash).

For the maps showcased below, you can find the code, files, and data used to create the maps in my personal repository [Sahil590/30daymapchallenge](https://github.com/Sahil590/30daymapchallenge).

In this post, I’ll focus on four maps I created during the challenge:

## 1. Choropleth

![Classic choropleth map. Use color to show data variation across regions. This simple but effective technique is a staple for showing thematic differences. 🎨. Number of vehichles accessable acros the UK](images/30day_map_challenge/Choropleth.png)

This map type got me started on my journey, so it’s only fitting that this is the first map to showcase. For this challenge, I explored some information about vehicle access per household gathered through the 2021 UK census. I thought it would be a nice visualization to see where the most concentrated population of vehicles would be across the UK.

This map provides Census 2021 estimates that classify households in England and Wales by car or van availability and by household. The data was sourced from [ons.gov.uk](https://www.ons.gov.uk/datasets/RM008/editions/2021/versions/3).

## 2. Points

![30DayMapChallenge classic: A map with points. Start the challenge with points. Show individual locations—anything from cities to trees or more abstract concepts. Simple, but a key part of the challenge. 📍 Nature reserves across the UK](images/30day_map_challenge/Points_map.png){:style="display:block;margin:auto;width:50%" }

I created this map using [Plotly](https://plotly.com/examples/) a free and open-source browser-based graphing library that can be used in Dash applications, which is a Python framework that allows developers to build interactive web apps, especially analytical interfaces like dashboards. I am very familiar with this tool, as I have used it in many projects to portray data in maps, charts, and much more!

As mentioned in the caption this was a simple but effective map that primarily focuses on points, where each point represents a local nature reserve across the UK. The data were sourced from [data.gov](https://www.data.gov.uk/dataset/acdf4a9e-a115-41fb-bbe9-603c819aa7f7/local-nature-reserves-england).

## 3. Lines

![Day 2 A map with focus on lines. Roads, rivers, routes, or borders — this day was all about mapping connections and divisions. Another traditional way to keep things moving. 📏. Undersea internet cables ](images/30day_map_challenge/Lines.png)

The Internet is a global network of interconnected cables that span continents, connecting people and devices worldwide. I wanted to visualize this network on a map.

To create this map, I used a GEOJSON file, a geospatial data interchange format based on JavaScript Object Notation (JSON). These files contain data that helps plot lines, points, and polygons on maps. In this case, I used it to plot lines but will also use it to plot polygons later. This was made with the same tools mentioned in the points map.

The data displayed on the map was sourced from the [ArcGIS hub](https://hub.arcgis.com/maps/c12642b516bc4ee5bc9e89870ab14089/about).

## 4. New tool: QGIS

![Day 13 Use a tool you’ve never tried before. The challenge has always been about trying new things. Use a tool, software, or drawing technique you’ve never worked with before. 🧪🔧 QGIS ](images/30day_map_challenge/Qgis.png)

This day revolves around trying out a new tool — something entirely unfamiliar to me. For this challenge, I decided to use QGIS, a free and open-source software specifically designed for geospatial data creation, editing, analysis, visualization, and publication.

QGIS is incredibly powerful. It can handle various tasks, from thematic maps to geospatial analysis and interactive visualizations. Despite its capabilities, I had never used it before. So, I saw this as an ideal opportunity to learn something new and challenge myself.

For my initial map, I opted for simplicity and focused on visualizing the East African Region. I found a detailed dataset from [Natural Earth Hub](https://www.naturalearthdata.com/) that was recommended in my tutorial for QGIS. Once I found the data I needed, setting up the map became the next challenge. Since I had no prior experience, it took some time to get up to speed and produce a map that I was satisfied with, as shown in figure 5.

When creating this map, I discovered that it was much easier to create a highly detailed and visually appealing map compared to the python tools I am used to using.

My initial impression of QGIS was that it was a tool with immense potential, but it also felt overwhelming due to the sheer number of options available. The interface was cluttered with menus, toolbars, and panels, but once I began exploring, everything became intuitive.
