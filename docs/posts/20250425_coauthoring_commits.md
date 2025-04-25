---
date: 2025-04-25
authors:
  - dc2917
categories:
  - Technology
tags:
  - Git
  - GitHub
  - Collaboration
  - Version Control
---

# Co-authoring git commits

In this blog post we explain how you can acknowledge contributions from collaborators in
your software (or other version-controlled project) development with `git` via commit
messages. This is a great way to encourage and reward collaboration in your projects,
and acknowledge efforts that may otherwise go unnoticed.

We assume that you are already familiar with version control - in particular with `git`;
if this is not the case, feel free to check out our introductory course on using [git
and GitHub][git_course].

<!-- more -->

## Describing commits

When making commits, it is best practice to include a message to describe the changes
made to the source file(s). When used well, this provides an effective means of browsing
the development history through `git` functionality such as `git log`. However,
repository hosting sites such as GitHub provide additional features that extend the
utility of commit messages, enabled by adding specific key words or phrases to the
messages themselves.

One such additional feature is the ability to declare co-authors, i.e. others who have
contributed to the set of changes in a given revision. You may have noticed this
feature, or perhaps inadvertently used it, when reviewing a pull request: when accepting
suggested changes, GitHub will provide a default commit message (which you are free -
and encouraged! - to change), and the list of commits will show the icons and usernames
of both the user who suggested the change, as well as the user who committed it. Well,
there is in fact nothing magic about commits made via suggestions - it all comes down to
what GitHub provides in the default commit message. We can achieve the exact same thing
when making commits via `git`'s command line interface, or through any graphical
front-end to `git` you may wish to use.

We'll come back to this shortly, but let's first explore some use cases for declaring
co-authors in your commits.

## Why declare co-authors?

Beyond the obvious point that it only seems fair that we credit others for their
contributions to our work, there are some additional benefits to documenting who has
contributed to a particular piece of the code or project.

For a start, you may find yourself wondering who to contact about a particular aspect of
the project - for example, if you are a new contributor, or are exploring an unfamiliar
part of a large codebase, and would like to know who the best person to get in touch
with is. Sure, `git show` (or the unfortunately named `git blame`) will tell you who the
author was, but they may no longer be involved in the project or available for contact.
If co-authors have been detailed, you have more options for useful points of contact.

Another reason to consider co-authoring commits is that the git log provides an audit
trail for the project. While we may most commonly associate `git` (and version control
in general) with software, this need not be the case; you may use version control for
documenting scientific experiments or financial transactions, where audit trails may be
legally required. Being able to declare who has worked on which aspects of the project
improves the quality of the audit trail, which ultimately makes things easier when it
comes to reporting to audit or funding bodies.

## Common co-authoring scenarios

So in what situations might you consider explicitly adding details of co-authors to your
commit messages? We'll describe some common scenarios here, but don't let this limit
you! Acknowledging contributions from others is fundamental to the success of
open-source software development, and we encourage you to credit your collaborators
whenever possible.

As mentioned above, GitHub provides default commit messages when committing suggested
changes as part of a pull request review. However, you may decide to implement suggested
changes locally rather than via the GitHub interface for a number of reasons. For
example, the suggestion may not meet the quality assurance (QA) checks in your
continuous integration pipeline, or follow code conventions that have been established
as part of the project. The editor provided in GitHub's code review interface has
minimal functionality, and will not warn you if your suggestion goes past the project's
preferred line length, for example. Committing such a change directly would result in
failed QA checks, and therefore require further commits to fix. To avoid this, you can
make the suggested change locally, perform any QA checks and commit the change, adding
the user who made the suggestion as a co-author.

Similarly, suggestions may come in the form of pseudocode, or as an abbreviated or
simplified code snippet to simply demonstrate some functionality. Such effort should
still be acknowledged, and adding them as a co-author is an ideal way to do so.

Of course, not all discussion takes place on GitHub, or even on-line, and a colleague
may have made a suggestion, provided insight, advice or help verbally, or by some other
means. This is especially common as an RSE, where we may not have the specialist domain
knowledge but do have the software expertise, while a member of the research group we
are working with has the domain knowledge but not necessarily know the best approach in
terms of implementation. Their input should not go unacknowledged just because they
didn't explicitly write the code. Adding them as a co-author on commits that incorporate
their expertise is a great way to provide credit.

Another great opportunity for co-authoring commits is when pair programming. In this
scenario, two people will usually be working together at the same machine, or remotely
editing the same file(s), so commits can only be made from a single `git` instance.
Being an inherently collaborative practice, both members of the pair should have their
contributions acknowledged, and co-authoring commits provides a way to do exactly that.

## How to declare co-authors

`git` supports a commit message format of a summary line followed by an optional section
for an extended description. While no strict convention is enforced, it is common to
limit the summary line to 50 characters, and provide further details in the extended
description section (note that GitHub hides contents of the summary line after 70
characters). It is within this extended description that you can add co-authors, which
GitHub and GitLab can identify via the `Co-authored-by:` token.

You can add extended commit messages by simply adding further `-m` flags to `git
commit`, since `git` will concatenate further messages as separate paragraphs, e.g.

```Shell
git commit -m "Summary line" -m "Extended commit description."
```

Alternatively, your shell may allow you to add multiple lines within a single set of
quotation marks, e.g.

```Shell
$ git commit -m "Summary line
>
> Extended commit description.
> "
```

However, the command line is not best suited for writing long portions of text. By
running `git commit` without the `-m` flag, `git` will open up an editor, allowing you
to provide multi-line commit messages natively.

!!! Note "Using your preferred editor"
    Ensure you have configured `git` to use your favourite editor via `git config
    core.editor`

You can then enter your commit description, adding co-authors as follows:

```Text title="COMMIT_EDITMSG"
Summary line

Extended description of changes made.

Co-authored-by: username_of_co-author_1 <email_address_of_co-author_1>
Co-authored-by: username_of_co-author_2 <email_address_of_co-author_2>
```

GitHub can then identify the co-authors by their email address, and thereby associate
the commits with their account, allowing them to be properly credited for their
contribution.

Note that some users opt to keep their email address private. GitHub also provides users
with a `noreply` email address, which takes the format:

```E-mail
username@users.noreply.github.com
```

if their account was created before 18th July 2017, or

```E-mail
ID+username@users.noreply.github.com
```

if their account was created after 18th July 2017, where `username` is their GitHub
username, and `ID` is the unique numerical identifier given to each GitHub account.

Be sure to respect your co-author's privacy considerations when adding them as a
co-author, using their `noreply` email address if they have chosen to keep their account
email address private.

## Summary

Project work generally involves input from many people with various areas of expertise,
and a single development may involve multiple members' time and effort. Adding
co-authors to your commits is a simple yet effective way to acknowledge your
collaborators and make their efforts feel valued.

Repository hosting sites such as GitHub and GitLab recognise the `Co-authored-by` token
in extended commit messages, so your co-author's contributions can be linked to their
account. This is a great way to encourage continued collaboration, as well as motivate
others to get involved in your project.

[git_course]: https://imperialcollegelondon.github.io/introductory_grad_school_git_course/index.html
