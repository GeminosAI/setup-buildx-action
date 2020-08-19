[![GitHub release](https://img.shields.io/github/release/docker/setup-buildx-action.svg?style=flat-square)](https://github.com/docker/setup-buildx-action/releases/latest)
[![GitHub marketplace](https://img.shields.io/badge/marketplace-docker--setup--buildx-blue?logo=github&style=flat-square)](https://github.com/marketplace/actions/docker-setup-buildx)
[![CI workflow](https://img.shields.io/github/workflow/status/docker/setup-buildx-action/ci?label=ci&logo=github&style=flat-square)](https://github.com/docker/setup-buildx-action/actions?workflow=ci)
[![Test workflow](https://img.shields.io/github/workflow/status/docker/setup-buildx-action/test?label=test&logo=github&style=flat-square)](https://github.com/docker/setup-buildx-action/actions?workflow=test)

## About

GitHub Action to set up Docker [Buildx](https://github.com/docker/buildx).

___

* [Usage](#usage)
  * [Quick start](#quick-start)
  * [With QEMU](#with-qemu)
* [Customizing](#customizing)
  * [inputs](#inputs)
  * [outputs](#outputs)
  * [environment variables](#environment-variables)
* [Limitation](#limitation)

## Usage

### Quick start

```yaml
name: ci

on:
  push:

jobs:
  buildx:
    runs-on: ubuntu-latest
    steps:
      -
        name: Checkout
        uses: actions/checkout@v2
      -
        name: Set up Docker Buildx
        id: buildx
        uses: docker/setup-buildx-action@v1
        with:
          version: latest
      -
        name: Builder instance name
        run: echo ${{ steps.buildx.outputs.name }}
      -
        name: Available platforms
        run: echo ${{ steps.buildx.outputs.platforms }}
```

### With QEMU

If you want support for more platforms you can use our [setup-qemu](https://github.com/docker/setup-qemu-action) action:

```yaml
name: ci

on:
  push:

jobs:
  buildx:
    runs-on: ubuntu-latest
    steps:
      -
        name: Checkout
        uses: actions/checkout@v2
      -
        name: Set up QEMU
        uses: docker/setup-qemu-action@v1
        with:
          platforms: all
      -
        name: Set up Docker Buildx
        id: buildx
        uses: docker/setup-buildx-action@v1
        with:
          version: latest
      -
        name: Available platforms
        run: echo ${{ steps.buildx.outputs.platforms }}
```

## Customizing

### inputs

Following inputs can be used as `step.with` keys

| Name               | Type    | Default                           | Description                        |
|--------------------|---------|-----------------------------------|------------------------------------|
| `version`          | String  |                                   | [Buildx](https://github.com/docker/buildx) version. (e.g. `v0.3.0`, `latest`) |
| `driver`           | String  | `docker-container`                | Sets the [builder driver](https://github.com/docker/buildx#--driver-driver) to be used |
| `driver-opt`       | String  |                                   | Passes additional [driver-specific options](https://github.com/docker/buildx#--driver-opt-options) |
| `buildkitd-flags`  | String  |                                   | [Flags for buildkitd](https://github.com/moby/buildkit/blob/master/docs/buildkitd.toml.md) daemon |
| `install`          | Bool    | `false`                           | Sets up `docker build` command as an alias to `docker buildx` |
| `use`              | Bool    | `true`                            | Switch to this builder instance |

### outputs

Following outputs are available

| Name          | Type    | Description                           |
|---------------|---------|---------------------------------------|
| `name`        | String  | Builder instance name |
| `platforms`   | String  | Available platforms (comma separated) |

### environment variables

The following [official docker environment variables](https://docs.docker.com/engine/reference/commandline/cli/#environment-variables) are supported:

| Name            | Type    | Default      | Description                                    |
|-----------------|---------|-------------|-------------------------------------------------|
| `DOCKER_CONFIG` | String  | `~/.docker` | The location of your client configuration files |

## Limitation

This action is only available for Linux [virtual environments](https://docs.github.com/en/actions/reference/virtual-environments-for-github-hosted-runners#supported-virtual-environments-and-hardware-resources).