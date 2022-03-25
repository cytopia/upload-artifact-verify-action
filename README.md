# Upload artifact verify action

[![GitHub release](https://img.shields.io/github/release/cytopia/upload-artifact-verify-action.svg?logo=github)](https://github.com/cytopia/upload-artifact-verify-action/releases/latest)
[![GitHub marketplace](https://img.shields.io/badge/marketplace-upload--artifact--verify--action-blue?logo=github)](https://github.com/marketplace/actions/upload-artifact-verify-action)
[![](https://img.shields.io/badge/github-cytopia%2Fupload--artifact--verify--action-red.svg?logo=github)](https://github.com/cytopia/upload-artifact-verify-action "github.com/cytopia/upload-artifact-verify-action")
[![test](https://github.com/cytopia/upload-artifact-verify-action/actions/workflows/test.yml/badge.svg)](https://github.com/cytopia/upload-artifact-verify-action/actions/workflows/test.yml)

This action allows you to upload an artifact. It will then download it in the background to ensure the upload was successful.


## :arrow_forward: Inputs

The following inputs can be used to alter the Docker tag name determination:

| Input     | Required | Default | Description                                |
|-----------|----------|----------|-------------------------------------------|
| `name`    | Yes      | ``       | The filename of the uploaded file         |
| `path`    | Yes      | ``       | The local file to upload.                 |


## :arrow_backward: Outputs

None


## :computer: Usage

```yaml
on: [push]

jobs:
  job1:
    runs-on: ubuntu-latest
    name: Pull docker image
    steps:

      - name: Checkout repository
        uses: actions/checkout@v2
        with:
          fetch-depth: 0

      - name: Create artifact
        id: file
        shell: bash
        run: |
          PRE_RAND="$( ${RANDOM} | md5sum | head -c 20;  )"
          PRE_DATE="$( date '+%s' )"
          NAME="${PRE_RAND}-${PRE_DATE}.txt"
          echo "somedata" > "${NAME}"
          echo "::set-output name=path::${NAME}"

      - name: upload artifact
        uses: cytopia/upload-artifact-verify@v0.1.0
        uses: ./
        with:
          name: ${{ steps.file.outputs.path }}
          path: ${{ steps.file.outputs.path }}
```


## :exclamation: Keep up-to-date with GitHub Dependabot

Since [Dependabot](https://docs.github.com/en/github/administering-a-repository/keeping-your-actions-up-to-date-with-github-dependabot) has [native GitHub Actions support](https://docs.github.com/en/github/administering-a-repository/configuration-options-for-dependency-updates#package-ecosystem), to enable it on your GitHub repo all you need to do is add the `.github/dependabot.yml` file:

```yml
version: 2
updates:
  # Maintain dependencies for GitHub Actions
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "daily"
```


## :octocat: [cytopia](https://github.com/cytopia) GitHub Actions

| Name                         | Description |
|------------------------------|-------------|
| [docker-tag-action]          | Determines Docker tags based on git branch, commit or git tag |
| [git-ref-matrix-action]      | Create stringified JSON list of git refs to be used as a build matrix |
| [shell-command-retry-action] | Retries shell commands to avoid failing pipelines due to network issues |

[docker-tag-action]: https://github.com/cytopia/docker-tag-action
[git-ref-matrix-action]: https://github.com/cytopia/git-ref-matrix-action
[shell-command-retry-action]: https://github.com/cytopia/shell-command-retry-action


## :page_facing_up: License

**[MIT License](LICENSE)**

Copyright (c) 2022 [cytopia](https://github.com/cytopia)
