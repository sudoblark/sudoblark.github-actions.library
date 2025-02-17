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

This repo contains a setup of re-usable GitHub Actions [workflows](https://docs.github.com/en/actions/sharing-automations/reusing-workflows).

Specifically it is a library of semantically versioned](https://semver.org) components, intended to be used as off-the-shelf, standardised,
building blocks for CI/CD processes involving GitHub Actions.

It fulfills the same function that [sudoblark.azure-devops.library](https://github.com/sudoblark/sudoblark.azure-devops.library)
does for Azure DevOps Pipelines.

<p align="right">(<a href="#readme-top">back to top</a>)</p>


### Built With

* [TODO](TODO)

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- GETTING STARTED -->
## Getting Started

Creating new re-usable workflows it as simple as writing a YAML file. However, it's important to keep
a few things in mind:
- YAML files should denote _what_ the task does simply and plainly
- The folder structure should denote what the task interacts with
- Re-usable workflows require the following `on` value at minimum:

```yaml
on:
  workflow_call:
```

- Workflows should be generalised, re-usable, and with a well-defined interface. Generally this means:
  - Think about how the action(s) you're defining may be used in more than just the specific use-case you're trying to fulfill
  - Avoid hard-coding, use inputs where possible.
  - Document your inputs, make them discere where possible
  - Document your outputs

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- USAGE EXAMPLES -->
## Usage

In order to use a task within a pipeline, simply:

- Identify the job(s) you wish to reference
- Identify their interface
  - i.e. what are the inputs
- Call that job(s) in your own pipeline, for example:

```yaml
jobs:
  TerraformPlan:
    strategy:
      matrix:
        target: [dev, stage, prod]
    uses: sudoblark/sudoblark.github-actions.library/terraform/plan.yml@1.0.0
    with:
      target: ${{ matrix.target }}
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