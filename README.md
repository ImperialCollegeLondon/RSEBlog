# RSE Team Blogs

This repository contains blogs by the [central RSE team](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/service-offering/research-software-engineering/about-the-team/).

## How to contribute a blog?

### Development tools setup

- Make sure you have [poetry](https://python-poetry.org/docs/#installation) installed.
- Clone the repository locally (`git clone https://github.com/ImperialCollegeLondon/RSEBlog.git`).
- Install dependencies `poetry install`
- Install the pre-commit hooks `poetry run pre-commit install`

### Adding your blog

- Create a `.md` file for your blog in the `docs/posts` folder and add the blog content to it.
- Any images should be added to `docs/posts/images/<blog-name>`.
- Add all the relevant metadata (for example, `date`, `authors`, `categories`, `tags`) at the start of the blog:

```zsh
---
date: 2024-07-22
authors:
  - github_username_author_a
  - github_username_author_b
  - github_username_author_c
categories:
  - category_a
  - category_b
  - category_c
tags:
  - tag_a
  - tag_b
  - tag_c
---

# Blog title

Introduction

```

Note: If authors are not already listed in `docs/.authors.yml`, please add them. Similarly, any mew `categories` should be listed under `categories_allowed` in `mkdocs.yml`.

- After the introduction paragraph of the blog, include `<!-- more -->`. This will render a shorter version of the blog on the landing page with a `Continue reading` link.
- Preview the blog locally using `mkdocs serve`.
- Open a `Pull Request` and request for a review.
