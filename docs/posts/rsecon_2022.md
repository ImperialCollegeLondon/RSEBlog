---
date: 2022-09-27
authors:
  - AdrianDAlessandro
  - alexdewar
  - cc-a
  - dc2917
  - TinyMarsh
categories:
  - Events
tags:
  - RSE
  - RSECon
  - Technology
---

# Highlights from RSECon22 from the central RSE team

## Overview

This year, the annual conference organised by the [Society of Research Software Engineering](https://society-rse.org/) was back on as an in-person event for the first time since 2019. This meant that for many of the college’s central RSE team it was their first opportunity to meet up with RSEs from across the country and further afield. The College was well represented, with five delegates attending from the central team along with RSEs based in specific departments, research groups and teaching staff from the Graduate School.

<!-- more -->

Overall, the conference was an excellent opportunity to strengthen old connections and make new ones, with plenty of opportunities for networking. The mix of talks, posters, panel discussions, tutorials and walkthroughs covered a wide range of topics from the highly technical to more community-focused issues. Although it is impossible to cover the whole event in detail, we have summarised some of our personal highlights below. The full set of recorded presentations should be available on the [SocRSE Youtube channel](https://www.youtube.com/channel/UCL7rYOIAP1Rx_VajLPDF-hA) soon.

## Highlights

### Citation File Format

[Stephan Druskat](https://sdruskat.net/) gave a talk on The [Citation File Format (CFF)](https://github.com/citation-file-format/citation-file-format), which lets you provide metadata for software or datasets directly in the repository. There are many advantages to doing this, primarily it helps move research software towards becoming a citable “first class” research output alongside papers. The CFF has become very widely used in recent years – there are now 8,000 CITATION.cff files on GitHub compared to 500 in 2018.

Some recent updates:

- There is [a webform to help generate a good citation file](https://citation-file-format.github.io/cff-initializer-javascript/#/) – this will ensure that the CFF is at least valid and picked up by GitHub.
- [Zenodo supports CFF](https://github.com/citation-file-format/citation-file-format#why-should-i-add-a-citationcff-file-to-my-repository-bulb) – if your GitHub repository has a CFF file, Zenodo can use that to get more accurate metadata for your repository than would otherwise automatically be pulled out.

### Software Licensing

A panel discussion led by [Bob Turner](https://rse.shef.ac.uk/contact/bob-turner/) focused on licensing code for better impact. This is always a lively topic of debate among RSEs, but in general the panel were broadly in agreement about most questions. We have distilled the gist into some one-sentence answers:

- **Should RSEs advise a default open-source license and if so, which?** RSEs should decide together with the research team what is the most appropriate license based on the goal of the project, this should be decided before work starts.
- **Should the organisations grant ownership of code we produce to license as we choose?** The university owning the code is fine as long as I can choose the license. (Top tip – write the license into the grant proposal so the university has already agreed to it)
- **How can we make good decisions on whether code should remain closed source for commercial reasons?** As RSEs, we all 💙 open-source software, but sometimes sustaining a code is easier through a commercial route, rather than trying to build a community around the code.

### Debugging GitHub actions locally

It is a very common and painful problem when your unit tests run locally but fail in the Continuous Integration run on GitHub. The usual way out of this is to make changes, commit, push and wait for the GitHub runners to complete your tests again, crossing your fingers that this time the problem is solved. A great walkthrough by [Pascal Fürlich](https://www.pik-potsdam.de/members/pascalfu) showed how [act](https://github.com/nektos/act) can be used to get much faster feedback by running GitHub actions locally using a Docker image.

### MLOps

Machine Learning (ML) is used more widely than ever across almost all spheres of research. ML outputs require the same attention to ensure their reproducibility and interpretability as any other research software, but many researchers may lack the tools to achieve this. [Stephen Haddad](https://x.com/i/flow/login?redirect_after_login=%2Fstevehadd)’s excellent walkthrough on ML Ops for Research Software Engineers highlighted some key concepts, tools, and resources that RSEs can use to support researchers with this aim. You can go through the walkthrough yourself by [cloning it from GitHub](https://github.com/informatics-lab/ukrse_2022_mlops_walkthrough).

### Tutorial-driven development

[Chris Woods](https://research-information.bris.ac.uk/en/persons/christopher-j-woods) from Bristol University gave a very interesting overview of “tutorial-driven development”. As the name suggests, this approach involves focusing on the tutorial for how a user would interact with software throughout the development process. For example, the documentation and tutorials should be updated in any pull requests and code is kept up-to-date with the docs (not the other way around!). A particularly interesting suggestion was to encourage researchers to write the tutorial for any software they want to develop as if the code already exists at the start of the project scoping process.

### Mental health

[Dave Horsfall](https://www.software.ac.uk/fellowship-programme/dave-horsfall) from Newcastle University gave a super plenary talk focused on mental health for RSEs and how we can create a judgement-free environment where topics around mental health and wellbeing can be discussed. Dave is a Software Sustainability Institute Fellow and advocates for better mental health in the RSE community. The presentation was highly interactive and insightful. We would encourage any RSEs who have not already done so to fill out [Dave’s short survey](https://softwaresaved.limequery.com/837235) that aims to capture a snapshot of mental health within the UK community.

### Quarto

[Quarto](https://quarto.org/) is the next generation of RMarkdown with support for more languages (Python, R, Julia) and document input/output formats (Jupyter notebooks, [RevealJS](https://revealjs.com/) presentations). Feature development in [RMarkdown is in fact stopping, with Quarto as the new focus](https://quarto.org/docs/faq/rmarkdown.html#is-r-markdown-going-away-will-my-r-markdown-documents-continue-to-work) (though RMarkdown will continue to be maintained and supported). Quarto is built on top of the venerable [Pandoc](https://pandoc.org/) for the conversion. You can convert Jupyter notebooks to other formats, but the native input format is qmd (Quarto markdown, a generalisation of RMarkdown). The main idea is that you can create webpages, presentations, pdfs etc with embedded code blocks in a supported language that are run when the document is converted. This core functionality seems to work pretty well.

Some caveats were observed however. Some of the fancier bells and whistles are still a bit rough around the edges. Support for multiple languages within a single document was quite limited, as were the opportunities for interactivity. The documentation of these aspects was also a bit limited. More interactivity can be achieved but you must embed Javascript and who wants to do that?

In summary, one to watch. Backed by Posit (formerly RStudio) and as the successor to RMarkdown it can’t be ignored. The rougher edges around the documentation/fancier features will no doubt be ironed out over time. The new Carpentries workbench (lesson template) is also moving to Quarto.
