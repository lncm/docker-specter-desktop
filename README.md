# Specter Desktop in a docker container

![Build on deploy](https://github.com/lncm/docker-specter-desktop/workflows/Docker%20build%20on%20tag/badge.svg)
![Version](https://img.shields.io/github/v/release/lncm/docker-specter-desktop?sort=semver) 
[![Docker Pulls Count](https://img.shields.io/docker/pulls/lncm/specter-desktop.svg?style=flat)](https://hub.docker.com/r/lncm/specter-desktop)

[Specter Desktop](https://github.com/cryptoadvance/specter-desktop) by [cryptoadvance](https://cryptoadvance.io/) in a docker container.

## Why?

So we can simplify things and make things easier (also try to build cross platform)

## Building

```
docker build -t nolim1t/specter-desktop:v0.6.1 . 
```

## Maintainer release notes

The github action takes in the current tag from  [upstream](https://github.com/cryptoadvance/specter-desktop/tags)  but you will need to do a

```
git tag -s vtag.version
```

and then push the tag. Use of -s meaning the tag should be signed.

## Running

### From docker command

```bash
# in HWI bridge mode
docker run --rm -v $HOME/.specter:/data/.specter lncm/specter-desktop:v0.6.1 --hwibridge

# Get the Help to see options
docker run --rm -v $HOME/.specter:/data/.specter lncm/specter-desktop:v0.6.1 --help

# Run in Daemon mode
docker run --rm -v $HOME/.specter:/data/.specter -v $HOME/.bitcoin:/data/.bitcoin lncm/specter-desktop:v0.6.1 --host your.ip.address --daemon

# Run in docker detached mode (so we can see the logs)
docker run -d=true --name=specter-desktop --rm -v $HOME/.specter:/data/.specter -v $HOME/.bitcoin:/data/.bitcoin lncm/specter-desktop:v0.6.1 --host your.ip.address

# with flask env file in root (Replace --help with other stuff
docker run --name=specter-desktop --network=host --rm -v $HOME/.specter:/data/.specter -v $HOME/.bitcoin:/data/.bitcoin -v $HOME/.flaskenv:/.flaskenv lncm/specter-desktop:v0.6.1 --help
```

### Docker compose

This is a bit complex but the idea is to make sure there is a bitcoind installation. Note that the IP needs to be specified (this is as per design by the specter project). However we probably can hack in an entrypoint to improve the flow of things.

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
                image: lncm/specter-desktop:v0.6.1
                container_name: specter-desktop
                command: /usr/local/bin/python3 -m cryptoadvance.specter server --host ip.addr
                restart: on-failure
                ports:
                    - "25441:25441"
                stop_grace_period: 5m30s
                network_mode: host                    
                volumes:
                        - ${PWD}/.bitcoin:/data/.bitcoin
                        - ${PWD}/.specter:/data/.specter
```
