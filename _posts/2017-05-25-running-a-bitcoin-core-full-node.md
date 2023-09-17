---
layout: post
published: true
title: Running a Bitcoin Core Full Node
date: 2017-05-25 21:08:00 -0700
comments: true
categories:
- linux
tags:
- ubuntu
- bitcoin
---

There has been a lot of hype concerning crypto currencies like Bitcoin and
Ethereum recently. I even had some of my own minor gains through an account I
have with [Coinbase.com](http://www.coinbase.com/).

I haven't been much into the zeitgeist of Bitcoin investment, or even the
possibilities of blockchain methods used for real-world applications other than
currency, until now.
<!--more-->

You will need a server that has at least 150 GB available, and as the size of
the blockchain increases this will rise. I configured my node to use the
auto-pruning feature, but it still is using 128 GB currently.

```shell
$ du -sh .bitcoin/
128G  .bitcoin/
```

## Install the software-properties-common Package

``` shell
sudo apt-get install software-properties-common
```

## Add the Bitcoin Personal Package Archive (PPA)

``` shell
sudo apt-add-repository ppa:bitcoin/bitcoin

 Stable Channel of bitcoin-qt and bitcoind for Ubuntu, and their dependencies

Note that you should prefer to use the official binaries, where possible, to
limit trust in Launchpad/the PPA owner.

No longer supports precise, due to its ancient gcc and Boost versions.
 More info: https://launchpad.net/~bitcoin/+archive/ubuntu/bitcoin
Press [ENTER] to continue or ctrl-c to cancel adding it
```

Ignore the message about 'precise' no longer being supported, and press ENTER
to continue. It's referring to Ubuntu 12.04.5 LTS (Precise Pangolin). You can
check your own version of Ubuntu by running `cat /etc/lsb-release`.

After this completes you should update all the packages.

``` shell
sudo apt-get update
```

## Install Bitcoin Core daemon (bitcoind)

``` shell
sudo apt-get install bitcoind
```

## Configuring Bitcoin Daemon

A problem that you will likely run into is where the daemon uses up all the disk
space on your server. I recommend creating a [bitcoin.conf] configuration file.
It's best to set the minimum value for pruning, and also set the db cache size
to be an appropriate amount of RAM in measured in megabytes.

```ini
# Enable pruning to reduce storage requirements by deleting old blocks.
# This mode is incompatible with -txindex and -rescan.
# 0 = default (no pruning).
# 1 = allows manual pruning via RPC.
# >=550 = target to stay under in MiB.
prune=550

# Set database cache size in megabytes (4 to 16384, default: 300)
dbcache=1000
```

[bitcoin.conf]: https://raw.githubusercontent.com/bitcoin/bitcoin/master/contrib/debian/examples/bitcoin.conf

## Run Bitcoin Core Daemon

Exit to an unprivileged user account, and then run `bitcoind -daemon`

```shell
bitcoind -daemon -conf=bitcoin.conf
Bitcoin server starting
```

After this point you can run various commands to interact with the daemon.

```shell
# Get Network Info
bitcoin-cli getnetworkinfo

# Get Blockchain Info
bitcoin-cli getblockchaininfo

# Get Wallet Info
bitcoin-cli getwalletinfo

# Stop the Node
bitcoin-cli stop
```

## Starting Bitcoin Daemon

It's cumbersome to type the command above each time you need to re-start the
daemon. By default Ubuntu configures your $PATH so that it includes `~/bin`,
should that directory exist. Do the following to prepare some scripts for easy
execution.

``` shell
mkdir ~/bin
touch ~/bin/btcstart
touch ~/bin/btclog
echo -e '#!/usr/bin/env bash\nbitcoind -daemon -conf=~/bitcoin.conf' > ~/bin/btcstart
echo -e '#!/usr/bin/env bash\ntail -F ~/.bitcoin/debug.log' > ~/bin/btclog
```

You'll have to logout and log in again, but now you will be able to start your
daemon using `btcstart` command.

```shell
$ btcstart
Bitcoin server starting

$ btclog
2017-05-26 15:59:19 Opened LevelDB successfully
2017-05-26 15:59:19 Using obfuscation key for /home/johnsmith/.bitcoin/blocks/index: 0000000000000000
...
...
```

You'll have to use CTRL+C to exit out of the logs.
