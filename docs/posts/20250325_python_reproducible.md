---
date: 2025-03-25
authors:
  - dalonsoa
categories:
  - Technology
tags:
  - Python
  - Reproducibility
---

# Mini-guide to reproducible Python code

A lot of modern research requires custom software to be written, either to do some
calculations, analyse experimental data or something else. Creating good quality, sustainable
software is always desirable, but ticking all the boxes that are often described as
necessary to accomplish this can be a daunting tasks for people - researchers - who have
often other priorities in mind.

Reproducibility is, however, not an optional feature of a piece of research - including
software or otherwise - and that is something that **researchers are fully responsible** to
address. Luckily, out of the many requirements of good quality and sustainable software,
only a handful are necessary, or can go a long way, to support the reproducibility of
the results.

In this post we describe these, absolutely essential steps that are necessary for
researchers to take in order to support the reproducibility of their software in the
case of Python. It might not apply to all cases, and it is not fool proof as
reproducibility is a really complex business, but it is a good start and will narrow the
chances of things going wrong when other people try to use the software.
<!-- more -->

## Starting with a script or notebook

Very often, the starting point of some Python code used in research is a plain script
(e.g. `analisys.py`) or a Jupyter Notebook (e.g. `analisys.ipynb`). In there, a few
dependencies might be loaded, as well as some input data, some analysis is done and
then, finally, a few plots or some processed data files are created. Your software
directory could look like:

```bash
my_software/
  └── analysis.py
```

Providing this with your paper is **not enough** to make it reproducible.

What versions of the dependencies are you using? In what operative system are you
working on? What version of the input data? Moreover, what version of your code? There are plenty
of things that can be different between your computer and where someone else might be
running the tool, so let's try to narrow down a bit the uncertainty.

## Adding a README and dependencies

The first step is no indicate what dependencies you are using **and** which versions of
them. This is key because different versions of the dependencies might produce different
results or simply be incompatible. For example, your code might need `pandas`,
`matplotlib` and `numpy`, but work with `numpy` versions older than version 2. You must
communicate this information so others know what they need to have installed.

Moreover, what version of Python are you using? Version 3.9 is very different than
version 3.13. And in what operative system. It is not rare that some code works well in
one platform, . Linux, but not in another one. Or works but produces different
results.

To address these two issues, we need to add two extra files.

The first one is a file indicating the dependencies and versions you are using. Common
choices are `requirements.txt` files and `environment.yml` files. The first one is used
with `pip` while the second one is used with `conda`, but both serve the same purpose:
define what other (typically) Python tools, specifically, your software depends on.

!!! Note
    You can get the full list of packages installed in your current environment with
    commands like [`pip freeze`](https://pip.pypa.io/en/stable/cli/pip_freeze/) or
    [`conda env export`](https://docs.conda.io/projects/conda/en/latest/commands/env/export.html)
    but keep in mind that this will include ALL packages in the environment, which might
    be more than actually needed by your software if you do not use virtual environments
    or were not careful when installing the dependencies.

The second is a README.md file. In simple terms this is just a small text filiving details
on the operative system, Python version, installation instructions - i.e. how to install
the dependencies - and usage instructions.

As an example, your software directory now could look like:

```bash
my_software/
  ├── README.md
  ├── requirements.txt
  └── analysis.py
```

And the contents of the new files could be:

- `requirements.txt`

```text
numpy==2.1
pandas==2.2
matplotlib==3.9
```

- `README.md`

```md
# Data analysis software

This software has been tested using:
- Windows 11 Enterprise
- Python 3.12.4

To install the dependencies, open Powershell and run:

`python -m pip install -r requirements.txt`

Then, to run the software run in the same terminal:

`python analysis.py path/to/input/data/folder`
```

There is plenty of other things that can be included, like how to create a virtual
environment, the format of the input data or instructions for other operative systems
but, at least, you are providing details on how to run the tool in a system like the
one you used.

However, including the above files is still **not enough** to make your software
reproducible. Because, what version of your software should other people be using
in order to reproduce your results?

## Using version control

Version control is the technique that enables you to track changes in your code, giving
you a unique identifier for each snapshot of your code whenever you `commit` changes. A
common tool to do that is `git`, which is a command line tool but that also has many
graphical user interfaces for those less keen on the terminal.

It is besides the point of this mini-post to explain how to do version control, so we
will just refer to [our Introductory course on Git and GitHub](https://imperialcollelondon.github.io/introductory_grad_school_git_course/).
For the purposes of reproducibility, understanding the processes and tools described in
this course is more than enough.

If using `git`, the only change to the software directory will be the creation of a
_hidden_ folder within, `.git`, which is created automatically when you initialise the
repository. It contains all the history of your version controlled directory, but you do
not need to worry about it. You will not need to modify the files there manually, but
only via `git` commands, like `git commit`.

With version control enabled for your software directory, we can start talking about
reproducibility. It is not infallible - it never will - but it is good enough for most
purposes with minimal effort.

## Publish in an online repository

This is all good, but your version controlled, properly described software is still in
your computer: how do you share it with others, including people reading your papers who
want to reproduce your results?

The answer is putting your software into an online repository.

Online code repositories like GitHub, GitLab or Bitbucket enable you to share your code
with others. This might be with yourself in another computer, with a restricted set of
collaborators or with the whole World in the case of public repositories. These
repositories contain your software and all its version controlled history, which means
you can easily indicate to other people what specific version of the software they need
to use in order to reproduce your results. Normally, they have features to easily tag
specially relevant snapshots of your code - e.g. creating releases - to make easier to
find the right versions with human readable names (e.g. `v1.0`, rather than `j245er...`,
which is a commit hash).

The same [introductory course](https://imperialcollelondon.github.io/introductory_grad_school_git_course/)
mentioned above includes instructions on how to use GitHub, so we will not provide more
details here.

!!! Note
    If you want to be a bit more thorough, you should archive the relevant versions of
    your code in archives like Zenodo or Figshare, which will give you a Digital
    Object Identifier (DOI) which will uniquely identify the version and will keep your
    code "forever". In the end, software history in GitHub can be altered and past versions
    deleted, so this sort of archiving is a more permanent solution supporting reproducibility.

## Conclusions

There is a lot that can be done to improve a piece of software and making it sustainable
and reproducible, but getting started and picking the low hanging fruit is very
accessible, takes very little time and can go a long way to support your research. You
can always refine things later, adding more tooling and creating a more complex
structure for your code. But this is a good place to bin.
