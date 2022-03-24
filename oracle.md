

# Oracle cloud

## Create a virtual machine

use Ubuntu image use id_rsa.pub key select fixed ip as ip address
<https://www.youtube.com/watch?v=_9FWri0a9Bo>

### change password for ubuntu
sudo passwd ubuntu

## install docker <https://docs.docker.com/engine/install/ubuntu/>

### Install using the repository

Before you install Docker Engine for the first time on a new host machine, you need to set up the
Docker repository. Afterward, you can install and update Docker from the repository.

Set up the repository Update the apt package index and install packages to allow apt to use a
repository over HTTPS:

sudo apt-get update sudo apt-get install\
ca-certificates\
curl\
gnupg\
lsb-release Add Docker's official GPG key:

curl -fsSL <https://download.docker.com/linux/ubuntu/gpg> \| sudo gpg --dearmor -o
/usr/share/keyrings/docker-archive-keyring.gpg Use the following command to set up the stable
repository. To add the nightly or test repository, add the word nightly or test (or both) after the
word stable in the commands below. Learn about nightly and test channels.

echo\
"deb [arch=\$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg]
<https://download.docker.com/linux/ubuntu>\
\$(lsb_release -cs) stable" \| sudo tee /etc/apt/sources.list.d/docker.list \> /dev/null Install
Docker Engine Update the apt package index, and install the latest version of Docker Engine and
containerd, or go to the next step to install a specific version:

sudo apt-get update sudo apt-get install docker-ce docker-ce-cli containerd.io

Verify that Docker Engine is installed correctly by running the hello-world image.

sudo docker run hello-world

# **Post-installation steps for Linux**

To create the `docker` group and add your user:

1.  Create the `docker` group.

        $ sudo groupadd docker

2.  Add your user to the `docker` group.

        $ sudo usermod -aG docker $USER

3.  Log out and log back in so that your group membership is re-evaluated.

    If testing on a virtual machine, it may be necessary to restart the virtual machine for changes
    to take effect.

    On Linux, you can also run the following command to activate the changes to groups:

        $ newgrp docker 

4.  Verify that you can run `docker` commands without `sudo`.

        $ docker run hello-world

Create directory

mkdir -p /volumes/portainer

cd /

check which user and group owns volumes directory ls -al

if it is root root then change to ubuntu:docker by

sudo chown -R ubuntu:docker /volumes

ls -al check it again

it should show ubuntu user and docker group permissions.

Now create network

docker network create docker-network

docker run -d -p 8000:8000 -p 9000:9000 -p 9443:9443 --domainname portainer.nicumanual.com --network
docker-network --hostname portainer --name=portainer --restart=always
--label=com.centurylinklabs.watchtower.enable=true -v /var/run/docker.sock:/var/run/docker.sock -v
/volumes/portainer/portainer\_<data:/data> cr.portainer.io/portainer/portainer-ce:latest

# To access this you need to setup fixed public IP and ingress on port 9000, 81 temporarity and

## assign a fixed ip address

<https://www.youtube.com/watch?v=_9FWri0a9Bo>

## open ports

<https://www.youtube.com/watch?v=SO11Og_WgSs&t=262s>

internet gateway \> security list \> add Ingress rules 80, 443, 81 for npm, 9000 for portainer

Access portainer by fixed_ip_address:9000

install npm

Access npm by fixed_ip_address:81

setup npm to access npm and portainer by url instead of Ip

Note the destination hostnames

Once these are setup properly may delete ingress rules for port 9000 and 81

# other containers need to be accessed using

hostname:80 (inside the container port not the hostport)

# not sure if following will work

# touch docker-compose.yml

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

# 
