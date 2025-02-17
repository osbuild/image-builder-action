# Build OS images in GitHub Actions using [image-builder](https://github.com/osbuild/image-builder-cli)

This GitHub Action builds OS images using the [image-builder-cli](https://github.com/osbuild/image-builder-cli) container to build operating system artifacts (disk images, ISOs, etc.)

## Prerequisites

- Building images requires privileges, so make sure that you are not running the job triggering this action in [a container](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobsjob_idcontainer).
- `podman` has to be installed (it is available on the hosted GitHub runners)

## Inputs

Run `podman run --rm --privileged ghcr.io/osbuild/image-builder-cli:latest list-images` to get the matrix of valid distributions and image types.

- **distro** (string, required):
  The Linux distribution version to build for. Example: `fedora-41`.

- **image_type** (string, required):
  The type of image to build. Example: `qcow2`.

- **image_build_ref** (string, optional)
  A reference to the image-builder container image to use for the build. Default: `ghcr.io/osbuild/image-builder-cli:latest`

## Example Workflow

Below is an example of how you might use this action in a GitHub workflow:

```yaml
name: Build a Fedora 41 minimal raw image

on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Run Image Builder Action
        uses: osbuild/image-builder-action@v1
        with:
          distro: fedora-41
          image_type: minimal-raw

      - name: Upload Build Artifact
        uses: actions/upload-artifact@v4
        with:
          name: os-image
          path: output/fedora-41-minimal-raw-x86_64/xz/disk.raw.xz
```

## Project

 * **Website**: https://www.osbuild.org
 * **Bug Tracker**: https://github.com/osbuild/image-builder-action/issues
 * **Discussions**: https://github.com/orgs/osbuild/discussions
 * **Matrix**: #image-builder on [fedoraproject.org](https://matrix.to/#/#image-builder:fedoraproject.org)

## License

 - **Apache-2.0**
 - See LICENSE file for details.
