# apply - Terraform apply task

This template simply takes the artefact produced by the plan template and applies it.

Intended to be used in conjunction with sudoblark.github-actions.library/terraform/plan to produce
a suitable artefact for application.

## Syntax

```yaml
- uses: sudoblark/sudoblark.github-actions.library/terraform/apply@<version>
  with:
    terraform_version: # string
    working_directory: #string
    artefact: #string
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

`artefact` - Artefact name

`string`. Required. Allowed values: any string value.

Full name of the workflow artefact to download which contains produced plan binary files.

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

N.A.

## Remarks

N.A.

## Examples

A full continuous delivery workflow, requiring approval before application of the produced plan artefact.

Source code available [here](https://github.com/sudoblark/sudoblark.terraform.modularised-demo/blob/main/.github/workflows/deploy.yaml).

```yaml
---
name: sudoblark.terraform.modularised-demo/deployment/sudoblark/deploy
env:
  AWS_ACCESS_KEY_ID: ${{ secrets.SUDOBLARK_AWS_ACCESS_KEY_ID }}
  AWS_SECRET_ACCESS_KEY: ${{ secrets.SUDOBLARK_AWS_ACCESS_KEY_VALUE }}
  AWS_DEFAULT_REGION: eu-west-2
  REPO_GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  ORG_GITHUB_TOKEN: ${{ secrets.SUDOBLARK_GITHUB_TOKEN }}

on:
  workflow_dispatch:
    inputs:
      apply:
        description: "If we should apply the terraform"
        type: boolean
        default: false

permissions:
  issues: write

jobs:
  plan:
    name: Run Terraform plan
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v3
        env:
          GITHUB_TOKEN: ${{ env.REPO_GITHUB_TOKEN }}

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

  approval:
    name: Wait for approval
    runs-on: ubuntu-20.04
    needs: plan
    if: ${{ success() && inputs.apply  }}
    steps:
    - uses: trstringer/manual-approval@v1
      with:
        secret: ${{ env.REPO_GITHUB_TOKEN }}
        approvers: benjaminlukeclark
        minimum-approvals: 1
        issue-title: "Deploying sudoblark.terraform.modularised-demo to sudoblark"
        issue-body: "Please approve or deny the deployment."
        exclude-workflow-initiator-as-approver: false

  apply:
    name: Terraform apply
    runs-on: ubuntu-20.04
    needs: approval
    steps:
    - uses: actions/checkout@v3
      env:
        GITHUB_TOKEN: ${{ env.REPO_GITHUB_TOKEN }}

    - name: Auto-discover Terraform version
      run: |
        TERRAFORM_VERSION=$(cat infrastructure/sudoblark/.terraform-version)
        echo "TERRAFORM_VERSION=$TERRAFORM_VERSION" >> $GITHUB_ENV
      shell: bash

    - name: ZIP lambdas
      run: |
        cd application/unzip-lambda/unzip_lambda
        zip -r lambda.zip lambda_function.py
        mkdir src
        mv lambda.zip src
      shell: bash

    - uses: sudoblark/sudoblark.github-actions.library/terraform/apply@<version>
      with:
        terraform_version: $TERRAFORM_VERSION
        working_directory: $GITHUB_WORKSPACE/infrastructure/sudoblark
        artefact: sudoblark-terraform-artefact
        aws_region: eu-west-2
        aws_access_key: $AWS_ACCESS_KEY_ID
        aws_secret_access_key: $AWS_SECRET_ACCESS_KEY
```

Utilise the fact that `env` is still valid for composite actions to pass in AWS credentials this
way instead:

```yaml
      - uses: sudoblark/sudoblark.github-actions.library/terraform/apply@<version>
        with:
          terraform_version: $TERRAFORM_VERSION
          working_directory: $GITHUB_WORKSPACE/infrastructure/sudoblark
          artefact_prefix: sudoblark-terraform-artefact
        env:
          AWS_DEFAULT_REGION: eu-west-2
          AWS_ACCESS_KEY_ID: $AWS_ACCESS_KEY_ID
          AWS_SECRET_ACCESS_KEY: $AWS_SECRET_ACCESS_KEY
```

Use `env` to apply against Azure DevOps:

```yaml
      - uses: sudoblark/sudoblark.github-actions.library/terraform/apply@<version>
        with:
          terraform_version: $TERRAFORM_VERSION
          working_directory: $GITHUB_WORKSPACE/infrastructure/sudoblark
          artefact_prefix: sudoblark-terraform-artefact
        env:
          AZDO_PERSONAL_ACCESS_TOKEN: <TOKEN>
```
