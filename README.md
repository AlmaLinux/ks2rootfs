# Kickstart to RootFS Builder

This project provides the ability build `rootfs` file from `kickstart` input file in a docker/podman container environment. Output rootfs files can be used create base images for different OSs (AlmaLinux, Cent OS, Rocky Linux) etc.

## Building local

Two different variants of the image `almalinux/ks2rootfs` are avilable. 

* [**`latest`**](https://hub.docker.com/r/almalinux/ks2rootfs/tags?page=1&ordering=last_updated&name=latest)
* [**`livecd-tools`**](https://hub.docker.com/r/almalinux/ks2rootfs/tags?page=1&ordering=last_updated&name=livecd-tools)

### Building `almalinux/ks2rootfs:latest` image tag

Use follwing command `latest` tag image to create `rootfs` files for `docker` and `WSL` images.

```sh
docker build -t almalinux/ks2rootfs -f Dockerfile .
```

### Building `almalinux/ks2rootfs:livecd-tools` image tag

Use follwing command `livecd-tools` tag image to create [`SIG/LiveMedia ISO`](https://github.com/AlmaLinux/sig-livemedia) images, check GitHub [repo](https://github.com/AlmaLinux/sig-livemedia).

```sh
docker build -t almalinux/ks2rootfs:livecd-tools -f Dockerfile.livecd .
```

## Using `almalinux/ks2rootfs:latest` image tag

Use `latest` tag image to create `rootfs` files for `docker` and `WSL` images.

### Using `lastet` Image tag

Run following command under `tests` folder. Test script kickstart files are available at `kickstarts`. **NOTE**: These scripts test use only. Official docker image kickstart files are avaialble in GitHub [docker-images](https://github.com/AlmaLinux/docker-images) repository.

Use command below to create `default` docker files

```sh
docker run --rm --privileged -v "$PWD:/build:z" \
    -e KICKSTART_FILE=kickstarts/almalinux-8-default.x86_64.ks \
    -e IMAGE_NAME=almalinux-8-docker-default.x86_64.tar.gz \
    -e OUTPUT_DIR=default \
    -e BUILD_TYPE=default \
    almalinux/ks2rootfs
```

Use command below to create `base` docker files

```sh
docker run --rm --privileged -v "$PWD:/build:z" \
    -e KICKSTART_FILE=kickstarts/almalinux-8-base.x86_64.ks \
    -e IMAGE_NAME=almalinux-8-docker-base.x86_64.tar.gz \
    -e OUTPUT_DIR=base \
    -e BUILD_TYPE=base \
    almalinux/ks2rootfs
```

Use command below to create `init` docker files

```sh
docker run --rm --privileged -v "$PWD:/build:z" \
    -e KICKSTART_FILE=kickstarts/almalinux-8-init.x86_64.ks \
    -e IMAGE_NAME=almalinux-8-docker-init.x86_64.tar.gz \
    -e OUTPUT_DIR=init \
    -e BUILD_TYPE=init \
    almalinux/ks2rootfs
```

### Environment variables

Container startup script `ks2rootfs` supports multiple environment varible to customize the output. The environment variables and their use as follows

```sh
ENVIRONMENT VARIABLES:
======================

KICKSTART_FILE  : Reuired - Input kickstart source file (.ks)
IMAGE_NAME     : Required - Rootfs output file name 

BUILD_WORK_DIR   : Optional - Working dir for kickstart source and image destination. Defaults to current directory.
OUTPUT_DIR     : Optional - Output directory name in working directory. Ddefault value is 'result'.
FLAG_OUTOUT_IN_PWD : Optional - Set this flag to true to write output files in current working directory. Default value is 'false'. When value is set to 'true', any value passed to 'OUTPUT_DIR' will be ignored.
FLAG_WRITE_META    : Optional - Generate meta data about the kickstart build system. Default value is 'true'.
FLAG_RETAIN_LOG    : Optional - When enabled, generated logs files retained under 'logs' output directory. Default value is 'false'.
BUILD_COMPTYPE     : Optional - Build compression type default 'xz', 'gzip' and 'lzma'.
BUILD_TYPE         : Optional - Build type 'base', 'default', 'init', 'micro', 'minimal' and 'wsl', Default value is 'default'.

USAGE:
    ks2rootfs KICKSTART_FILE_NAME ROOTFS_FILE_NAME

EXAMPLES:
    ks2rootfs os-minimal.ks os-minimal.tar.gz
```

## Using `almalinux/ks2rootfs:livecd-tools` Image

`livecd-tools` tag image to create [`SIG/LiveMedia ISO`](https://github.com/AlmaLinux/sig-livemedia) images, check GitHub [repo](https://github.com/AlmaLinux/sig-livemedia) for more detailed use.
