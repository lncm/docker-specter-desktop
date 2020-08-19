# Specter Desktop in a docker container

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
docker run --rm -v $HOME:/.specter:/data/.specter lncm/specter-desktop:v0.6.1 --hwibridge

# Get the Help to see options
docker run --rm -v $HOME:/.specter:/data/.specter lncm/specter-desktop:v0.6.1 --help

```
