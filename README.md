# nics-docker

## Synopsis

Provides a [Docker Compose] configuration for running a Next-Generation Incident Command System (NICS) v6.x instance
suitable for development and testing.

To start

    % docker-compose up --build

To stop press `<ctrl>-c`. Or

    % docker-compose stop

Access NICS at [www.nicsdev.tabordasolutions.net/nics](https://www.nicsdev.tabordasolutions.net/nics).
Access OpenAM at [www.nicsdev.tabordasolutions.net/openam](https://www.nicsdev.tabordasolutions.net/openam).

## Description

Include diagram here.
Describe debugging configs.

### Prerequisites

1. Java 8 (1.8.0_91)
1. Maven (3.3.9)
1. Source code: all the NICS repositories, see below for required directory structure
1. Each of the library projects built and installed in the local Maven repo on the host that will be used for
 [Docker Compose]
1. Each of the web application projects built and packaged on the local filesystem
1. OpenAM web application and policy agent downloaded and in correct place in `nics-openam` and `nics-www` projects
1. Apache configuration copied and updated with correct hostnames in `nics-www` project
1. NICS-Web application configured as per instructions in `nics-web`, with files in config directory
1. EM-API application configured as per instructions in `nics-emapi` project, with files in config and tomcat-config
 directory
1. [Docker Compose] (1.9.0)
1. [Docker] host with at least 10 GB available :ram: and 2 GB available disk space; [Docker] may be local to the machine
 used for [Docker Compose] or may be running on a remote host

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

This structure allows [Docker Compose] to build [Docker] images required to run the system.

### Configuration

NICS requires a consistent canonical hostname for the system. OpenAM places further requirements on the hostname to the
 extent that the only effective options must contain a host, zero or more subdomains, a domain and a known top level
 domain (TLD). The simplest way to satisfy these requirements is to choose a valid domain to use for development and
 test and map the [Docker] server address to your [Docker] host using an /etc/hosts file. In the following example, the
 [Docker] server address is 10.90.223.202 and the domain used for development and testing is
 `nicsdev.tabordasolutions.net`.

    10.90.223.202 nicsdev.tabordasolutions.net am.nicsdev.tabordasolutions.net www.nicsdev.tabordasolutions.net

Note three hostnames in use

1. `am.nicsdev.tabordasolutions.net` is the hostname used to access the OpenAM user interface
1. `www.nicsdev.tabordasolutions.net` is the hostname used to access the NICS web-based user interface
1. `nicsdev.tabordasolutions.net` is hostname used to test the redirect to the www host

### Running

Refer to [Docker Compose] documentation for complete details but, for basic usage, you will likely want to run

    % docker-compose up --build

This will cause [Docker Compose] to pull or build images and deploy the system to [Docker]. You will see aggreggated
logs in the terminal window running [Docker Compose]. When you're done testing, pressing `<ctrl>-c` will gracefully
shut down the system.

Note that a running system is only the first step to a functional system. Additional configuration will be required
before the system will function as designed.

## Options

[Docker Compose] maps the following ports from the [Docker] host to the containers based on the `docker-compose.yml` 
file. You may edit these mappings if needed.

| Docker Port | Container Port | Purpose |
|---|---|---|
| 5432 | postgis:5432 | PostreSQL |
| 5672 | rabbitmq:5672 | RabbitMQ |
| 7082 | nicsweb:8000 | Java debug |
| 7083 | emapi:8000 | Java debug |
| 8000 | openam:8000 | Java debug |
| 8080 | proxy:80 | HTTP |
| 8081 | web:80 | HTTP |
| 8082 | nicsweb:8080 | HTTP |
| 8083 | emapi:8080 | HTTP |
| 8084 | geoserver:8080 | HTTP |
| 8443 | proxy:443 | HTTPS |
| 9000 | openam:8080 | HTTP |
| 9443 | openam:8443 | HTTPS |
| 15672 | rabbitmq:15672 | RabbitMQ admin |

[Docker Compose] maps several volumes to the [Docker] host filesystem. These volumes will be stored in 
`/var/lib/docker/volumes` but may be changed in the `docker-compose.yml` file.

## See Also

### Repositories

| Repo | MIT Location | Taborda Solutions Fork |
|---|---|---|
| em-api | https://github.com/hadrsystems/em-api | https://github.com/tabordasolutions/em-api |
| iweb-modules | https://github.com/hadrsystems/iweb-modules | |
| nics-assembly | https://github.com/hadrsystems/nics-assembly | |
| nics-common | https://github.com/hadrsystems/nics-common | |
| nics-db | https://github.com/hadrsystems/nics-db | https://github.com/tabordasolutions/nics-db |
| nics-docker | | https://github.com/tabordasolutions/nics-docker |
| nics-openam | | https://github.com/tabordasolutions/nics-openam |
| nics-proxy | | https://github.com/tabordasolutions/nics-proxy |
| nics-tools | | https://github.com/tabordasolutions/nics-tools |
| nics-web | https://github.com/hadrsystems/nics-web | https://github.com/tabordasolutions/nics-web |
| nics-www | | https://github.com/tabordasolutions/nics-www |

Prefer the Taborda Solutions fork over the MIT repo and the docker branch over master branch until [Docker] support is
merged to master.

[Docker]: https://docs.docker.com/
[Docker Compose]: https://docs.docker.com/compose/
