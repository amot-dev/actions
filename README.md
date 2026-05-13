# Using these Actions & Workflows


## Actions
None so far.


## Workflows


### [`ghcr.yml`](.github/workflows/ghcr.yml):
> Builds docker images from the top-level directory, tagging them with semantic versioning.

Inputs:
```yml
image_name: # ghcr.io/{image_name}
  required: false
  type: string
  default: ${{ github.repository }}
build_args: # Passed directly to docker/build-push-action
  required: false
  type: string
  default: ""
```

Usage:
```yml
name: Docker Build to GHCR
run-name: Build version ${{ github.ref_name }} by @${{ github.actor }}

on:
  push:
    tags: ['v*']

jobs:
  build-ghcr:
    uses: amot-dev/actions/.github/workflows/ghcr.yml@v1
    with:
      build_args: |
        MOTOTWIST_VERSION=${{ github.ref_name }} # Example
```


### [`close-issues.yml`](.github/workflows/close-issues.yml):
> Closes issues linked from a release via `#`. Comments on the issue and links back to the release.

Inputs:
```yml
release_body: # Text to parse for issue numbers
  required: true
  type: string
tag_name: # Tag that closed these issues
  required: true
  type: string
```

Usage:
```yml
on:
  release:
    types: [published]
  workflow_dispatch:
    inputs:
      manual_tag:
        description: 'Tag to process (e.g., v1.0.1)'
        required: true
      manual_body:
        description: 'Release notes / body to parse'
        required: true

jobs:
  close-issues:
    uses: amot-dev/actions/.github/workflows/close-issues.yml@v1
    with:
      tag_name: ${{ github.event.release.tag_name || inputs.manual_tag }}
      release_body: ${{ github.event.release.body || inputs.manual_body }}
```
