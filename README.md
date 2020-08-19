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

