---
editor_options: 
  markdown: 
    wrap: 100
---

# Oracle cloud

## Create a virtual machine

## assign a fixed ip address

<https://www.youtube.com/watch?v=_9FWri0a9Bo>

## open ports

<https://www.youtube.com/watch?v=SO11Og_WgSs&t=262s>

internet gateway \> security list \> add Ingress rules 80, 443, 81 for npm

## install docker

This example downloads the script from get.docker.com and runs it to install the latest stable
release of Docker on Linux:

### Try this first (next time)

sudo snap install docker

sudo apt install docker-compose.

### use script to do full install

curl -fsSL <https://get.docker.com> -o get-docker.sh sudo sh get-docker.sh

### install prerequisite for rootless setup

sudo sh -eux \<\<EOF \# Install newuidmap & newgidmap binaries

apt-get install -y uidmap EOF

### install rootless script

dockerd-rootless-setuptool.sh install

Once done export variables:

export PATH=/usr/bin:\$PATH export DOCKER_HOST=unix:///run/user/1001/docker.sock

Create directory

mkdir -p ./apps/portainer

cd ./apps/portainer

touch docker-compose.yml

Portainer:

docker-compose.yml

```{bash}
version: "3"
services:
  portainer:
    container_name: portainer
    domainname: portainer.nicumanual.com
    hostname: portainer
    image: cr.portainer.io/portainer/portainer-ce:latest
    labels:
      com.centurylinklabs.watchtower.enable: true
    networks:
      - docker-network
    ports:
      - 9000:9000/tcp
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /volumes/portainer/portainer_data:/data
    working_dir: /
networks:
  docker-network:
    external: true

```
