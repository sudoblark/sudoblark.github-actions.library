<a id="readme-top"></a>
<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/sudoblark/sudoblark.github-actions.library">
    <img src="docs/logo.png" alt="Logo" width="80" height="80">
  </a>

<h3 align="center">sudoblark.github-actions.library</h3>

  <p align="center">
    Template library of re-usable components for usage in GitHub Actions. - repo managed by sudoblark.terraform.github
  </p>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#technical-documentation">Technical Documentation</a></li>
      </ul>
    </li>
    <li><a href="#getting-started">Getting Started</a></li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project
[![CD](https://github.com/sudoblark/sudoblark.github-actions.library/actions/workflows/release.yaml/badge.svg)](https://github.com/sudoblark/sudoblark.github-actions.library/actions/workflows/release.yaml)
[![Version](https://img.shields.io/github/v/release/sudoblark/sudoblark.github-actions.library?label=action%20library&sort=semver)](https://github.com/sudoblark/sudoblark.github-actions.library/releases)
[![Docs](https://img.shields.io/badge/docs-latest-blue?logo=readthedocs)](https://sudoblark.github.io/sudoblark.github-actions.library/)
[![License](https://img.shields.io/github/license/sudoblark/sudoblark.github-actions.library)](https://github.com/sudoblark/sudoblark.github-actions.library/blob/main/LICENSE.txt)


This repo contains a setup of re-usable GitHub composition [actions](https://docs.github.com/en/actions/sharing-automations/creating-actions/creating-a-composite-action).

Specifically it is a library of semantically versioned](https://semver.org) actions, intended to be used as off-the-shelf, standardised,
building blocks for CI/CD processes involving GitHub Actions.

It fulfills the same function that [sudoblark.azure-devops.library](https://github.com/sudoblark/sudoblark.azure-devops.library)
does for Azure DevOps Pipelines.

In the instance that several of these composite actions, when combined, fulfill a use-case this is either done via:

- Direct instantation in a caller repo
- As a re-usable workflow via [sudoblark.github-actions.workflows](https://github.com/sudoblark/sudoblark.github-actions.workflows)

The live documentation base for this project is [here](https://sudoblark.github.io/sudoblark.github-actions.library/latest).

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- TECHNICAL DOCUMENTATION -->
### Technical Documentation

mkdocs is used in order to auto-generation documentation. It is configured via
the [docs/mkdocs.yml](./docs/mkdocs.yml) file and - for the most part - doesn't
need to be altered.

In order to generation a local web server of documentation:

```sh
mkdocs serve
```

However, it should be noted that live versioned documentation is produced via
the appropriate workflow as per the CI/CD section of this document.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->
## Getting Started

Creating new re-usable composite actions it as simple as writing a YAML file. However, it's important to keep
a few things in mind:
- YAML files should denote _what_ the task does simply and plainly
- The folder structure should denote what the task interacts with
- Composite actions should be generalised, re-usable, and with a well-defined interface. Generally this means:
  - Think about how the action(s) you're defining may be used in more than just the specific use-case you're trying to fulfill
  - Avoid hard-coding, use inputs where possible.
  - Document your inputs, make them discere where possible via `options`
  - Document your outputs

A simple truism is thus:

_If I can't tell what the composite action does via its documented inputs and outputs, you haven't
documented it well enough_

The beauty of such a core library - like any other software - is that if we maintain the same interface, we are free
to chop and change the underlying implementation(s) as we wish. Whilst maintaining all
the other benefits of a [component-based architecture](https://www.mendix.com/blog/what-is-component-based-architecture/).


<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- USAGE EXAMPLES -->
## Usage

See the live [docs](https://sudoblark.github.io/sudoblark.github-actions.library/latest)
for more details on usage.

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- LICENSE -->
## License

Distributed under the project_license. See `LICENSE.txt` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

* [Othneil Drew](https://github.com/othneildrew) for their amazing [Best README template](https://github.com/othneildrew/Best-README-Template) that was shameless used for this repo
* [codeql-action](https://github.com/github/codeql-action) for a robust example of multiple actions in one repository, something that is lacking from the official [docs](https://docs.github.com/en/actions/sharing-automations/creating-actions/creating-a-composite-action).

<p align="right">(<a href="#readme-top">back to top</a>)</p>
