# Avlan

Avlan is a VLAN monitoring and setup tool.

## Quickstart
Download pre-build images:
[avlan.tar.xz](https://github.com/astachecki/avlan-docker/releases/download/0.1/avlan.tar.xz)
[avlan-mysql.txz](https://github.com/astachecki/avlan-docker/releases/download/0.1/avlan-mysql.txz)
[avlan-redis.txz](https://github.com/astachecki/avlan-docker/releases/download/0.1/avlan-redis.txz)
[avlan-nginx.txz](https://github.com/astachecki/avlan-docker/releases/download/0.1/avlan-nginx.txz)

Load pre-build images:
```console
$ docker load -i avlan.tar.xz
$ docker load -i avlan-mysql.txz
$ docker load -i avlan-redis.txz
$ docker load -i avlan-nginx.txz
```

Start cluster:
```console
$ docker-compose up
```

## Installation
As this project is intended to be self-contained, please build, download (3rdparty images) or import following images prior to first execution:

**Hint: it is required to extract avlan application repository and netmiko module under avlan/config/resources directory**
```console
avlan
└── config
    └── resources
        ├── avlan
        └── netmiko
```

```console
$ docker-compose build
```

### Permanent installation (Linux only)
  - Place docker-compose.yml in an installation directory (reffered later on as INSTALL_DIR):
```console
$ mkdir -p INSTALL_DIR
$ cp docker-compose.yml INSTALL_DIR/
```
  - Configure and install service environment file pointing to execution directory:
```
$ sed -e "s#WORKING_DIRECTORY=#WORKING_DIRECTORY=INSTALL_DIR#g" -i systemd/avlan.conf
$ cp systemd/avlan.conf /etc/sysconfig/ 
```
  - Install service files: 
```console
$ cp systemd/docker-*.service /etc/systemd/system/
$ systemctl enable docker-avlan.service
$ systemctl enable docker-mysql.service
$ systemctl enable docker-redis.service
$ systemctl enable docker-nginx.service
```
Final layout should look like this:
```console
INSTALL_DIR
├── avlan
├── docker-compose.yml
├── mysql
├── redis
└── nginx-proxy
```

## Usage

### Testing 
Run using docker-compose:

* start the avlan container + its dependencies (mysql database, redis database, nginx-proxy)
```console
$ docker-compose up
```

* stop the avlan container + its dependencies
```console
$ docker-compose stop
```

* remove stopped containers
```console
$ docker-compose rm -v
```

* stop containers and removes containers, networks, volumes, and images created by ```up```. 
```console
$ docker-compose down --rmi all
```

## Production (Linux only)
Run using systemd service:
```console
$ systemctl start docker-nginx-proxy.service
``` 
