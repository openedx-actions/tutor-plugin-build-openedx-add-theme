<img src="https://avatars.githubusercontent.com/u/40179672" width="75">

[![Tests](https://github.com/openedx-actions/tutor-plugin-build-openedx-add-theme/actions/workflows/testRelease.yml/badge.svg)](https://github.com/openedx-actions/tutor-plugin-build-openedx-add-theme/actions)
[![Open edX Discussion](https://img.shields.io/static/v1?logo=discourse&label=Forums&style=flat-square&color=000000&message=discuss.openedx.org)](https://discuss.openedx.org/)
[![docs.tutor.overhang.io](https://img.shields.io/static/v1?logo=readthedocs&label=Documentation&style=flat-square&color=blue&message=docs.tutor.overhang.io)](https://docs.tutor.overhang.io)
[![hack.d Lawrence McDaniel](https://img.shields.io/badge/hack.d-Lawrence%20McDaniel-orange.svg)](https://lawrencemcdaniel.com)<br/>
[![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)

# tutor-plugin-build-openedx-add-theme

Github Action that downloads and adds a custom Open edX theme repository to your tutor configuration.

This action was originally created to work seamlessly as a supporting action for [openedx-actions/tutor-plugin-build-openedx](https://github.com/openedx-actions/tutor-plugin-build-openedx) but it should also work with your own custom workflows.

## Usage

```yaml
name: Example workflow

on: workflow_dispatch

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      # required antecedent
      - uses: actions/checkout

      # required antecedent
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials
        with:
          aws-access-key-id: ${{ secrets.THE_NAME_OF_YOUR_AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.THE_NAME_OF_YOUR_AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-2

      # install and configure tutor
      - name: Configure Github workflow environment
        uses: openedx-actions/tutor-k8s-init@v1

      # THIS ACTION:
      #   repository-token is an optional input. Default value is ''
      - name: Add a custom theme
        uses: openedx-actions/tutor-plugin-build-openedx-add-theme@v1
        with:
          repository: edx-theme-example
          repository-organization: lpm0073
          repository-ref: master
          repository-token: ${{ secrets.PAT }}

      #
      # ... more configuration stuff ...
      #

      # Build your openedx container
      - name: Build the image and upload to AWS ECR
        uses: openedx-actions/tutor-plugin-build-openedx@v1
```

## Contributing

Pull requests are welcome! Please note that this repository uses [semantic release](https://github.com/semantic-release/semantic-release) for automated processessing of commits and pull requests, and package publication for new releases. Please note the following about your commit message:

- pull requests can be approved and merged by any **two** authorized core committers
- only the 'next' branch can be merged to main. Thus, your Pull Request should be created from the 'next' branch
- we use [Angular commit message format](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#-commit-message-format). See below
- use the imperative, present tense: "change" not "changed" nor "changes"
- don't capitalize the first letter
- no dot (.) at the end

### Branches associated with CI automation

| Branch     | Description                                                                                           |
|:-----------|:------------------------------------------------------------------------------------------------------|
| main       | commits are prohibited. Only accepts automated merges via Github Actions                              |
| next       | this is the branch that I (Lawrence) primarly use for normal code maintenance                         |
| next-major | special use, in the unlikely event that we ever bump beyond version 1.x.x                             |
| beta       | if you're working on something large then merge here before doing anything in 'next'                  |
| alph       | if you're doing some really big then start here                                                       |

### About the Angular commit message format

An example:

```bash
  git commit -m "fix: fix bug in the yadda yadda step"
```

Your commit message should be prefixed with one of the following:

| Prefix   | Description                                                                                           |
|:---------|:------------------------------------------------------------------------------------------------------|
| build    | changes that affect the build system or external dependencies (example - scopes: gulp, broccoli, npm) |
| ci       | changes to our CI configuration files and scripts (examples: Github Actions, CircleCi, SauceLabs)     |
| docs     | documentation only changes                                                                            |
| feat     | a new feature                                                                                         |
| fix      | a bug fix                                                                                             |
| perf     | a code change that improves performance                                                               |
| refactor | a code change that neither fixes a bug nor adds a feature                                             |
| test     | adding missing tests or correcting existing tests                                                     |

More generally, less is more: don't use two words where one will suffice. Simple words are better than fancy words.
