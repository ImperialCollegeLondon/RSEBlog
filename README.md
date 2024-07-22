# RSE Team Blogs

This repository contains blogs by the [central RSE team](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/service-offering/research-software-engineering/about-the-team/).

## How to contribute a blog?

- Make sure you have [poetry](https://python-poetry.org/docs/#installation) installed.
- Clone the repository locally (`https://github.com/ImperialCollegeLondon/RSEBlog.git`).
- Create a `.md` file for your blog in the `docs/posts` folder and add the blog content to it.
- Any images should be added to `docs/posts/images/<blog-name>`.
- Add all the relevant metadata (for example, `date`, `authors`, `categories`, `tags`).
- Add any new authors to `docs/.authors.yml`.
- After the introduction paragraph of the blog, include `<!-- more -->`. This will render a shorter version of the blog on the landing page with a `Continue reading...` link.
- Preview the blog locally using `mkdocs serve`.
- Open a `Pull Request` and request for a review.
