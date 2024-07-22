---
date: 2024-05-22
authors:
  - cc-a
categories:
  - Events
tags:
  - Open Source
  - R
  - SatRdays
  - Weblate
---

# Creating impact in R from London

## R Dev Day at Imperial College London

On Friday, April 26th, 2024, the central Research Software Engineering (RSE) team at Imperial College London hosted the R Dev Day at the Seminar and Learning Centre on the South Kensington Campus. This event, proposed by Dr. Heather Turner, aimed to foster collaboration among both new and experienced contributors interested in contributing to base R. I had the pleasure of co-organising this event alongside Dr. Turner and Dr. Diego Alonso Alvarez.

<!-- more -->

### R Dev Day structure

The work for the dev day was organised into various issues labelled “Imperial 2024” on the `r-dev-day` repository of the `r-devel` organisation on GitHub. Contributors were assigned to different issues based on their interests and expertise.

### My Contribution

I focused on improving the consistency of glossaries across languages in the R project on Weblate. The current glossary on Weblate is not consistent across different languages. Hence, I created a combined glossary by reviewing the glossary for each language currently listed on the Weblate. The code snippet for the same and the combined glossary that it generates are available in the issue comments. The next steps for this work is to share the combined glossary with the translations team leaders and ask if they would be happy for the glossary to be managed globally, so that the glossary is common across languages and new terms can only be added by the Weblate Admins.  Any translator could suggest a new term by contacting the team leader or posting on the #core-translation channel of the R Contributors Slack.

### Other Contributions

Other contributors worked on issues such as Bug 17713 and Bug 18472 on Bugzilla, and improving the `substr()` documentation.

## SatRdays London

The R Dev Day was a satellite event to SatRdays London, which I attended on April 27th, 2024. This was my first in-person SatRdays event, hosted by the Centre for Urban Science & Progress London at King’s College London and co-organised by Jumping Rivers. SatRdays events are open to anyone interested in R, from beginners to seasoned users.

At the event, there was a line of speakers presenting all things R, each offering valuable lessons. Below, I will briefly summarise some of the talks that I found particularly interesting.

Andrie de Vries delivered a talk titled “Lessons learnt from product management, applied to data science”. Andrie shared his journey as a product manager, explaining the product life cycle and the concept of “Jobs to be done (JTBD)” as the first step in creating a product’s value proposition. He discussed the application of product management principles to data science and how to 10x better exercise can be implemented product improvement. Andrie also highlighted examples of successful projects that turned into products, such as data.table, R Open Sci, H2O.ai.

Nicola Rennie presented a talk on “Typst or LaTeX? Styling PDF Documents with Quarto Extensions”. In her talk, Nicola provided an overview of ways to create customised PDF documents using Quarto with a little help from R, LaTeX, and Typst. She compared the styling of  Quarto PDFs using LaTeX and with Typst discussing the pros and cons of each method. Nicola also briefly explained how Quarto Extensions can be used to enhance document styling.

Myles Mitchell’s talk, “Using R to Teach R” focused on how Jumping Rivers utilises R in their training stack to deliver both R and non-R training courses. Myles described their standardised approach, which includes a central repository with internal R packages for building courses. He acknowledged some drawbacks, such as a restrictive course structure, but emphasised that the benefits of using this approach, such as reduced duplication and ability to conduct numerous courses, outweigh these limitations.  

Matt Thomas in his talk, “Where data meets disaster: A journey through the British Red Cross’s ‘humaniverse’” shared about the British Red Cross’s ‘humaniverse‘ project which is a collection of R packages that clean and summarise public UK humanitarian data into a tidy data format, aiding in disaster response and preparedness.

Charlie Gao presented a talk on how mirai can be used with Shiny and Plumber applications. Mirai is a lightweight R package for parallel code execution and distributed computing. He mentioned that mirai is a simple and robust package which is highly performant and is designed for production. It has been added as the first alternative communications backend for the parallel package in base R.

Hannah Frick in her talk “Survival analysis is coming to tidymodels!” discussed how tidymodels can perform survival analysis and deal with time-to-event data. The tidymodels package provides a framework for modelling and machine learning using tidyverse principles in R.

Attending the SatRdays London event was a fantastic opportunity to get an idea of the diverse applications and innovations within the R community. The presentations highlighted how R is being used in fields ranging from data science and product management to document styling and disaster response. Each speaker brought practical knowledge, enriching my understanding of how R can be leveraged to solve complex problems and enhance workflows. Overall, it was an inspiring day that reinforced the collaborative spirit and continuous growth within the R ecosystem.

## Future Events

The central RSE team at Imperial College London is interested in supporting the R community at Imperial and host similar events in the future. If you have any ideas or suggestions, please feel free to reach out and we can discuss the possibilities. By participating in these events, we continue to grow and strengthen the R community, fostering collaboration and learning among R users of all levels, at Imperial College London and beyond.
