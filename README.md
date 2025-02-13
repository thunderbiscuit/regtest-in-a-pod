# Readme
This repo is a companion to the [Using Podman Containers for Regtest Bitcoin Development](https://thunderbiscuit.com/posts/podman-bitcoin/) blog article.

It allows you to create a robust regtest environment which you can turn on and off at will using Podman. The final environment includes a lot of useful tools, namely:
1. A bitcoin core node and daemon (serving compact block filters)
2. bitcoin-cli enabled
3. An Electrum server
4. An Esplora server
5. A block explorer
6. Useful aliases and just commands for working with the daemon from your command line

Read the article linked above for all the information on how to use the container to its fullest, but here is a quick cheatsheet:

## Usage
You can use the aliases defined in the `aliases.sh` file directly, but even better is to use the [`just`](https://github.com/casey/just) tool and leverage the commands defined in the `justfile`.

```shell
just mine 21
just sendto <address>
just bitcoindlogs
just esploralogs
```

## Building, Starting, and Stopping the Pod
```shell
# Building the container
cd ~/podman/regtest-in-a-pod/
podman machine start regtest
# Set BITCOIN_VERSION and TARGET_ARCH to use the bitcoin-core version you need
podman --connection regtest build --build-arg BITCOIN_VERSION=28.1 --build-arg TARGET_ARCH=x86_64-linux-gnu --tag localhost/regtest:v1.0.0 --file ./Containerfile
podman create --name RegtestBitcoinEnv --publish 18443:18443 --publish 18444:18444 --publish 3002:3002 --publish 3003:3003 --publish 60401:60401 localhost/regtest:v1.1.0
podman start RegtestBitcoinEnv
source aliases.sh
# Using the container
cd ~/podman/regtest-in-a-pod/
podman machine start regtest
podman start RegtestBitcoinEnv
source aliases.sh
# podmine
# podcli
# explorer

# Stopping the container and the machine
podman stop RegtestBitcoinEnv
podman machine stop regtest
```
