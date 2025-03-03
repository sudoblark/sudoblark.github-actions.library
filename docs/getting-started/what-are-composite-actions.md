# The official docs

According to GitHub's [documentation](https://docs.github.com/en/actions/sharing-automations/creating-actions/creating-a-composite-action#composite-actions-and-reusable-workflows):

> Composite actions allow you to collect a series of workflow job steps into a single action
> which you can then run as a single job step in multiple workflows.

# A layman's explanation

In object-orientated programming we strive for code re-use, avoidance of duplication,
and cohesive modules within our codebase.

The same may be said for CI/CD operations. Any CI/CD platform boils down simply to
a bunch of arbitrary code that is executed in response to a stimulus. A commit to a pull request,
a merge to the main branch, a release, a tag, a requested workflow execution... these are all just
stimuli to trigger some form of arbitrary code to fulfill a need.

In short, if you imagine CI/CD as an event-based architecture, and the operations that our
pipelines/workflows perform as arbitrary code like any other, you're off to a good start.

# Application of quality principles

The question then becomes how to apply the usual software engineering quality attributes
to such a codebase. How do we ensure cohesive modules, promote re-use, avoid duplication? These
questions become even more pressing if we attempt to "shift-left" and create a platform of
self-service that others can just pickup and use.

The answer, or at least part of it, is component libraries of re-usable tasks and/or entire workflows.

- Azure DevOps Pipelines has [resources](https://learn.microsoft.com/en-us/azure/devops/pipelines/yaml-schema/resources-repositories-repository?view=azure-pipelines),
which may be utilised to download a component library of tasks to utilise
- GitLab CI has [components](https://docs.gitlab.com/ci/components/), which allows you to define
a repository of tasks to reference with `include` statements
- GitHub Actions has both the Actions marketplace and GitHub Actions

Having a centralised, standardised, set of re-usable actions controlled within an organisation
is the first step in scaling CI/CD without the hassle. This project is thus a show-piece
for doing so with GitHub, via the usage of GitHub Actions.