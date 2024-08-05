---
date: 2021-11-22
authors:
  - dalonsoa
categories:
  - Technology
tags:
  - Django
  - Python
  - Technology
---

# Fine Tuning Django User Permissions

*Dr Dan Davies from the Imperial RSE team has written a how-to guide based on his experiences with the Django web framework for python. Read the full blog post here.*

The RSE team is involved in an increasing number of software projects requiring a front-end web app. The main advantage to having a web app element for your research software is that users can interact with it via a web browser, without having to install anything to their local machine. There are of course downsides, including the need to deploy, host and maintain software somewhere suitable. However, there is a wide range of popular frameworks to make the whole process a lot smoother.

<!-- more -->

User permissions are an important consideration for any web app. This is not necessarily just to do with overall security, but how you might want different types of users – with different roles – to interact with your software. For example, it is common to require admin users to be able to perform a wide variety of actions, while the majority of users should only be able to perform a small subset of actions. The degree of complexity required will depend on the overall aim.
