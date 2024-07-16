---
date: 2024-06-26
authors:
  - cc-a
categories:
  - Events
tags:
  - GitHub Actions
  - Environment
  - Continuous integration
---

# Adopting a more rational use of Continuous Integration with GitHub Actions

In the Imperial RSE Team we make extensive use of [continuous integration](https://en.wikipedia.org/wiki/Continuous_integration) (CI) with
[GitHub Actions](https://docs.github.com/en/Actions). We use CI to ensure our projects build and are correct across a range of
scenarios (OS, python version, dependency version, etc.). Widely accepted wisdom is that
it is best practice to catch issues early via frequent and thorough CI rather than to
catch them later. This must however be set against the monetary and environment cost of
running unnecessary compute workloads on every push to GitHub. In particular, the
pricing structure of GitHub Actions means workloads run on Windows and MacOS are more
costly (certainly financially and presumably environmentally). This is particularly the
case for private repositories for which Imperial has a fixed budget of minutes.

<!-- more -->

Some recent issues with hanging GitHub Actions runners caused us to review our approach
to CI across our full set of projects. We decided there is no one size fits all
approach, but we should be more parsimonious in our use of CI by default whilst
reviewing the need for more comprehensive CI as issues arise in individual projects.

To come up with our new standard approach we considered the key points in development
where CI can be used. Under our typical development practices, there are four times
when CI workflows may be run. Decreasing in frequency of occurrence:

- Push to feature branches.
- (non-draft) Pull request against primary development branch.
- Push to primary development branch.
- Before release.

The most parsimonious viable approach to CI that we landed was:

- Not running workflows on pushes to feature branches.
- Running QA and test workflows under a single scenario (eg. in Linux with Python 3.12) on pull request.
- Not running workflows on pushes to primary development branch.
- Running test workflows under a full range of scenarios before release.

From this starting point we can revise the extent of testing performed at each stage
based on experiences on the project e.g. if we consistently experience issues with a
particular OS at release time then we can consider testing on that more frequently.

The above has wider implications for our development practices:

- Branch protection rules must be used to prevent direct pushes to main/dev and to require “branches to be up to date before merging” and these should be adhered to strictly.
- Account for potential issues arising around releases. You should build in time/manage expectations to fix anything that might come up.

We also identified several other key best practices that help reduce our footprint:

- The default timeout for workflows on GitHub hosted runners is 6 hours. This is far larger than it needs to be and in the event of a hanging runner can be very expensive. All jobs should have reduced a timeout set (unfortunately this has to be done on a per-job basis), but especially those on non-linux runners.
- Use caching wherever possible. This can be particularly useful for [Python dependencies](https://github.com/actions/setup-python#caching-packages-dependencies) or build artifacts (via ccache).
- Consider carefully what OS’s, Python versions, etc. your project actually needs to support and only test against those. Also bear in mind that it may not be necessary to do a full “matrix” of tests e.g. all different OS’s with all different Python versions.
- Enable manual execution of workflows (via the `workflow_dispatch` trigger) so you can run things as desired. This may be useful to e.g. get early warning of issues before attempting to create a release or to force checks to run against a draft PR.

In summary, CI is a valuable tool to ensure the correctness of software projects, but
there is some low hanging fruit ready to pick to make a more rational use of it
saving time, money and reducing the environmental impact of the process.
