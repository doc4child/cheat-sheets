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

# **Post-installation steps for Linux**

*Estimated reading time: 15 minutes*

This section contains optional procedures for configuring Linux hosts to work better with Docker.

## **Manage Docker as a non-root user**

The Docker daemon binds to a Unix socket instead of a TCP port. By default that Unix socket is owned
by the user `root` and other users can only access it using `sudo`. The Docker daemon always runs as
the `root` user.

If you don't want to preface the `docker` command with `sudo`, create a Unix group called `docker`
and add users to it. When the Docker daemon starts, it creates a Unix socket accessible by members
of the `docker` group.

> **Warning**
>
> The `docker` group grants privileges equivalent to the `root` user. For details on how this
> impacts security in your system, see [*Docker Daemon Attack
> Surface*](https://docs.docker.com/engine/security/#docker-daemon-attack-surface).

To create the `docker` group and add your user:

1.  Create the `docker` group.

        $ sudo groupadd docker

2.  Add your user to the `docker` group.

        $ sudo usermod -aG docker $USER

3.  Log out and log back in so that your group membership is re-evaluated.

    If testing on a virtual machine, it may be necessary to restart the virtual machine for changes
    to take effect.

    On a desktop Linux environment such as X Windows, log out of your session completely and then
    log back in.

    On Linux, you can also run the following command to activate the changes to groups:

        $ newgrp docker 

4.  Verify that you can run `docker` commands without `sudo`.

        $ docker run hello-world

    This command downloads a test image and runs it in a container. When the container runs, it
    prints a message and exits.

    If you initially ran Docker CLI commands using `sudo` before adding your user to the `docker`
    group, you may see the following error, which indicates that your `~/.docker/` directory was
    created with incorrect permissions due to the `sudo` commands.

        WARNING: Error loading config file: /home/user/.docker/config.json -
        stat /home/user/.docker/config.json: permission denied

    To fix this problem, either remove the `~/.docker/` directory (it is recreated automatically,
    but any custom settings are lost), or change its ownership and permissions using the following
    commands:

        $ sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
        $ sudo chmod g+rwx "$HOME/.docker" -R

## **Configure Docker to start on boot**

Most current Linux distributions (RHEL, CentOS, Fedora, Debian, Ubuntu 16.04 and higher) use
[`systemd`](https://docs.docker.com/config/daemon/systemd/) to manage which services start when the
system boots. On Debian and Ubuntu, the Docker service is configured to start on boot by default. To
automatically start Docker and Containerd on boot for other distros, use the commands below:

    $ sudo systemctl enable docker.service
    $ sudo systemctl enable containerd.service

To disable this behavior, use `disable` instead.

    $ sudo systemctl disable docker.service
    $ sudo systemctl disable containerd.service

If you need to add an HTTP Proxy, set a different directory or partition for the Docker runtime
files, or make other customizations, see [customize your systemd Docker daemon
options](https://docs.docker.com/config/daemon/systemd/).

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

mkdir -p \~/apps/portainer

cd \~/apps/portainer

touch docker-compose.yml

Portainer:

sudo nano docker-compose.yml

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
      - ~/apps/portainer/portainer_data:/data
    working_dir: /
networks:
  docker-network:
    external: true

```

docker network create docker-network

docker-compose up -d
