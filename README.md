# NHoS - ubuntu-defaults-image

Builds ISO of an Ubuntu (/flavor) from a defaults package. This version has added support for repos hosted on packagecloud.io and multiple extra packages.

Based on [ubuntu-defaults-builder](https://launchpad.net/ubuntu/xenial/+package/ubuntu-defaults-builder) by Martin Pitt.

## Getting started

Basic usage follows the manpage for the source package. Here's an example using the NHoS PPA and defaults settings package.

```
./ubuntu-defaults-image --ppa nhos/ppa --release xenial --flavor ubuntu-gnome --package nhos-default-settings
```

Specify a repository on packagecloud.io with `--repo organisation/reponame`

```
./ubuntu-defaults-image --repo nhos/nhos-default-settings --release xenial --flavor ubuntu-gnome --package nhos-default-settings
```

Specify additional PPAs with `--ppa name/ppa`

```
./ubuntu-defaults-image --repo nhos/nhos-default-settings --release xenial --flavor ubuntu-gnome --package nhos-default-settings --ppa libreoffice/ppa
```

Specify additional packages `--xpackage packagename`

```
./ubuntu-defaults-image --repo nhos/nhos-default-settings --release xenial --flavor ubuntu-gnome --package nhos-default-settings --ppa libreoffice/ppa --xpackage libreoffice-style-breeze
```

## Create NHoS ISOs - command line

Included is the NHoS build helper `build.sh`, a wrapper for ubuntu-defaults-image. You can use this script to create the NHoS ISOs for yourself. It is the same script used by Travis CI to automagically create the NHoS ISOs.

### Usage

To create 64bit ISO

    sudo -E BUILDARCH=amd64 ./build.sh

To create 32bit ISO

    sudo -E BUILDARCH=i386 ./build.sh

### Our patch to live-build

The helper script `build.sh` patches the live-build configuration file `lb_binary_disk` to use the variable `$LB_ISO_VOLUME` for `.disk/info` during the finalisation of the ISO images.

## Create NHoS ISOs - Dockerised

You can use our Docker container to build NHoS ISOs.

```
docker pull nhos/ubuntu-defaults-image:latest
docker run --env BUILDARCH=amd64 --rm -it -v `pwd`:`pwd` -w `pwd` --name buildamd64 --privileged nhos/ubuntu-defaults-image /bin/bash -c "./build.sh"
docker run --env BUILDARCH=i386 --rm -it -v `pwd`:`pwd` -w `pwd` --name buildi386 --privileged nhos/ubuntu-defaults-image /bin/bash -c "./build.sh"
```

## Travis CI [![Build Status](https://travis-ci.org/nhos-project/ubuntu-defaults-image.svg?branch=master)](https://travis-ci.org/nhos-project/ubuntu-defaults-image)

In the small hours of the morning Travis gets busy building NHoS ISOs for us.

## Authors

- [Rob Dyke](https://github.com/robdyke) - added support for _packagecloud.io_ and _extra packages_.

## License

This project is licensed under the GNU GENERAL PUBLIC LICENSE v3. See the LICENSE file for details.

## Acknowledgments

- Based on [ubuntu-defaults-builder](https://launchpad.net/ubuntu/xenial/+package/ubuntu-defaults-builder) by Martin Pitt.
