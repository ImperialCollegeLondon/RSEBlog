# RSE Team Blogs

This repository contains blogs by the [central RSE team](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/service-offering/research-software-engineering/about-the-team/).

## How to contribute a blog?

### Development tools setup

- Make sure you have [poetry](https://python-poetry.org/docs/#installation) installed.
- Clone the repository locally (`git clone https://github.com/ImperialCollegeLondon/RSEBlog.git`).
- Install the dependencies `poetry install`
- Install the pre-commit hooks `poetry run pre-commit install`

### Adding your blog

- Create a `.md` file for your blog in the `docs/posts` folder and add the blog content to it.
- The file name should start with the publication date without spaces or dashes to faciliate finding the posts in the file structure, for example `20240722_post_name.md` for the example below.
- Any images should be added to `docs/posts/images/<blog-name>`.
- Add all the relevant metadata (for example, `date`, `authors`, `categories`, `tags`) at the start of the blog:

``` markdown
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

Note: If authors are not already listed in `docs/.authors.yml`, please add them. Similarly, any new `categories` should be listed under `categories_allowed` in `mkdocs.yml`. We should endeavour to keep categories very broad.

- After the introduction paragraph of the blog, include `<!-- more -->`. This will render a shorter version of the blog on the landing page with a `Continue reading` link.
- Preview the blog locally using `mkdocs serve`.
- Open a `Pull Request` and request for a review.

### Additional features

The global settings of this repository (based on the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme) allows you to include additional features to your blog posts like admonitions, code blocks, etc.

#### Admonitions

Admonitions or call-outs are useful for including side content without interrupting the flow of your blog.

- To include an admonition block, start with `!!!` followed by a single [type qualifier](https://squidfunk.github.io/mkdocs-material/reference/admonitions/#supported-types) keyword.
- Add the block content on the next line, indented by four spaces.
- The type qualifier (in titlecase) is the default title of the block. You can add a customised title after the type qualifier in quoted string.

```` markdown
!!! note "Placeholder for title"

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.
````

Refer to the [admonitions documentation](https://squidfunk.github.io/mkdocs-material/reference/admonitions/) to learn more about the different types of admonitions and possible customisations.

#### Code blocks

- To add code blocks, begin and end the block with lines containing three backticks.
- To add syntax highlighting to those blocks, add the language shortcode directly after the opening block. Refer to the [list of available lexers](https://pygments.org/docs/lexers/) to find the shortcode for a given language.
- To add a custom title to the code block use `title="<custom title>"` option directly after the language shortcode.

Here is an example of a Python code block that adds three numbers:

```` markdown
``` py title="add_numbers.py"
def add(x, y, z):
  return x + y + z
```
````

Refer to the [code blocks documentation](https://squidfunk.github.io/mkdocs-material/reference/code-blocks/) for more customisations.

#### Rendering mathematical expressions

You can use LaTeX markup to render mathematical expressions, enabled via [Mathjax](https://www.mathjax.org) and [mdx_math](https://github.com/mitya57/python-markdown-math).

- To write an inline expression, wrap the expression in `$` delimiters.
- To write the expression as a block, delimit the expression with either `$$` or as a [code block](#code-blocks) with the `math` shortcode.

#### Customising images

Images are converted to figures automatically using the [MkDocs Caption](https://tobiasah.github.io/mkdocs-caption/) extension. This extension enables putting captions on tables and lists as well as images, however the most basic behaviour uses the image's Alt Text field to populate the figure caption.

For Example:

``` markdown
![figure caption](img.jpg)
```

Will be converted to:

```html
<p>
<figure id=_figure-1>
    <img alt="figure caption" src="img.jpg" />
    <figcaption>Figure 1: figure caption</figcaption>
</figure>
</p>
```

Which will render as a centred image with the caption "Figure 1: figure caption" and maintain the alt text of the image as "figure caption". MkDocs Caption also provides other ways to define the Figure caption, which allows you to have different text for the figure and alt text.

If you do not want the image to have a caption, you can label it as a `no-caption` class using an [Attribute List](https://squidfunk.github.io/mkdocs-material/setup/extensions/python-markdown/#attribute-lists). Reminder, [it is important to include Alt Text](https://www.imperial.ac.uk/staff/tools-and-reference/web-guide/training-and-events/materials/accessibility/images/) for accessibility reasons.

``` markdown
![figure caption](img.jpg) {: .no-caption }
```

You can then add to the attribute list to further specify the style. A useful one is for centering images:

``` markdown
![figure caption](img.jpg) {: .no-caption style="display:block;margin:auto;"}
```

More information on image style options can be found in the [Mozilla Documentation](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img#styling_with_css).
