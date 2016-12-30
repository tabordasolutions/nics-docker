# nics-docker

## Synopsis

Provides a Docker Compose configuration for running a Next-Generation Incident Command System (NICS) v6.x instance
suitable for development and testing.

To start

    % docker-compose up --build

To stop press <ctrl>-c. Or

    % docker-compose stop

## Description

Include diagram here.
Describe debugging configs.

#### Prerequisites

1. Java 8 (1.8.0_91)
1. Maven (3.3.9)
1. Source code: all the NICS repositories, see below for required directory structure
1. Each of the library projects built and installed in the local Maven repo on the host that will be used for
 Docker Compose
1. Docker Compose (1.9.0)
1. Docker host with at least 10 GB available :ram: and 2 GB available disk space; Docker may be local to the machine
 used for Docker Compose or may be running on a remote host

The NICS repositories must be located in the same subdirectory, like this

```
    ~/codez
        - em-api
        - iweb-modules
        - nics-assembly
        - nics-common
        - nics-db
        - nics-docker
        - nics-openam
        - nics-proxy
        - nics-tools
        - nics-web
        - nics-www
```

This structure allows Docker Compose to build Docker images required to run the system.

#### Configuration

NICS requires a consistent canonical hostname for the system. OpenAM places further requirements on the hostname to the
 extent that the only effective options must contain a host, zero or more subdomains, a domain and a known top level
 domain (TLD). The simplest way to satisfy these requirements is to choose a valid domain to use for development and
 test and map the Docker server address to your Docker host using an /etc/hosts file. In the following example, the
 Docker server address is 10.90.223.202 and the domain used for development and testing is nics.coderhythm.io.

    10.90.223.202 nics.coderhythm.io am.nics.coderhythm.io www.nics.coderhythm.io

Note three hostnames in use

1. am.nics.coderhythm.io is the hostname used to access the OpenAM user interface
1. www.nics.coderhythm.io is the hostname used to access the NICS web-based user interface
1. nics.coderhythm.io is hostname used to test the redirect to the www host

## Options

Ports
Volumes

## See Also

### Repositories

| Repo | hadrsystems | tabordasolutions |
|---|---|---|
| em-api | https://github.com/hadrsystems/em-api | https://github.com/tabordasolutions/em-api |