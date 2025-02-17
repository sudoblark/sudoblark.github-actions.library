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
        <li><a href="#built-with">Built With</a></li>
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

This repo contains a setup of re-usable GitHub composition [actions](https://docs.github.com/en/actions/sharing-automations/creating-actions/creating-a-composite-action).

Specifically it is a library of semantically versioned](https://semver.org) actions, intended to be used as off-the-shelf, standardised,
building blocks for CI/CD processes involving GitHub Actions.

It fulfills the same function that [sudoblark.azure-devops.library](https://github.com/sudoblark/sudoblark.azure-devops.library)
does for Azure DevOps Pipelines.

In the instance that several of these composite actions, when combined, fulfill a use-case this is either done via:

- Direct instantation in a caller repo
- As a re-usable workflow via [sudoblark.github-actions.workflows](https://github.com/sudoblark/sudoblark.github-actions.workflows)

<p align="right">(<a href="#readme-top">back to top</a>)</p>


### Built With

* [TODO](TODO)

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

In order to use a task within a pipeline, simply:

- Identify the actions(s) you wish to reference
- Identify their interface
  - i.e. what are the inputs and outputs
- Figure out how this interacts with your intended use-case
- Call those action(s) in your own pipeline, for example:

```yaml
on: [pull_request]

jobs:
  terraform_quality_checks:
    runs-on: ubuntu-latest
    name: A simple job to validate that pushed terraform is valid for all environment in the repo
    strategy:
      matrix:
        ENVIRONMENTS: [
          {
            terraform_version: 1.5.1,
            folder: dev,
            prefix: dev,
            aws_region: eu-west-2
          },
          {
            terraform_version: 1.8.1,
            folder: test,
            prefix: test,
            aws_region: eu-west-1
          }
          {
            terraform_version: 1.10.1,
            folder: prod,
            prefix: prod,
            aws_region: us-east-1
          }
        
        ]
    steps:
      - uses: actions/checkout@v4
      - uses: sudoblark/sudoblark.github-actions.library/terraform/plan@1.0.0
        with:
          terraform_version: ${{ matrix.ENVIRONMENTS.terraform_version }}
          working_directory: infrastructure/${{ matrix.ENVIRONMENTS.folder }}
          artefact_prefix: ${{ matrix.ENVIRONMENTS.prefix }}
          aws_region: ${{ matrix.ENVIRONMENTS.aws_region }}
          ## Assumed singular IAM role that can assume roles in other accounts
          aws_access_key: ${{ secrets.ACCESS_KEY_ID }}
          aws_secret_access_key: ${{ secrets.ACCESS_KEY_VALUE }}
```

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

<p align="right">(<a href="#readme-top">back to top</a>)</p>