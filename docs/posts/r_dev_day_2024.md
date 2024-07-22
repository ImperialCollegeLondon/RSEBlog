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

On Friday, April 26th, 2024, the [central Research Software Engineering (RSE) team](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/service-offering/research-software-engineering/) at Imperial College London hosted the [R Dev Day](https://pretix.eu/r-contributors/r-dev-day-imperial-2024/) at the Seminar and Learning Centre on the South Kensington Campus. This event, proposed by [Dr. Heather Turner](https://warwick.ac.uk/fac/sci/statistics/staff/academic-research/turner), aimed to foster collaboration among both new and experienced contributors interested in contributing to base R. I had the pleasure of co-organising this event alongside Dr. Turner and [Dr. Diego Alonso Alvarez](https://profiles.imperial.ac.uk/d.alonso-alvarez).

<!-- more -->

### R Dev Day structure

The work for the dev day was organised into various [issues](https://github.com/r-devel/r-dev-day/issues?q=is%3Aopen+is%3Aissue+label%3A%22Imperial+2024%22) labelled “Imperial 2024” on the `r-dev-day` repository of the `r-devel` organisation on GitHub. Contributors were assigned to different issues based on their interests and expertise.

### My Contribution

I focused on [improving the consistency of glossaries across languages](https://github.com/r-devel/r-dev-day/issues/8) in the R project on [Weblate](https://translate.rx.studio/projects/r-project/glossary/). The current glossary on Weblate is not consistent across different languages. Hence, I created a combined glossary by reviewing the glossary for each language currently listed on the Weblate. The code snippet for the same and the combined glossary that it generates are available in the [issue comments](https://github.com/r-devel/r-dev-day/issues/8). The next steps for this work is to share the combined glossary with the [translations team leaders](https://contributor.r-project.org/translations/Conventions_for_Languages/#languages-and-contributions) and ask if they would be happy for the glossary to be managed globally, so that the glossary is common across languages and new terms can only be added by the Weblate Admins.  Any translator could suggest a new term by contacting the team leader or posting on the #core-translation channel of the [R Contributors Slack](https://contributor.r-project.org/slack).

### Other Contributions

Other contributors worked on issues such as [Bug 17713](https://bugs.r-project.org/show_bug.cgi?id=17713) and [Bug 18472](https://bugs.r-project.org/show_bug.cgi?id=18472) on [Bugzilla](https://bugs.r-project.org/), and [improving the `substr()` documentation](https://github.com/r-devel/r-dev-day/issues/5).

## SatRdays London

The R Dev Day was a satellite event to [SatRdays London](https://satrday-london-2024.jumpingrivers.com/), which I attended on April 27th, 2024. This was my first in-person SatRdays event, hosted by the [Centre for Urban Science & Progress London](https://cusplondon.ac.uk/) at King’s College London and co-organised by [Jumping Rivers](https://www.jumpingrivers.com/). SatRdays events are open to anyone interested in R, from beginners to seasoned users.

At the event, there was a line of speakers presenting all things R, each offering valuable lessons. Below, I will briefly summarise some of the talks that I found particularly interesting.

[Andrie de Vries](https://www.linkedin.com/in/andriedevries/) delivered a talk titled “[Lessons learnt from product management, applied to data science](https://andrie.quarto.pub/pmds-saturdays/#/title-slide)”. Andrie shared his journey as a product manager, explaining the product life cycle and the concept of “Jobs to be done (JTBD)” as the first step in creating a product’s value proposition. He discussed the application of product management principles to data science and how to 10x better exercise can be implemented product improvement. Andrie also highlighted examples of successful projects that turned into products, such as [data.table](https://rdatatable.gitlab.io/data.table/), [R Open Sci](https://ropensci.org/), [H2O.ai](https://h2o.ai/).

[Nicola Rennie](https://www.linkedin.com/in/nicola-rennie/) presented a talk on “[Typst or LaTeX? Styling PDF Documents with Quarto Extensions](https://nrennie.rbind.io/talks/satrdays-london-2024/)”. In her talk, Nicola provided an overview of ways to create customised PDF documents using Quarto with a little help from R, LaTeX, and Typst. She compared the styling of  [Quarto](https://quarto.org/) PDFs using [LaTeX](https://www.latex-project.org/) and with [Typst](https://typst.app/docs) discussing the pros and cons of each method. Nicola also briefly explained how [Quarto Extensions](https://quarto.org/docs/extensions/) can be used to enhance document styling.

[Myles Mitchell](https://www.linkedin.com/in/myles-mitchell-4009aa98)’s talk, “[Using R to Teach R](https://myles-mitchell.github.io/satrdays-2024/#1)” focused on how [Jumping Rivers](https://www.jumpingrivers.com/) utilises R in their training stack to deliver both R and non-R [training courses](https://www.jumpingrivers.com/training/). Myles described their standardised approach, which includes a central repository with internal R packages for building courses. He acknowledged some drawbacks, such as a restrictive course structure, but emphasised that the benefits of using this approach, such as reduced duplication and ability to conduct numerous courses, outweigh these limitations.

[Matt Thomas](https://www.linkedin.com/in/matthewgthomas/) in his talk, “Where data meets disaster: A journey through the British Red Cross’s ‘[humaniverse](https://github.com/humaniverse)’” shared about the British Red Cross’s ‘humaniverse‘ project which is a collection of R packages that clean and summarise public UK humanitarian data into a tidy data format, aiding in disaster response and preparedness.

[Charlie Gao](https://github.com/shikokuchuo) presented a talk on how [mirai can be used with Shiny and Plumber applications](https://shikokuchuo.net/satRdays-London-2024/#/title-slide). [Mirai](https://cran.r-project.org/web/packages/mirai/index.html) is a lightweight R package for parallel code execution and distributed computing. He mentioned that mirai is a simple and robust package which is highly performant and is designed for production. It has been added as the first alternative communications backend for the parallel package in base R.

[Hannah Frick](https://www.linkedin.com/in/hannah-frick/) in her talk “Survival analysis is coming to tidymodels!” discussed how [tidymodels](https://www.tidymodels.org/) can perform survival analysis and deal with time-to-event data. The tidymodels package provides a framework for modelling and machine learning using tidyverse principles in R.

Attending the SatRdays London event was a fantastic opportunity to get an idea of the diverse applications and innovations within the R community. The presentations highlighted how R is being used in fields ranging from data science and product management to document styling and disaster response. Each speaker brought practical knowledge, enriching my understanding of how R can be leveraged to solve complex problems and enhance workflows. Overall, it was an inspiring day that reinforced the collaborative spirit and continuous growth within the R ecosystem.

## Future Events

The [central RSE team](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/service-offering/research-software-engineering/about-the-team/) at Imperial College London is interested in supporting the R community at Imperial and host similar events in the future. If you have any ideas or suggestions, please feel free to reach out and we can discuss the possibilities. By participating in these events, we continue to grow and strengthen the R community, fostering collaboration and learning among R users of all levels, at Imperial College London and beyond.
