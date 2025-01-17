# viam-cartographer

## (In)stability Notice
> **Warning**
> This is an experimental feature. Stability is not guaranteed. Breaking changes are likely to occur, and occur often.

## Overview

This repo wraps [Cartographer](https://github.com/cartographer-project/cartographer) as a [modular resource](https://docs.viam.com/program/extend/modular-resources/) so it is easily usable with the rest of Viam's ecosystem. Cartographer is a system that provides real-time Simultaneous Localization
And Mapping ([SLAM](https://en.wikipedia.org/wiki/Simultaneous_localization_and_mapping)) in 2D and 3D across multiple platforms and sensor configurations.

## Getting started

### Stable AppImages

Install viam-cartographer:

* Linux aarch64:
    ```bash
    sudo curl -o /usr/local/bin/carto_grpc_server http://packages.viam.com/apps/slam-servers/carto_grpc_server-stable-aarch64.AppImage
    sudo chmod a+rx /usr/local/bin/carto_grpc_server
    ```
 * Linux x86_64:
    ```bash
    sudo curl -o /usr/local/bin/carto_grpc_server http://packages.viam.com/apps/slam-servers/carto_grpc_server-stable-x86_64.AppImage
    sudo chmod a+rx /usr/local/bin/carto_grpc_server
    ```
* MacOS/Linux
    ```bash
    brew tap viamrobotics/brews && brew install carto-grpc-server
    ```

For next steps, see the [Run Cartographer SLAM on your Robot with a LIDAR Tutorial](https://docs.viam.com/services/slam/run-slam-cartographer/).

## Development

You can either install the latest AppImages for testing, or build the code from source.

### Latest AppImages

You can install the latest AppImages using:
* Linux aarch64:
    ```bash
    sudo curl -o /usr/local/bin/carto_grpc_server http://packages.viam.com/apps/slam-servers/carto_grpc_server-latest-aarch64.AppImage
    sudo chmod a+rx /usr/local/bin/carto_grpc_server
    ```
 * Linux x86_64:
    ```bash
    sudo curl -o /usr/local/bin/carto_grpc_server http://packages.viam.com/apps/slam-servers/carto_grpc_server-latest-x86_64.AppImage
    sudo chmod a+rx /usr/local/bin/carto_grpc_server
    ```

### Build from source

#### Download
```bash
git clone --recurse-submodules https://github.com/viamrobotics/viam-cartographer.git
```

If you happened to use `git clone` only, you won't see the `cartographer` folder and will need to fetch it:

`git submodule update --init`

### (Optional) Using Canon Images

If desired, Viam's canon tool can be used to create a docker container to build `arm64` or `amd64` binaries of the SLAM server. The canon tool can be installed by running the following command: 

```bash
go install github.com/viamrobotics/canon@latest
```

And then by running one of the following commands in the viam-cartographer repository to create the container:

```bash
canon -arch arm64
```

```bash
canon -arch amd64
```

These containers are set to persist between sessions via the `persistent` parameter in the `.canon.yaml` file located in the root of viam-cartographer. More details regarding the use of Viam's canon tool can be found [here](https://github.com/viamrobotics/canon).

#### Setup, build, and run the binary

```bash
# Setup the gRPC files
make bufinstall && make buf 
# Install dependencies
make setup
# Build & install the binary
make build
sudo cp viam-cartographer/build/carto_grpc_server /usr/local/bin
# Run the binary
carto_grpc_server
```

### Linting

```bash
make lint-setup
make lint
```
### Testing

```bash
make test
```
### Working with submodules

#### Commit and push
1. Commit and push changes in the `cartographer` submodule first.
2. Commit and push changes in the `viam-cartographer` library last.

Or, alternatively:
1. Commit changes in the `cartographer` submodule
2. Commit changes in the main repo
3. Push all changes by running `git push --recurse-submodules=on-demand`

#### Changing branches in a submodule
When changing branches in a submodule, update `.gitmodules`, e.g., changing to a branch called `kk/fix-install`:

```bash
...
[submodule "viam-cartographer/cartographer"]
        path = viam-cartographer/cartographer
        url = git@github.com:kkufieta/cartographer.git
        branch=kk/fix-install
```

Commit & push the changes.

When pulling those changes, run the following:
```bash
git pull
git submodule update --init --recursive
```

## License
Copyright 2023 Viam Inc.

Apache 2.0 - See [LICENSE](https://github.com/viamrobotics/slam/blob/main/LICENSE) file

