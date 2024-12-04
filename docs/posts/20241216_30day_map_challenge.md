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
---


# **Mapping with Python: The 30-Day Map Challenge**

![Map challenge overview](images/30day_map_challenge/30dmc_2024.png)

Every year, cartography enthusiasts, geographers, data visualizers, and mapmakers from all over the world gather to participate in the 30-Day Map Challenge. Organized by Topi Tjukanov, this prestigious event celebrates the art and science of mapmaking. Whether you’re an experienced GIS professional or just starting out, this challenge invites you to create and share a map every day, guided by a unique daily theme.

The 30-Day Map Challenge is an inclusive and open-ended initiative that encourages creativity and experimentation in mapmaking. Each day in November presents a different theme, ranging from “Points” and “Lines” to “Fantasy” and “Historical.” Participants are encouraged to interpret these prompts flexibly, using any data, tools, or artistic styles that resonate with them.

Participation is voluntary, and there’s no competition involved. Instead, this challenge is a celebration of cartographic creativity. Participants share their creations on social media platforms using the hashtag #30DayMapChallenge, creating a vibrant global gallery of diverse and imaginative maps.

This was my first year participating in the challenge. I was inspired to take on the challenge because of a recent project the RSE team worked on. They created an interactive map to display result data over a map of England. The app used Django, so I had limited options for mapping abilities. Since I wanted to directly query the Django database, I chose the library [django-plotly-dash](https://github.com/GibbsConsulting/django-plotly-dash).

For this post, I’ll focus on five of the maps I created during the challenge:

## 1. Points Day 1

![Day 1 Points. Nature reserves across the UK](images/30day_map_challenge/Points_map.png){:style="display:block;margin:auto;width:50%" }

The map above was created using the [dash-leaflet](https://www.dash-leaflet.com/) library, which is a wrapper for the JavaScript library [leaflet](https://leafletjs.com/). This library provided many tools, including interactive mapping with click events and unique markers. I embraced these features and used them to create the map.

The data displayed on the map is every nature reserve and park located in the UK. The data was sourced from [data.gov](https://www.data.gov.uk/dataset/acdf4a9e-a115-41fb-bbe9-603c819aa7f7/local-nature-reserves-england).

## 2. Lines Day 2

For this day, I continued to use the same tools I mentioned in Day 1. However, this day focused on lines, and what better lines to talk about than the internet?

It’s no secret that the internet world is connected by a series of cables spanning continents, connecting us all. I wanted to visualize this on a map.

To create this map, I used a GEOJSON file (a geospatial data interchange format based on JavaScript Object Notation (JSON)). These files contain data that helps plot lines, points, and polygons on maps. In this case, I used it to plot lines but will also use it to plot polygons later on.

The data displayed on the map was sourced from the [ArcGIS hub](https://hub.arcgis.com/maps/c12642b516bc4ee5bc9e89870ab14089/about).

## 3. Data: HDX (Humanitarian Data Exchange) Day 8

## 4. New tool QGIS Day 13

## 5. Choropleth Day 16

This was the map type that got me started on this journey, so it’s only fitting that this is the last map to showcase. I decided to use this map type.
