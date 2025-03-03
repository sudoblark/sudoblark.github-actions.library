# plan - Terraform plan task

Run quality checks against terraform, in addition to outputting a plan, with results outputted
to a pipeline artefact ZIP file with the name {{ outputs.artefact_name }}, contents
of which are as follows:
  
- terraform.plan        : Binary terraform plan
- terraform.validate    : Results of terraform validation
- terraform.show        : Terraform plan in human-readable format
- terraform.json        : Terraform plan in JSON format, required for some downstream CLI tooling
- terraform.format      : List of files which have failed terraform format checks, else an empty file
- checkov.xml           : JUnit output of Checkov results, can be used to upload test results downstream

## Syntax

```yaml
- uses: sudoblark/sudoblark.github-actions.library/terraform/plan@<version>
  with:
    terraform_version: # string
    working_directory: #string
    artefact_prefix: #string
    #aws_region: #string
    #aws_access_key: #string
    #aws_secret_access_key: #string
```

## Inputs

`terraform_version` - Terraform Version

`string`. Required. Allowed values: any string value, but should be a semantic version of terraform to
actually work.

Semantic version of Terraform to utilise for the task.

---

`working_directory` - Working Directory

`string`. Required. Allowed values: any string value, but should path to terraform folder to actually work.

The working directory to utilise when performing the task.

---

`working_directory` - Working Directory

`string`. Required. Allowed values: any string value, but should path to terraform folder to actually work.

The working directory to utilise when performing the task.

---

`artefact_prefix` - Artefact prefix

`string`. Required. Allowed values: any string value.

Prefix to append to terraform-artefact produced by the task.

---

`aws_region` - AWS Region

`string`. Optional. Allowed values: any string value.

AWS_DEFAULT_REGION value, required if the hashicorp/aws provider is utilised.

---

`aws_access_key` - AWS Access key

`string`. Optional. Allowed values: any string value.

AWS_ACCESS_KEY_ID value, required if the hashicorp/aws provider is utilised.

---

`aws_secret_access_key` - AWS Secret Access key

`string`. Optional. Allowed values: any string value.

AWS_SECRET_ACCESS_KEY value, required if the hashicorp/aws provider is utilised.

## Outputs

`artefact_name` - Artefact name

`string`.

Name of the artefact which contains results of the plan operation.

## Remarks

N.A.

## Examples

Run a simple plan for AWS, discovering terraform version from a local .terraform-version file.

Source code available [here](https://github.com/sudoblark/sudoblark.terraform.modularised-demo/blob/main/.github/workflows/deploy.yaml).

```yaml
---
      - name: Auto-discover Terraform version
        run: |
          TERRAFORM_VERSION=$(cat infrastructure/sudoblark/.terraform-version)
          echo "TERRAFORM_VERSION=$TERRAFORM_VERSION" >> $GITHUB_ENV

      - uses: sudoblark/sudoblark.github-actions.library/terraform/plan@<version>
        with:
          terraform_version: $TERRAFORM_VERSION
          working_directory: $GITHUB_WORKSPACE/infrastructure/sudoblark
          artefact_prefix: sudoblark
          aws_region: eu-west-2
          aws_access_key: $AWS_ACCESS_KEY_ID
          aws_secret_access_key: $AWS_SECRET_ACCESS_KEY
```

Utilise the fact that `env` is still valid for composite actions to pass in AWS credentials this
way instead:

```yaml
      - uses: sudoblark/sudoblark.github-actions.library/terraform/plan@<version>
        with:
          terraform_version: $TERRAFORM_VERSION
          working_directory: $GITHUB_WORKSPACE/infrastructure/sudoblark
          artefact_prefix: sudoblark
        env:
          AWS_DEFAULT_REGION: eu-west-2
          AWS_ACCESS_KEY_ID: $AWS_ACCESS_KEY_ID
          AWS_SECRET_ACCESS_KEY: $AWS_SECRET_ACCESS_KEY
```

Use `env` to plan against Azure DevOps:

```yaml
      - uses: sudoblark/sudoblark.github-actions.library/terraform/plan@<version>
        with:
          terraform_version: $TERRAFORM_VERSION
          working_directory: $GITHUB_WORKSPACE/infrastructure/sudoblark
          artefact_prefix: sudoblark
        env:
          AZDO_PERSONAL_ACCESS_TOKEN: <TOKEN>
```

Matrix usage in order to plan against multiple environments, especially useful if you're using
a microservice approach and one repository is responsible for one application across all its
pertinent environments.


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

