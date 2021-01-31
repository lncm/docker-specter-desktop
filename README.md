# Specter Desktop in a docker container

[![Build on deploy](https://github.com/lncm/docker-specter-desktop/workflows/Docker%20build%20on%20tag/badge.svg)](https://github.com/lncm/docker-specter-desktop/actions?query=workflow%3A%22Docker+build+on+tag%22)
![Version](https://img.shields.io/github/v/release/lncm/docker-specter-desktop?sort=semver) 
[![Docker Pulls Count](https://img.shields.io/docker/pulls/lncm/specter-desktop.svg?style=flat)](https://hub.docker.com/r/lncm/specter-desktop)

[Specter Desktop](https://github.com/cryptoadvance/specter-desktop) by [cryptoadvance](https://cryptoadvance.io/) in an [auditable](https://github.com/lncm/docker-specter-desktop) docker [container](https://hub.docker.com/r/lncm/specter-desktop).

## Why?

So we can simplify things and make things easier also ease of running through connected devices (raspberry PIs) as this runs cross platform

## Building

To build yourself you can run

```
docker build -t nolim1t/specter-desktop:v1.1.0 . 
```

## Tags

> **NOTE:** For an always up-to-date list see: https://hub.docker.com/r/lncm/specter-desktop/tags

* `latest`
* `v1.0.0` `v1.1.0`
* `v0.10.4` `v0.10.2` `v0.10.1` `v0.10.0` 
* `v0.9.2` `v0.9.1`
* `v0.8.0` `v0.8.1`
* `v0.7.1` `v0.7.2`
* `v0.6.1` `v0.7.0`

## Maintainer release notes

The github action takes in the current tag from  [upstream](https://github.com/cryptoadvance/specter-desktop/tags)  but you will need to do a

```
git tag -s vtag.version
```

and then push the tag. Use of -s meaning the tag should be signed which is highly recommended that you do.

## Running

There are two ways you can run this

### From docker command

```bash
# in HWI bridge mode (meaning you would like to run a bridge to HWI)
# Also ensure that your username is permissioned for accessing the USB device. (group=plugdev) or use the --privileged switch
docker run --rm -v $HOME/.specter:/data/.specter lncm/specter-desktop:v1.1.0 --hwibridge

# Get the Help to see options
docker run --rm -v $HOME/.specter:/data/.specter lncm/specter-desktop:v1.1.0 --help

# Run in Daemon mode
docker run --rm -v $HOME/.specter:/data/.specter -v $HOME/.bitcoin:/data/.bitcoin lncm/specter-desktop:v1.1.0 --host your.ip.address --daemon

# Run in docker detached mode (so we can see the logs)
docker run -d=true --name=specter-desktop --rm -v $HOME/.specter:/data/.specter -v $HOME/.bitcoin:/data/.bitcoin lncm/specter-desktop:v1.1.0 --host your.ip.address

# with flask env file in root (Replace --help with other stuff
docker run --name=specter-desktop --network=host --rm -v $HOME/.specter:/data/.specter -v $HOME/.bitcoin:/data/.bitcoin -v $HOME/.flaskenv:/.flaskenv lncm/specter-desktop:v1.1.0 --help
```

### Docker compose

This is a bit complex but the idea is to make sure there is a bitcoind installation. Note that the IP needs to be specified (this is as per design by the specter project). However we probably can hack in an entrypoint to improve the flow of things going forward. 

I also used host networking for ease of use, and also added ```privileged``` for  further ease of use in case your user can't access the usb socket if you would like to run as a bridge to HWI or use the ```--hwibridge``` flag

Or you can use the sample docker-compose in HWIBridge mode [here](https://github.com/lncm/docker-specter-desktop/blob/master/docker-compose.yml.hwibridge)

```yaml
version: '3.8'
services:
        bitcoin:
                image: lncm/bitcoind:v0.20.1
                container_name: bitcoin
                volumes:
                        - ${PWD}/bitcoin:/root/.bitcoin
                        - ${PWD}/bitcoin:/data/.bitcoin
                        - ${PWD}/bitcoin:/data/bitcoin
                restart: on-failure
                ports:
                    - "8333:8333"
                    - "8332:8332"
                stop_grace_period: 20m30s
                network_mode: host
        specter:
                image: lncm/specter-desktop:v1.1.0
                container_name: specter-desktop
                privileged: true
                command: --host ip.addr
                restart: on-failure
                ports:
                    - "25441:25441"
                stop_grace_period: 5m30s
                network_mode: host                    
                volumes:
                        - ${PWD}/.bitcoin:/data/.bitcoin
                        - ${PWD}/.specter:/data/.specter
                        - /dev:/dev
                        - /etc/udev:/etc/udev
```

## Mirrors

### Gitlab

To support decentralization, we also have a [gitlab](https://gitlab.com/lncm/docker/spector-desktop) repository too, with [docker images on gitlab](https://gitlab.com/lncm/docker/spector-desktop/container_registry/1510240)

To use the docker image on gitlab, check out ```registry.gitlab.com/lncm/docker/spector-desktop:v0.10.0-3512dfd4```

## Troubleshooting

Please ensure that you have the correct [udev rules](https://github.com/lncm/docker-specter-desktop/blob/master/udevrules.md) installed

