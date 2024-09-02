## Introduction

This pipeline will bump the version in package.json, create a new tag version, update the changelog and push the commit with --follow-tags! This package uses the same pipeline for versioning.

## Submodule VS Git Submodule

The action will work for git sub modules and modules

Context Submodule : You can publish all registries with the same version

Context Git Submodule : You can publish with different versions

## parameters

- secret_input: "Github Secret"
- gh_pat: "Full accesss auth token"
- release_as: 'Bump version kind'
- publish_type: 'Publish type'
- push_to: "Push to new branch after releasing"
- pre_action: "Allowed values: checkout, none"
- should_build: "Run yarn build automatically to generate dist"
- custom_publish_command_name: "publish command for monorepo packages"

## usage example

you can implement this custom action in your service by adding :

```yaml
name: ci-release

permissions:
  actions: write
  checks: write
  contents: write
  deployments: write
  id-token: write
  issues: write
  discussions: write
  packages: write
  pages: write
  pull-requests: write
  repository-projects: write
  security-events: write
  statuses: write

on:
  workflow_dispatch:
    inputs:
      release_as:
        description: "Bump version kind"
        required: true
        default: "patch"
        type: choice
        options:
          - patch
          - minor
          - major

jobs:
  release:
    runs-on: ubuntu-22.04
    steps:
      - name: release package
        uses: xotosphere/xotopipe-release-yarn@v0.0.1
        with:
          as: ${{ inputs.release_as }}
          registry: "true"
          github_token: "${{ secrets.GITHUB_TOKEN }}"
          access_token: "token"
          build: "true"
```
