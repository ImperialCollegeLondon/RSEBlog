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

Every year, cartography enthusiasts, geographers, data visualizers, and mapmakers from all over the world gather to participate in the 30-Day Map Challenge. Organized by Topi Tjukanov, this prestigious event celebrates the art and science of mapmaking. Whether you’re an experienced GIS professional or just starting out, this challenge invites you to create and share a map every day, guided by a unique daily theme.

The 30-Day Map Challenge is an inclusive and open-ended initiative that encourages creativity and experimentation in mapmaking. Each day in November presents a different theme, ranging from “Points” and “Lines” to “Fantasy” and “Historical.” Participants are encouraged to interpret these prompts flexibly, using any data, tools, or artistic styles that resonate with them.
<!-- more -->
Participation is voluntary, and there’s no competition involved. Instead, this challenge is a celebration of cartographic creativity. Participants share their creations on social media platforms using the hashtag #30DayMapChallenge, creating a vibrant global gallery of diverse and imaginative maps.

This was my first year participating in the challenge. I was inspired to take on the challenge because of a recent project the RSE team worked on. They created an interactive map to display result data over a map of England. The app used Django, so I had limited options for mapping abilities. Since I wanted to directly query the Django database, I chose the library [django-plotly-dash](https://github.com/GibbsConsulting/django-plotly-dash).

For this post, I’ll focus on five of the maps I created during the challenge:

## 1. Choropleth

![Day 16 Choropleth. Nature reserves across the UK](images/30day_map_challenge/Choropleth.png)

This map type got me started on my journey, so it’s only fitting that this is the first map to showcase. I decided to use this map type. For this challenge, I delved into some information about vehicle access per household gathered through the 2021 UK census and thought it would be a nice visualization to see where the most concentrated population of vehicles would be throughout the UK.

*What is a Choropleth?*

A choropleth map uses variations in shading or color to represent data tied to specific geographic areas. Each region—be it a country, state, county, or district—is shaded based on the value of the chosen variable.

## 2. Points

![Day 1 Points. Nature reserves across the UK](images/30day_map_challenge/Points_map.png){:style="display:block;margin:auto;width:50%" }

The data displayed on the map is every nature reserve and park located in the UK. The data was sourced from [data.gov](https://www.data.gov.uk/dataset/acdf4a9e-a115-41fb-bbe9-603c819aa7f7/local-nature-reserves-england).

## 3. Lines

![Day 2 Lines. Undersea internet cables ](images/30day_map_challenge/Lines.png)

The internet is a global network of interconnected cables that span continents, connecting people and devices worldwide. I wanted to visualize this network on a map.

To create this map, I used a GEOJSON file, a geospatial data interchange format based on JavaScript Object Notation (JSON). These files contain data that helps plot lines, points, and polygons on maps. In this case, I used it to plot lines but will also use it to plot polygons later.

The data displayed on the map was sourced from the [ArcGIS hub](https://hub.arcgis.com/maps/c12642b516bc4ee5bc9e89870ab14089/about).

## 4. Data: HDX (Humanitarian Data Exchange) Day 8

## 5. New tool QGIS Day 13
