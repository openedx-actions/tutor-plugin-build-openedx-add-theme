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


## Usage:


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
        uses: openedx-actions/tutor-k8s-init

      # THIS ACTION:
      #   repository-token is an optional input. Default value is ''
      - name: Add a custom theme
        uses: openedx-actions/tutor-plugin-build-openedx-add-theme
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
        uses: openedx-actions/tutor-plugin-build-openedx
```
