---
date: 2024-10-21
authors:
  - dalonsoa
  - alexdewar
  - SaranjeetKaur
  - AdrianDAlessandro
categories:
  - Events
tags:
  - Open Source
  - Hacktoberfest
---

# Hacktoberfest 2024: Bring your own code

![The Hacktoberfest Logo](images/hacktoberfest24/logo.png){: .no-caption style="display:block;margin:auto;width:69%" }

[Hacktoberfest](https://hacktoberfest.com/) is a month-long annual event that encourages people to contribute to open source throughout October. The motivation of Hacktoberfest is to celebrate all things open source, especially the people that make open source so special.

This year the central RSE team at Imperial planned two in-person events during Hacktoberfest on the 1st and the 23rd of October. The events were open to anybody interested in participating in open source software, either coding or with non-code contributions. Everybody was welcome to join the events and bring their own code along for a discussion with the central RSE team members. This included [low code and non-code contributions](https://hacktoberfest.com/participation/#low-or-non-code) - a big chunk of what surrounds good open source software has nothing to do with code!

<!-- more -->

## First event: 1st of October 2024

The first event took place in the Chemistry Building, in the South Kensington Campus. Most of the attendees were there in person, but a few also joined online. We tried together to work in groups when dealing with the same piece of software, but we were so much in the flow, that we forgot and end up hacking intensely with our own thoughts. Before we realised, it was already lunch time and, then the wrap-up session.

![In person attendees _in the flow_.](images/hacktoberfest24/attendees_day1.jpg){: .no-caption style="display:block;margin:auto;width:69%" }

The following projects were pitched at the start of the event and chosen by the attendees to work on:

### Crystalbot – Benjamin Scharpf​

​(TBC)

### Clockify TUI – Alexander Dewar​

​(TBC)
​
### WSIMOD – Barnaby Dobson​

The terrestrial water cycle is a highly interconnected system where the movement of water is affected by physical and human processes. Thus, environmental models may become inaccurate if they do not provide a complete picture of the water cycle, missing out on unexpected opportunities and omitting impacts that arise from complex interactions.

[WSIMOD](https://imperialcollegelondon.github.io/wsi/) is a modelling framework written in Python to integrate these different processes. It provides a message passing interface to enable different subsystem models to communicate water flux and water quality information between one other, as well as self-contained representations of the key parts of the water cycle (rivers, reservoirs, urban and rural hydrological catchments, treatment plants, and pipe networks).

The main work done for Hacktoberfest was related to [refactoring some bits of code](https://github.com/ImperialCollegeLondon/wsi/pull/106) to improve its modularity.

### SWMManywhere – Barnaby Dobson​

[SWMManywhere](https://imperialcollegelondon.github.io/SWMManywhere/) is a tool to synthesise urban drainage network models (UDMs) using publicly available data such as street networks, DEMs, and building footprints, across the globe. It also provides tools for generating SWMM input files and performing simulations for the synthesised UDMs.

Also only one issue was tackled during the event by our colleague Tom Bland, [setting up Codecov in the repo](https://github.com/ImperialCollegeLondon/SWMManywhere/pull/304), but it brought up a lot of trouble due to a bug in Codecov, apparently. The lessons learnt have been used for other projects, so it was time well spent!

### PyCSVY – Diego Alonso Alvarez​

[CSVY](https://github.com/ImperialCollegeLondon/pycsvy) is a small Python package to handle CSV files in which the metadata in the header
is formatted in YAML. It supports reading/writing tabular data contained in numpy
arrays, pandas DataFrames, and nested lists, as well as metadata using a standard python
dictionary. Ultimately, it aims to incorporate information about the [CSV
dialect](https://specs.frictionlessdata.io/csv-dialect/) used and a [Table
Schema](https://specs.frictionlessdata.io/table-schema/) specifying the contents of each
column to aid the reading and interpretation of the data.

There was a lot of activity in this project, which included:

- [Adding support for polars Dataframes](https://github.com/ImperialCollegeLondon/pycsvy/pull/94)
- Give the [first steps to support CSV Dialects](https://github.com/ImperialCollegeLondon/pycsvy/pull/93)
- [Modernizing the linters and code formatters](https://github.com/ImperialCollegeLondon/pycsvy/pull/95), using `ruff`
- Improving the [continuous integration workflows](https://github.com/ImperialCollegeLondon/pycsvy/pull/95) and [the use of Dependabot](https://github.com/ImperialCollegeLondon/pycsvy/pull/89)

### repositoryr – Saranjeet Kaur Bhogal

​`repositoryr` is a step by step guide for creating an R package repository. This repository itself is structured as a R package that is installable from GitHub. It showcases how `devtools` can be used in RStudio for package development. It also provides steps to create a website for an R package using `pkgdown`.

The activities completed in this project as a part of Hacktoberfest include:

- [Describing the use of `devtools` in package development](https://github.com/ImperialCollegeLondon/repositoryr/pull/5)
- [Adding steps to create a website for the package](https://github.com/ImperialCollegeLondon/repositoryr/pull/6)

## 23rd of October

(summary)

(photo day 2)

The following projects were pitched at the start of the event and chosen by the attendees to work on, some of them, repeating from the previous event:

### Crystalbot – Benjamin Scharpf​

​(TBC)

### Clockify TUI – Alexander Dewar​

​(TBC)

### WSIMOD – Barnaby Dobson​

​(TBC)

### SWMManywhere – Barnaby Dobson​

​(TBC)

### PyCSVY – Diego Alonso Alvarez​

​(TBC)

### E2M – Yilin Jing​

​(TBC)

### Bubble analyser – Yiyang Guan​

​(TBC)

### Auto-CORPus – Joram Posma​

​(TBC)

### Image-draper – Ryan Smith​

​(TBC)

### RSE Competencies Toolkit – Adrian D'Alessandro​

[The RSE Competencies Toolkit](https://rsetoolkit.github.io/rse-competencies-toolkit/) is a community resource to support RSEs (Research Software Engineers) in tracking and managing their professional development. It is in the very early days of development for the interactive web app. This hack day was used to kick start the development of the tool.

The activities are captured in [this umbrella issue on the Django project setup](https://github.com/AdrianDAlessandro/rse-competencies-toolkit-webapp/issues/6).

### repositoryr – Saranjeet Kaur Bhogal

​(TBC)
