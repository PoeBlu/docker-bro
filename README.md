![bro-logo](https://raw.githubusercontent.com/blacktop/docker-bro/master/logo.png)

Bro IDS Dockerfile
==================

[![CircleCI](https://circleci.com/gh/blacktop/docker-bro.png?style=shield)](https://circleci.com/gh/blacktop/docker-bro) [![License](http://img.shields.io/:license-mit-blue.svg)](http://doge.mit-license.org) [![Docker Stars](https://img.shields.io/docker/stars/blacktop/bro.svg)](https://hub.docker.com/r/blacktop/bro/) [![Docker Pulls](https://img.shields.io/docker/pulls/blacktop/bro.svg)](https://hub.docker.com/r/blacktop/bro/) [![Docker Image](https://img.shields.io/badge/docker image-157.3 MB-blue.svg)](https://hub.docker.com/r/blacktop/bro/)

This repository contains a **Dockerfile** of [Bro-IDS](http://www.bro.org/index.html) **blacktop/bro**.

**Table of Contents**

- [Bro IDS Dockerfile](#bro-ids-dockerfile)
    - [Dependencies](#dependencies)
    - [Image Tags](#image-tags)
    - [Installation](#installation)
    - [Getting Started](#getting-started)
      - [Using the included heartbleed test pcap](#using-the-included-heartbleed-test-pcap)
    - [Documentation](#documentation)
      - [Usage](#usage)
        - [Capture Live Traffic](#capture-live-traffic)
        - [Use your own pcap](#use-your-own-pcap)
      - [Tips and Tricks](#tips-and-tricks)
        - [Create a pcap](#create-a-pcap)
        - [Get rid of the `WARNING: No Site::local_nets have been defined.` message.](#get-rid-of-the-warning-no-sitelocal_nets-have-been-defined-message)
        - [Use **blacktop/bro** like a host binary](#use-blacktopbro-like-a-host-binary)
    - [Issues](#issues)
    - [Todo](#todo)
    - [CHANGELOG](#changelog)
    - [Contributing](#contributing)
    - [License](#license)

### Dependencies

-	[debian:wheezy (*84.93 MB*\)](https://hub.docker.com/_/debian/)

### Image Tags

```bash
$ docker images

REPOSITORY          TAG                 SIZE
blacktop/bro        latest              157.3 MB
blacktop/bro        2.5-beta            202.2 MB
blacktop/bro        2.4                 157.3 MB
blacktop/bro        elastic             640 MB
blacktop/bro        2.4.1               482.8 MB
blacktop/bro        2.3.2               530.9 MB
blacktop/bro        2.2                 527.7 MB
```

### Installation

1.	Install [Docker](https://docs.docker.com).
2.	Download [trusted build](https://hub.docker.com/r/blacktop/bro/) from public [Docker Registry](https://hub.docker.com): `docker pull blacktop/bro`

### Getting Started

#### Using the included heartbleed test pcap

```bash
$ docker run --rm -v `pwd`/pcap:/pcap blacktop/bro -r heartbleed.pcap local "Site::local_nets += { 192.168.11.0/24 }"
```

```bash
$ ls -l

-rw-r--r--  1 blactop  staff   635B Jul 30 12:11 pcap/conn.log
-rw-r--r--  1 blactop  staff   754B Jul 30 12:11 pcap/files.log
-rw-r--r--  1 blactop  staff   384B Jul 30 12:11 pcap/known_certs.log
-rw-r--r--  1 blactop  staff   239B Jul 30 12:11 pcap/known_hosts.log
-rw-r--r--  1 blactop  staff   271B Jul 30 12:11 pcap/known_services.log
-rw-r--r--  1 blactop  staff    17K Jul 30 12:11 pcap/loaded_scripts.log
-rw-r--r--  1 blactop  staff   1.9K Jul 30 12:11 'pcap/notice.log'
-rw-r--r--  1 blactop  staff   253B Jul 30 12:11 pcap/packet_filter.log
-rw-r--r--  1 blactop  staff   1.2K Jul 30 12:11 pcap/ssl.log
-rw-r--r--  1 blactop  staff   901B Jul 30 12:11 pcap/x509.log
```

```bash
$ cat pcap/notice.log | awk '{ print $11 }' | tail -n4

Heartbleed::SSL_Heartbeat_Attack
Heartbleed::SSL_Heartbeat_Odd_Length
Heartbleed::SSL_Heartbeat_Attack_Success
```

### Documentation

#### Usage

##### Capture Live Traffic

```bash
docker run --rm --cap-add=NET_RAW --net=host -v `pwd`:/pcap:rw blacktop/bro -i eth0
```

##### Use your own pcap

```bash
$ docker run --rm -v /path/to/pcap:/pcap:rw blacktop/bro -r my.pcap local
```

#### Tips and Tricks

##### Create a pcap

Capturing packets from an interface and writing them to a file can be done like this:

```bash
$ sudo tcpdump -i en0 -s 0 -w my_capture.pcap
```

To capture packets from a VMWare Fusion VM using **vmnet-sniffer** you can do this:

```bash
sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-sniffer -e -w my_capture.pcap vmnet8
```

##### Get rid of the `WARNING: No Site::local_nets have been defined.` message.

```bash
bro -r my_capture.pcap local "Site::local_nets += { 1.2.3.0/24, 5.6.7.0/24 }"
```

##### Use **blacktop/bro** like a host binary

Add the following to your bash or zsh profile

```bash
alias bro='docker run --rm -v `pwd`:/pcap:rw blacktop/bro $@'
```

### Issues

Find a bug? Want more features? Find something missing in the documentation? Let me know! Please don't hesitate to [file an issue](https://github.com/blacktop/docker-bro/issues/new) and I'll get right on it.

> NOTE: I am now using the precompiled bro package to decrease the docker image size, if that caused a loss in functionality you depend on please let me know.

### Todo

-	[x] Install/Run Bro-IDS
-	[x] Fix Geolocation feature
-	[x] Refine my extract-all.bro script
-	[ ] Start Daemon and watch folder with supervisord
-	[ ] Have container take a URL as input and download/scan pcap
-	[ ] Add ELK Stack

### CHANGELOG

See [`CHANGELOG.md`](https://github.com/blacktop/docker-bro/blob/master/CHANGELOG.md)

### Contributing

[See all contributors on GitHub](https://github.com/blacktop/docker-bro/graphs/contributors).

Please update the [CHANGELOG.md](https://github.com/blacktop/docker-bro/blob/master/CHANGELOG.md) and submit a [Pull Request on GitHub](https://help.github.com/articles/using-pull-requests/).

### License

MIT Copyright (c) 2015-2016 **blacktop**
