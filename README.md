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

## Running

```bash
# in HWI bridge mode
docker run --rm -v $HOME/.specter:/data/.specter lncm/specter-desktop:v0.6.1 --hwibridge

# Get the Help to see options
docker run --rm -v $HOME/.specter:/data/.specter lncm/specter-desktop:v0.6.1 --help

```

