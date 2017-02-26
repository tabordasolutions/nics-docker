# nics-docker

## Synopsis

Provides a [Docker Compose] configuration for running a Next-Generation Incident Command System (NICS) v6.x instance
suitable for development and testing.

To start

    % docker-compose -f build-docker-compose.yml build
    % docker-compose -f build-docker-compose.yml create
    % docker-compose -f build-docker-compose.yml start

To stop

    % docker-compose stop

Access NICS at [www.nicsdev.tabordasolutions.net/nics](https://www.nicsdev.tabordasolutions.net:3443/nics).
Access OpenAM at [am.nicsdev.tabordasolutions.net/openam](http://am.nicsdev.tabordasolutions.net:8080/openam).

## Description

When it works you should have something like this running on your [Docker] host

```
chris@pivot:nics-docker (master) % docker-compose ps
      Name                    Command               State                                             Ports                                            
------------------------------------------------------------------------------------------------------------------------------------------------------
nics_emapi_1       catalina.sh run                  Up      0.0.0.0:7083->8000/tcp, 0.0.0.0:8083->8080/tcp                                             
nics_geoserver_1   catalina.sh run                  Up      0.0.0.0:8084->8080/tcp                                                                     
nics_nicsweb_1     catalina.sh run                  Up      0.0.0.0:7082->8000/tcp, 0.0.0.0:8082->8080/tcp                                             
nics_openam_1      catalina.sh run                  Up      0.0.0.0:8000->8000/tcp, 0.0.0.0:8080->8080/tcp, 0.0.0.0:8443->8443/tcp                     
nics_postgis_1     /docker-entrypoint.sh postgres   Up      0.0.0.0:5432->5432/tcp                                                                     
nics_proxy_1       /docker-entrypoint.sh hapr ...   Up      0.0.0.0:3443->443/tcp, 0.0.0.0:3000->80/tcp                                                
nics_rabbitmq_1    docker-entrypoint.sh rabbi ...   Up      15671/tcp, 0.0.0.0:15672->15672/tcp, 25672/tcp, 4369/tcp, 5671/tcp, 0.0.0.0:5672->5672/tcp 
nics_web_1         /bin/sh -c /docker-entrypo ...   Up      443/tcp, 0.0.0.0:8081->80/tcp                                                              
```

Additionally, the Java Tomcat hosts will each be configured with Java Debug Wire Protocol agent running and a port
mapped to the [Docker] host for debugging. Reference table below for port numbers.

### Prerequisites

The following are required to run a NICS 6.x instance using [Docker Compose]. Where applicable, the last tested software
version number is included in parenthesis.

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

    127.0.0.1 nicsdev.tabordasolutions.net am.nicsdev.tabordasolutions.net www.nicsdev.tabordasolutions.net

Note three hostnames in use

1. `am.nicsdev.tabordasolutions.net` is the hostname used to access the OpenAM user interface
1. `www.nicsdev.tabordasolutions.net` is the hostname used to access the NICS web-based user interface
1. `nicsdev.tabordasolutions.net` is hostname used to test the redirect to the www host

### Running

Refer to [Docker Compose] documentation for complete details but, for basic usage, you will likely want to run

To start

    % docker-compose -f build-docker-compose.yml build
    % docker-compose -f build-docker-compose.yml create
    % docker-compose -f build-docker-compose.yml start

To stop

    % docker-compose stop

This will cause [Docker Compose] to pull or build images and deploy the system to [Docker].

Note that a running system is only the first step to a functional system. Additional configuration will be required
before the system will function as designed.

## Options

[Docker Compose] maps the following ports from the [Docker] host to the containers based on the `docker-compose.yml` 
file.

| Docker Port | Container Port | Purpose |
|---|---|---|
| 5432 | postgis:5432 | PostreSQL |
| 5672 | rabbitmq:5672 | RabbitMQ |
| 7082 | nicsweb:8000 | Java debug |
| 7083 | emapi:8000 | Java debug |
| 8000 | openam:8000 | Java debug |
| 3000 | proxy:80 | HTTP |
| 8081 | web:80 | HTTP |
| 8082 | nicsweb:8080 | HTTP |
| 8083 | emapi:8080 | HTTP |
| 8084 | geoserver:8080 | HTTP |
| 3443 | proxy:443 | HTTPS |
| 8080 | openam:8080 | HTTP |
| 8443 | openam:8443 | HTTPS |
| 15672 | rabbitmq:15672 | RabbitMQ admin |

## See Also

### Next Steps

Check out the [nics-common/docs] directory for additional NICS build and configuration details.

### Repositories

All repositories are available under the [Taborda Solutions GitHub organization].

[Taborda Solutions GitHub organization]: https://github.com/tabordasolutions
[Docker]: https://docs.docker.com/
[Docker Compose]: https://docs.docker.com/compose/
[nics-common/docs]: https://github.com/tabordasolutions/nics-common/tree/master/docs
